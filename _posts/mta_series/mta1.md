---
date: 2023-05-24T13:00:00.000Z
layout: post
title: Malware Traffic Analysis 1
subtitle: CTF challenge [write-up]
description: >-
  This is my write-up for the Malware Traffic Analysis 1 challenge on the CyberDefenders platform
image: >-
  /assets/img/uploads/malware_traffic__analysis/mta1.jpg
optimized_image: >-
  /assets/img/uploads/malware_traffic__analysis/mta1.jpg
category: writeup
tags:
  - writeup
  - cyber_defenders
  - dfir
  - malware_analysis
  - wireshark
  - brim
  - zeek
  - network_miner
  - malware_traffic_analysis_series
author: Abdullah Aiman
paginate: true
---

Challenge Requirements:
- Wireshark
- Network Miner
- BrimSecurity & Suricata (Just follow the video instructions on the details page)
- VirusTotal Website

> Follow the challenge details & instructions from [here](https://cyberdefenders.org/blueteam-ctf-challenges/17#nav-overview) before the start.

> You can find the challenge questions [here](https://cyberdefenders.org/blueteam-ctf-challenges/17)

given a **.zip** file: `c04-MalwareTrafficAnalysis1.zip` which contains a pcap file (The password of zip is: `cyberdefenders.org`)

Lets analyse the file and answer the questions...
___

## Q1. What is the IP address of the Windows VM that gets infected?

We have many ways to get the IP address depending on the tool we use, But I will use **NetworkMiner** to get it faster:
Open the pcap file in NetworkMiner, under **Hosts** tab you will see only 1 windows host with IP `172.16.165.165` and that is the answer.

![](/assets/img/uploads/malware_traffic__analysis/mta1/1.png)

If you want to get it with **Brim**, Just drop the pcap file into brim, wait to load the data, go to Queries in the left panel and select `Suricata Alerts by Source and Destination` you will see the exploit kit & trojan alerts about the same IP that we found in the previous step `172.16.165.165`:

![](/assets/img/uploads/malware_traffic__analysis/mta1/2.png)

You can get back to the data and use filter `event_type="alert"` to show all alerts that gives the same result about Exploit_KIT delivering an archive to the client with destination IP (172.16.165.165):

![](/assets/img/uploads/malware_traffic__analysis/mta1/3.png)
 
## Q2. What is the hostname of the Windows VM that gets infected?

In NetworkMiner, double-click on the windows host and you will see the `hostname`:

![](/assets/img/uploads/malware_traffic__analysis/mta1/4.png)

You can also get it from Wireshark by filtering the traffic with `nbns` or `dhcp` and look for Host-Name in the dhcp-request or dhcp-inform packet details:

![](/assets/img/uploads/malware_traffic__analysis/mta1/5.png)

If you are using Brim, just use `dhcp` filter and you will see the hostname directly which is `K34EN6W3N-PC`:

![](/assets/img/uploads/malware_traffic__analysis/mta1/6.png)

## Q3. What is the MAC address of the infected VM?

Do the same steps as above in NetworkMiner,Wireshark or Brim to get the MAC address of the host:

![](/assets/img/uploads/malware_traffic__analysis/mta1/7.png)

We already got it in the previous question.

## Q4. What is the IP address of the compromised web site?

In Wireshark, Go to `Statistics` > `HTTP` > `Requests` you will notice that we have 6 websites:
- www.youtube.com
- www.ciniholland.nl
- www.bing.com
- stand.trustandprobaterealty.com
- adultbiz.in
- 24corp-shop.com

and www.ciniholland.nl has the most number of requests which is 20 while stand.trustandprobaterealty.com is the 2nd one with only 12 requests:

![](/assets/img/uploads/malware_traffic__analysis/mta1/8.png)

Also go to `Statistics` > `HTTP` > `Request Sequences` you will see that the user opened bing.com first and make a search for ciniholland and then visited www.ciniholland.nl which redirects the user to all other next pages & websites include the website that conatains the exploit-kit.

![](/assets/img/uploads/malware_traffic__analysis/mta1/9.png)

If you filterd the data to show all http traffic related to the infected host with `http && ip.addr == 172.16.165.165`, you will see the IP address for www.ciniholland.nl website which is `82.150.140.30` and this is the right answer.

![](/assets/img/uploads/malware_traffic__analysis/mta1/10.png)

## Q5. What is the FQDN of the compromised website?

FQDN stands for **Fully Qualified Domain**, the FQDN for www.ciniholland.nl is `ciniholland.nl`.

## Q6. What is the IP address of the server that delivered the exploit kit and malware?

According to the 3rd screenshot in **Question1**, the IP that delivered Exploit-kit to the infected host is `37.200.69.143` as we can see:

![](/assets/img/uploads/malware_traffic__analysis/mta1/11.png)

## Q7. What is the FQDN that delivered the exploit kit and malware?

As we know the IP address from previous question, we can use it to get the FQDN from the hostname by Wireshark, NetworkMiner or Brim.

If you are using NetworkMiner, just write the IP `37.200.69.143` in **Filter** and you will get the host name which is `stand.trustandprobaterealty.com`:

![](/assets/img/uploads/malware_traffic__analysis/mta1/12.png)

By Wireshark, add `http && ip.addr == 37.200.69.143` filter to get the hostname:

![](/assets/img/uploads/malware_traffic__analysis/mta1/13.png)

Another way using Brim, use `_path="http" and 37.200.69.143` filter and you will get it:

![](/assets/img/uploads/malware_traffic__analysis/mta1/14.png)

## Q8. What is the redirect URL that points to the exploit kit (EK) landing page?

We already know the IP for the website that delivered the Exploit Kit from question 6, So we can check its http request to get the **Referrer* header which is the redirect URL..

To do that with Brim, just use the following filter `37.200.69.143 _path="http" | sort ts` to show only logs releated to that IP and sort them with time, then check the **referrer** column  for the first request as the following shot:

![](/assets/img/uploads/malware_traffic__analysis/mta1/15.png)

For Wireshark users, use `http && ip.addr == 37.200.69.143` filter, click on **Time** column to sort it, then check the referrer of the first request as the following:

![](/assets/img/uploads/malware_traffic__analysis/mta1/16.png)

> Note: If you want to make a new column that display the refferer next to host columns without going to packet details, just follow the next steps..

Go to `Edit` > `Preferences` > `Column`, click on **+** mark to make a new column, type its **Title** (any thing such as `Referrer`), in **Type** select `Custom`, in **Fields** type `http.referer`, drag the column and drop it the place you want (maybe under Host) and then click `OK`:

![](/assets/img/uploads/malware_traffic__analysis/mta1/17.png)

You can see the new column as the following image:

![](/assets/img/uploads/malware_traffic__analysis/mta1/18.png)

## Q9. Other than CVE-2013-2551 IE exploit, another application was targeted by the EK and starts with "J". Provide the full application name.

By checking alerts in **Brim** with `event_type="alert" dest_ip=172.16.165.165` filter, we can notice that all alert.signatures are about JAR/Java files:

![](/assets/img/uploads/malware_traffic__analysis/mta1/19.png)

As we know the answer starts with "J" according to the question, then the Exploit Kit maybe targeting `Java`, which is the correct answer.

Another way to make sure of that: export http objects (files) related to the IP that delivered the malware (37.200.69.143) using Wireshark or NetworkMiner, scan them on **VirusTotal** which results that:
- .swf file has a vulnerability targeting Adobe Flash Player
- .jar file has a vulnerability targeting Java Runtime Environment

So, Answer will be `Java`.

## Q10. How many times was the payload delivered?

In Wireshark, by using this filter `http && ip.addr == 37.200.69.143` and sorting the result by **Time** column as we see in the following image:

![](/assets/img/uploads/malware_traffic__analysis/mta1/20.png)

We can notice that, There are 2 flash files, 2 java archives, 2 xml files & 3 application/x-msdownload files

We can check the exported http objects also, it gives the same result:

![](/assets/img/uploads/malware_traffic__analysis/mta1/21.png)

By exporting these files and scanning them on [VirusTotal](https://www.virustotal.com/gui/home/upload) we will see only the html, swf & java files are malicious

**I noticed that:**
- **swf** file has a cve `cve-2014-0569` which is a security vulnerability in **Adobe Flash Player** and the .swf file is used as the exploit file for it.
- **jar** file has a cve `cve-2012-0507` which is a critical security vulnerability that existed in **Java Runtime Environment** (JRE) software. the .jar file is a part of the exploiation for this vulnerability

![](/assets/img/uploads/malware_traffic__analysis/mta1/22.png)

However, The payload will be application/x-msdownload file that deliverd `3` times in 3 requests as 3 files with same size, and this is the answer.

Another way to solve it, just follow the requests and alerts in **Brim** using some filters such as `event_type="alert" src_ip=37.200.69.143 dest_ip=172.16.165.165 | sort ts` you will notice a `3 alerts` for The Exploit-Kit: `ET EXPLOIT_KIT GoonEK encrypted binary (3)`:

![](/assets/img/uploads/malware_traffic__analysis/mta1/23.png)

## Q11. The compromised website has a malicious script with a URL. What is this URL?

From Question 4 and 5, the compromised website is ciniholland.nl with IP 82.150.140.30 ..

We can filter the traffic in **Wireshark** by `http`, go to the first frame belongs to ciniholland.nl and follow tcp or http stream:

![](/assets/img/uploads/malware_traffic__analysis/mta1/24.png)

By looking for a malicious script with a URL, I found a (hidden) script that redirect the victim to `http://24corp-shop.com` website which is the answer:

![](/assets/img/uploads/malware_traffic__analysis/mta1/25.png)


## Q12. Extract the two exploit files. What are the MD5 file hashes? (comma-separated)

According to Question10, I said We have many files but only `jar` and `swf` are used as exploit files in their vulnerabilities.. So Let's check their MD5 hashes on **VirusTotal**:

- jar:	`1e34fdebbf655cebea78b45e43520ddf`
- swf:	`7b3baa7d6bb3720f369219789e38d6ab`

The answer format tells us first part will start with 7 and second start with 1 So the final answer will be: `7b3baa7d6bb3720f369219789e38d6ab,1e34fdebbf655cebea78b45e43520ddf`


Finally, In this challenge I'v tried to show you how you can use other tools to solve the questions because in the next challenges Wireshark will not be enough .