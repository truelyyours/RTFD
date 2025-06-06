## Back End Server
A back-end server is the hardware and operating system on the back end that hosts all of the applications necessary to run the web application. The back end server would fit in the [Data access layer](https://en.wikipedia.org/wiki/Data_access_layer).

### Software
The back end server contains the other 3 back end components:

- `Web Server`
- `Database`
- `Development Framework`
![[backend-server 1.jpg]]

There are many popular combinations of "stacks" for back-end servers, which contain a specific set of back end components. Some common examples include:

|Combinations|Components|
|---|---|
|[LAMP](https://en.wikipedia.org/wiki/LAMP_\(software_bundle\))|`Linux`, `Apache`, `MySQL`, and `PHP`.|
|[WAMP](https://en.wikipedia.org/wiki/LAMP_\(software_bundle\)#WAMP)|`Windows`, `Apache`, `MySQL`, and `PHP`.|
|[WINS](https://en.wikipedia.org/wiki/Solution_stack)|`Windows`, `IIS`, `.NET`, and `SQL Server`|
|[MAMP](https://en.wikipedia.org/wiki/MAMP)|`macOS`, `Apache`, `MySQL`, and `PHP`.|
|[XAMPP](https://en.wikipedia.org/wiki/XAMPP)|Cross-Platform, `Apache`, `MySQL`, and `PHP/PERL`.|
## Web Servers
A [web server](https://en.wikipedia.org/wiki/Web_server) is an application that runs on the back end server, which handles all of the HTTP traffic from the client-side browser, routes it to the requested pages, and finally responds to the client-side browser.
Web servers usually run on TCP [ports](https://en.wikipedia.org/wiki/Port_\(computer_networking\)) `80` or `443`, and are responsible for connecting end-users to various parts of the web application, in addition to handling their various responses.
### Workflow
A typical web server accepts HTTP requests from the client-side, and responds with different HTTP responses and codes, like a code `200 OK` response for a successful request, a code `404 NOT FOUND` when requesting pages that do not exist, code `403 FORBIDDEN` for requesting access to restricted pages, and so on.

A typical web server accepts HTTP requests from the client-side, and responds with different HTTP responses and codes, like a code `200 OK` response for a successful request, a code `404 NOT FOUND` when requesting pages that do not exist, code `403 FORBIDDEN` for requesting access to restricted pages, and so on.

![[web-server-requests.jpg]]
The following are some of the most common [HTTP response codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status):

|Code|Description|
|---|---|
|**Successful responses**||
|`200 OK`|The request has succeeded|
|**Redirection messages**||
|`301 Moved Permanently`|The URL of the requested resource has been changed permanently|
|`302 Found`|The URL of the requested resource has been changed temporarily|
|**Client error responses**||
|`400 Bad Request`|The server could not understand the request due to invalid syntax|
|`401 Unauthorized`|Unauthenticated attempt to access page|
|`403 Forbidden`|The client does not have access rights to the content|
|`404 Not Found`|The server can not find the requested resource|
|`405 Method Not Allowed`|The request method is known by the server but has been disabled and cannot be used|
|`408 Request Timeout`|This response is sent on an idle connection by some servers, even without any previous request by the client|
|**Server error responses**||
|`500 Internal Server Error`|The server has encountered a situation it doesn't know how to handle|
|`502 Bad Gateway`|The server, while working as a gateway to get a response needed to handle the request, received an invalid response|
|`504 Gateway Timeout`|The server is acting as a gateway and cannot get a response in time|
### Apache
[Apache](https://www.apache.org/) 'or `httpd`' is the most common web server on the internet, hosting more than `40%` of all internet websites. `Apache` usually comes pre-installed in most `Linux` distributions and can also be installed on Windows and macOS servers.
### NGINX
[NGINX](https://www.nginx.com/) is the second most common web server on the internet, hosting roughly `30%` of all internet websites. `NGINX` focuses on serving many concurrent web requests with relatively low memory and CPU load by utilizing an async architecture to do so. This makes `NGINX` a very reliable web server for popular web applications and top businesses worldwide, which is why it is the most popular web server among high traffic websites, with around 60% of the top 100,000 websites using `NGINX`.
### IIS (Microsoft)
[IIS (Internet Information Services)](https://en.wikipedia.org/wiki/Internet_Information_Services) is the third most common web server on the internet, hosting around `15%` of all internet web sites. `IIS` is developed and maintained by Microsoft and mainly runs on Microsoft Windows Servers. `IIS` is usually used to host web applications developed for the Microsoft .NET framework, but can also be used to host web applications developed in other languages like `PHP`, or host other types of services like `FTP`.
[IIS (Internet Information Services)](https://en.wikipedia.org/wiki/Internet_Information_Services) is the third most common web server on the internet, hosting around `15%` of all internet web sites. `IIS` is developed and maintained by Microsoft and mainly runs on Microsoft Windows Servers. `IIS` is usually used to host web applications developed for the Microsoft .NET framework, but can also be used to host web applications developed in other languages like `PHP`, or host other types of services like `FTP`.
## Databases
