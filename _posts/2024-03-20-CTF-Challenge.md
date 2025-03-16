---
categories:
- Schoolwork
layout: post
image:
  path: hacking.png
media_subpath: /assets/posts/2024-03-20-CTF-Challenge
tags:
- Basic
title: CTF - Challenges - School
---
## Challenge 1


 At the start of the challenge we were given few files to examinate.




![](ch01_1.png)

*Here are the hints*
* Programming: Filter out the bad stuff

* Cyber: What is a mirror?

* Media: GIF me the flag

* Networking: No hint. Extra points for fully understanding the content



**Solving media.txt:**

This chunk of text seems to be in Base64 format so lets try to convert it.
![](chunktext2.png)
Output of the conversion was total nonsense but by using the hint there was a recipe in Cyberchef to convert this from Base64 and rendering the output as an image. Therefore i was able to pickup the first flag.

![](ch01_2.png)



**2. cyber.txt:**

Let´s try to convert this from Base64
```bash
dW96dHs4dHhzRGswemtSejlVZ3B9
``````
Result:
```bash
uozt{8txsDk0zkRz9Ugp}
``````
By entering this string to Affine cipher and bruteforcing alphabets i was able to reveal the flag

![](CH01_cyber.png)
```bash
flag{8gchWp0apIa9Ftk}
```



## Challenge 2



 In this challenge I will need to complete atleast 2 out of 3 to proceed further. I choose to begin with the 1st option.


### Challenge 1/3

Obtained archive called hi.zip

![](ch02_7.png)\
It contained a txt file which was password protected.
Lets investigate deeper.

![](ch02_8.png)\
Both Strings and Binwalk did not give any valuable information

![](ch02_9.png)

### Bruteforcing
Finally i extracted the archive´s hash with zip2john and ran john with wordlist "rockyou". Password was cracked right away: "topsecret1"

![](ch02_10.png)

![](ch02_11.png)

Textfile inside archive contained a string which reminded the Base64 format.

![](ch02_12.png)

By using cyberchef i obtained the flag by decoding the string in Base64
```bash
flag{mBULPXy3ZdKYaNw}
````



### Challenge 2/3

>XML file which had both request and response encoded in Base64. By decoding both i was able to read what plaintext contained. One line in response was kinda obvious that this might be the solution.

![](ch02_15.png)

After decoding this string the answer was there.

``````bash
flag{sLpufQN9MK9x7Cb}
``````



### Challenge 3/3

To get started we are given a .zip file called i_am.

![](ch02_1.png)

By opening the archive there is another archive called am_i inside but it cannot be opened due an error which indicates that it might be broken somehow. 

First thing which comes into my mind is to try binwalk for the archive.

![](ch02_3.png)


Besides the am_i.rar archive nothing else interesting seems to be inside. Lets extract the other rar archive.
As above binwalk did not give any good results about the new archive so i tried to extract some strings from it.

![](ch02_4.png)

This "LAME3" part is weird and never seen this before. By googling it it seems to indicate somekind of media file.
Also the file command will output that it is an audio file instead of rar-archive.
ID3 header also points out that it is mp3

![](ch02_14.png)

![](ch02_13.png)

I will convert the filetype to mp3 and use audacity to play with it.

![](ch02_5.png)

It seems to have soundwaves maybe spectrogram gives an answer? :)

![](ch02_6.png)

**Nothing weird inside :(**

### Conclusion

Funny part is that my kali´s sound settings were somehow off and i didnt hear anything in this file. Now i downloaded the file to windows and found the flag.

``````bash
flag{nopeitisnot}
``````
Last flag by changing two flags into binary and then bitwise XOR to hexadecimal.
![](chunktext.png)

## Challenge 3
By looking the Javascript code inside the game provided for this challenge, i quickly noticed that there is a function which does not do anything for the game itself. This doGravity function combines 3 strings which includes myID, gravStr/Mod and another string. By adding these together it will produce a encoded text in Base64. Via decoding it will reveal the flag.

![](ch03_1.png)
```bash
myID = (Zmx) + myGamePiece.gravStr(hZ3tqS1g0R) + EZWRExkOHI2REN9 
= ZmxhZ3tqS1g0REZWRExkOHI2REN9
```
```bash 
flag{jKX4DFVDLd8r6DC}
```


## Challenge 4 - Challenger 

Wordpress site which belongs to somekind of software company.

![](ch04_2.png)
![](ch04_1.png)

Site has 3 pages: Main, Services and Contact page.
None of these sites had anything interesting on the page itself.
After examining the source code of the index page. I was able to find that there is an indicator for robots.txt file.

![](ch04_3.png)

This reminded earlier CTF's ive done so lets take look at the robots.txt if there is such file available. And for this challenge there was no need for hacking so this might be the way to go.

![](ch04_4.png)

Ok lets try this first string with Cyberchef which appeared at the top.

![](cho04_5.png)

```bash
flag{7pzcgyRF9r8wZJp}
``````
Here it was :)



## Challenge 5 - Phishing email

For this challenge there are two files to start with. I chose to start with the first one which had all the information what the email had which was send to IT-department from IT-department :))

Text in the email did not seem to be helpful for solving this challenge. That´s why i focused to the attachment and base64 encoded text at the bottom.

![](ch05_1.png)

I started decoding this to plaintext.

![](ch05_2.png)

Lets examine the output..

![](ch05_3.png)

Okay, it does not make any sense but i can see there some strings which are readable. Lets use Strings to extract all the possible outcomes.

![](Screenshot%202023-09-30%20at%2021.22.39.png)

Well, there is a flag which was the answer for this challenge. Im not quite sure was this the main solution for this challenge but i solved it though :)
```bash
flag{5tr1ng}
``````
