---
categories:
- CTF
layout: post
media_subpath: /assets/posts/2025-02-01-HTB-Academy-Bug-Bounty-Hunter
tags:
- HTBAcademy
title: HTB Academy - Bug Bounty Hunter Path
---
# Web Requests Module


### cURL
*Use cURL to download from target system and obtain the flag*

***Question:***

To get the flag, start the above exercise, then use cURL to download the file returned by '/download.php' in the server shown below:

![alt text](image.png)

### HTTP/S Requests and Responses

>"HTTP is an application-level protocol used to access the World Wide Web resources. The term hypertext stands for text containing links to other resources and text that the readers can easily interpret."
>
>"HTTPS (HTTP Secure) protocol was created, in which all communications are transferred in an encrypted format, so even if a third party does intercept the request, they would not be able to extract the data out of it." 
(HTBAcademy)

*Good example of the HTTP attack vector would be, where traffic is not encrypted and the attacker could read the trafficflow in plaintext, just joining into the same network if the target is not using any securing measures for example Virtual Private Network (VPN).* 

*HTTPS requires SSL-certificate for the encryption therefore attacker would need to install a Man-in-the-middle (MiTM) certificate into the target machine. This could be achieved also with social engineering but requires much more effort.*



***Questions:*** 

What is the HTTP method used while intercepting the request? (case-sensitive) : GET

![webdev](2025-02-08-10-40-49.png)



Send a GET request to the above server, and read the response headers to find the version of Apache running on the server, then submit it as the answer. (answer format: X.Y.ZZ): 2.4.41

![apa](2025-02-08-10-39-01.png)

### HTTP Headers
*Headers will transfer information between the client and the server. Different headers are for example: General, Request, Response, Security etc. This differs, when there is a specific HTTP-method used.*

***Question:***

The server above loads the flag after the page is loaded. Use the Network tab in the browser devtools to see what requests are made by the page, and find the request to the flag. 



![ffa](2025-02-10-35.png)

*After requesting the target website and opening the network tab in browser's developers tools, I was able to catch the flag from the response.*

![flagg](2025-02-11-15-29-52.png)

### HTTP Methods

  *HTTP supports different kind of methods for interacting the server with browser. Most common ones are GET, which will receive data and POST, which will send data, for example forms.*

***Questions***

 The exercise above seems to be broken, as it returns incorrect results. Use the browser devtools to see what is the request it is sending when we search, and use cURL to search for 'flag' and obtain the flag. 

 ![](2025-02-13-54.png)

 *Developers tools shows "please use cURL" in the network section, lets do that. We need also the authorization token to retrieve any information*

 ![](2025-02-13-04.png)


Obtain a session cookie through a valid login, and then use the cookie with cURL to search for the flag through a JSON POST request to '/search.php' 

![](2025-02-14-35.png)

```bash
curl -X POST -d '{"search":"flag"}' -b 'PHPSESSID=ig0kpqkqtot858cj9vflhimcde' -H 'Content-Type: application/json'  http://hackthebox:port/search.php -i

"flag: HTB{p0$t_r3p34t3r}"
```

*First using curl to authenticate myself into the website via credentials "adminadmin" and then gaining the PHP-session id, therefore using that specific id to request "search.php" and finding flag.*

*Note: I like this academy, fast paced, little achievements and good knownledge.*

### CRUD API

*With Crud API, we can perform different actions towards the server. Each operation corresponds to HTTP Method, for example "Create" is "POST" and "READ" is "GET" and so on. These actions will be executed in the servers database.*

***Question***

First, try to update any city's name to be 'flag'. Then, delete any city. Once done, search for a city named 'flag' to get the flag. 

![](2025-02-14-12-11-21.png)

*Using PUT-method to update Memphis into "flag:HTB" and then deleting Baltimore. Searching for city named "flag".*

![](2025-02-14-12-13-10.png)

*First module done, then to next one*

### Introduction to Web Applications


> Interactive applications, which typically have frontend and backend as components. Different from static websites, these applications provide different view for each user, therefore its dynamic. Applications can run on any browser without any dependency of OS etc. Popular opensource applications are for example Wordpress and Joomla. These applications have big attack surface therefore its really important to make these secure as possible. Common vulnerabilities are SQL Injection, File Inclusion, Unrestricted File Uploads or Broken Access Control. (HTBAcademy)


