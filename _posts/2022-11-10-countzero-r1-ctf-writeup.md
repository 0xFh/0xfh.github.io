---
date: 2022-11-10T13:00:00.000Z
layout: post
title: CountZero R1 Competition
subtitle: CTF challenge [write-up]
description: >-
  This is my write-up for Flags Storm challenge from CountZero Round1 Final Competition
image: >-
  /assets/img/uploads/countzero-r1-ctf-writeup/storm.png
optimized_image: >-
  /assets/img/uploads/countzero-r1-ctf-writeup/storm.png
category: writeup
tags:
  - writeup
  - ctf
  - packet_analysis
  - wireshark
  - cyberchef
author: Abdullah Aiman
paginate: true
---
Count Zero | Round 1 { Network }
CTF challenge write-up

# Flags Storm (The challenge)

given a **.pcap** file with a big number of packets & many questions .. So lets solve all of them

## Q1.What is the total number of packets & How many TCP conversations?
flag format: countZero{Packets_TCPconversations}

We can get the total number of packets from the status bar
For TCP conversations, we can get them from `Statistics > Conversations > TCP [Number]` :

![](/assets/img/uploads/countzero-r1-ctf-writeup/1.png)

>**Flag:** countZero{7621_144}

## Q2.What is the password used in the Telnet session?
flag format: countZero{some words here}

Since there is a big number of packets (7621), we can use `telnet` filter to display only telnet packets (186 only) :

![](/assets/img/uploads/countzero-r1-ctf-writeup/2.png)

Then we can follow the tcp stream from 1st packet and we will get the password of the session which is the flag:

![](/assets/img/uploads/countzero-r1-ctf-writeup/3.png)

>**Flag:** countZero{An0Th3R_$tr1nG}

## Q3.What is the Source & Destination MAC Address of the malformed packet in the Telnet?
flag format: countZero{Source_Destination}

The question is asking about **the malformed packet** so if we back to the challenge and apply `telnet` filter we will get this packet, then we can identify the source & destination mac addressess as you can see in this image:

![](/assets/img/uploads/countzero-r1-ctf-writeup/4.png)

>**Flag:** countZero{00:0c:29:ab:84:38_00:0c:29:21:bb:ab}

## Q4.There is a hidden comment encoded or encrypted, Can you find & decrypt it?
flag format: countZero{some words here}

There are too many ways to get a comment from pcap file such as;

1. Going to `Statistics > Capture File Properties` & scrolling down to find the comments in the last line:

![](/assets/img/uploads/countzero-r1-ctf-writeup/5.png)

2. Going to `Analyze > Expert Information` & check the comments:

![](/assets/img/uploads/countzero-r1-ctf-writeup/6.png)

![](/assets/img/uploads/countzero-r1-ctf-writeup/7.png)

3-We can use `pkt_comment` filter to display only packets that contains a comment:

![](/assets/img/uploads/countzero-r1-ctf-writeup/8.png)

The comment seems to be a Base64, After decoding it we will get the flag in the right format:

![](/assets/img/uploads/countzero-r1-ctf-writeup/9.png)

>**Flag:** countZero{Well!I_Hate_$tr1nGs}

## Q5.There is important data hidden in protected archive file, What is it?
flag format: countZero{some words here}

After Analysing the pcap file, I noticed that there are some files in HTTP objects, one of them is archive which name is `Attachment.rar` :

![](/assets/img/uploads/countzero-r1-ctf-writeup/10.png)

So I saved it (You can do it by following these steps: `File > Export object > HTTP > Save`):

![](/assets/img/uploads/countzero-r1-ctf-writeup/11.png)

Once I opened this file it asked for a password:

![](/assets/img/uploads/countzero-r1-ctf-writeup/12.png)

Remember that We have a hint: `Take a look on the HTTP headers of the same request of archive`

So lets check HTTP headers..

By following the tcp stream of the http get request that contains `Attachment.rar` file:

![](/assets/img/uploads/countzero-r1-ctf-writeup/13.png)

we will notice that there is a cookie named `_pass` with value `tpass&Ad1Min80-107`

![](/assets/img/uploads/countzero-r1-ctf-writeup/14.png)

By using this value `tpass&Ad1Min80-107` as the password of the archive, we will get 3 files from the archive file which there is one of them `flag.txt` contains our flag as you can see in the next image:

![](/assets/img/uploads/countzero-r1-ctf-writeup/15.png)

>**Flag:** countZero{3xfiltra_ted_@rc!ve_0n_th3_W!Ld}

## Q6.What is the DNS Address, Record Type & The full Domain Name of "vulnweb" website?
flag format: countZero{Address_Type_DomainName}

We can use `frame contains "vulnweb"` filter to show only packets that contains vulnweb and it will display only 2 dns packets, then we can get the answer of our question:

![](/assets/img/uploads/countzero-r1-ctf-writeup/16.png)

from the last image:
- DNS address: `44.228.249.3`
- Record Type: `A`
- Domain: `testphp.vulnweb.com`

>**Flag:** countZero{44.228.249.3_A_testphp.vulnweb.com}

## Q7.Malware can hide important data in images, can you check them?
flag format: c0unt_Z3r0{some words here}

Backing to HTTP objects, We will notice png image as we see her:

![](/assets/img/uploads/countzero-r1-ctf-writeup/17.png)

Just save & open it you will get the flag:

![](/assets/img/uploads/countzero-r1-ctf-writeup/18.png)

>**Flag:** c0unt_Z3r0{Th!s_1s_a_Go0D_Try}
