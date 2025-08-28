Because this is still one of my initial boxes, I am going to do this in guided mode (It's available for retired machines).
Starting with `nmap`:
```
# Nmap 7.94SVN scan initiated Thu Aug 28 17:03:04 2025 as: nmap -sC -sV -oA nmap/twomillion 10.10.11.221
Nmap scan report for 10.10.11.221
Host is up (0.11s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx
|_http-title: Did not follow redirect to http://2million.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Aug 28 17:03:15 2025 -- 1 IP address (1 host up) scanned in 11.83 seconds
```

So, we have 2 TCP ports open. As seen, we have `2million.htb` on port 80, so I put this in `/etc/hosts` and open the site in browser.
This is the classic HTB site. Back then we had to find an invite code to join the HTB. I remember I prolly got the code from somewhere else during my college days. But let's do this properly this time!
![[Pasted image 20250828181708.png]]

Upon clicking "Join HTB", we are taken to the page `/invite`:
![[Pasted image 20250828181750.png]]

The `/invite` page loads script from `inviteapi.min.js`. We can beautify the javascript and get to know the function:
```javascript
function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function(response) {
            console.log(response)
        },
        error: function(response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function(response) {
            console.log(response)
        },
        error: function(response) {
            console.log(response)
        }
    })
}
```

So, we have a `makeInviteCode` function that can be helpful! It sends a POST request. Let's send this POST request via curl from terminal.
```
(venv) ┌─[htb_lab_truelyyours]─[10.10.16.82]─[truelyyours@parrot]─[~/htb]
└──╼ [★]$ curl -sq -X POST 2million.htb/api/v1/invite/how/to/generate | jq .
{
  "0": 200,
  "success": 1,
  "data": {
    "data": "Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/i1/vaivgr/trarengr",
    "enctype": "ROT13"
  },
  "hint": "Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."
}
```
So we are given a ROT 13 encrypted data. This is decrypted to:
```
In order to generate the invite code, make a POST request to /api/v1/invite/generate
```

Welp, let's send another POST request via curl:
```
{
  "0": 200,
  "success": 1,
  "data": {
    "code": "VDdRMUctUDVHQUYtQURHR0wtR08xVjM=",
    "format": "encoded"
  }
}
```
This base 64 can be decoded to our invite code and voila! We have our HTB account!
```
┌─[htb_lab_truelyyours]─[10.10.16.82]─[truelyyours@parrot]─[~/htb/twomillion]
└──╼ [★]$ curl -sq -X POST 2million.htb/api/v1/invite/generate | jq .data.code -r |base64 -d; echo
1AJW5-GU5EJ-T3Z5O-ZN96C
```

We login on the lading page:
![[Pasted image 20250828183645.png]]

The next Task (6) asks us to find endpoints. So, I send this to BurpSuite and explore endpoints!
On the `/home` page (Dashboard) there many option on the right. So, I explore those and the only new links are `/rules`, `/changelog` and `/home/access`. From the "Access" part, we can download VPN files. So, I intercept this request via BurpSuite and download a VPN file.

When checking different API endpoints, `/api/v1` gives us the list of all the APIs present. (Note that you dont have to do this manually. You can use any of the other tools to traverse different API paths!)
```json
{
  "v1": {
    "user": {
      "GET": {
        "/api/v1": "Route List",
        "/api/v1/invite/how/to/generate": "Instructions on invite code generation",
        "/api/v1/invite/generate": "Generate invite code",
        "/api/v1/invite/verify": "Verify invite code",
        "/api/v1/user/auth": "Check if user is authenticated",
        "/api/v1/user/vpn/generate": "Generate a new VPN configuration",
        "/api/v1/user/vpn/regenerate": "Regenerate VPN configuration",
        "/api/v1/user/vpn/download": "Download OVPN file"
      },
      "POST": {
        "/api/v1/user/register": "Register a new user",
        "/api/v1/user/login": "Login with existing user"
      }
    },
    "admin": {
      "GET": {
        "/api/v1/admin/auth": "Check if user is admin"
      },
      "POST": {
        "/api/v1/admin/vpn/generate": "Generate VPN for specific user"
      },
      "PUT": {
        "/api/v1/admin/settings/update": "Update user settings"
      }
    }
  }
}
```

We have 3 "admin" APIs available too! 
