---
date: 2022-07-11T23:48:05.000Z
layout: post
title: CountZero R1 challenges
subtitle: Task [write-up]
description: >-
  This is a write-up for all challenges that we solved in Round1 {Network Basics} with CountZero
image: >-
  https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_liy8LItzFOW0tZIOWMzAVLHzokYDSiRN-TU01QSRGDKoWgbjVj22IZ_6rq_D0CLsHiOL3z5RO-dclizFNq2Q5TUg0_8uONTu062pMJ_7L63unZTHiA-AnfqJvS8pypWBP660E3qz6TeQoxJJ4LF0uv1Xlb4gj9IqTtSHCGX5tbsrdxuS8KGA2A4aYQ/w640-h360/r1.png
optimized_image: >-
  https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_liy8LItzFOW0tZIOWMzAVLHzokYDSiRN-TU01QSRGDKoWgbjVj22IZ_6rq_D0CLsHiOL3z5RO-dclizFNq2Q5TUg0_8uONTu062pMJ_7L63unZTHiA-AnfqJvS8pypWBP660E3qz6TeQoxJJ4LF0uv1Xlb4gj9IqTtSHCGX5tbsrdxuS8KGA2A4aYQ/w640-h360/r1.png
category: writeup
tags:
  - writeup
  - ctf
  - packet_analysis
  - wireshark
author: Abdullah Aiman
paginate: true
---
# Challenges list
- Packets Primer
- Shark on wire 1
- Wireshark doo dooo do doo
- Easy101
- Filteration-01
- Wireshark twoo twooo two twoo
- ARP Storm

**Note:** These challenges from (PicoCTF, JISCTF, CyberTalents)

## Challenge [1] | Packets Primer 
- **Hint:** Follow the stream
- **Flag format:** picoCTF{some words here}

According to the hint, just follow the tcp stream & you will get the flag:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg4yclumB7MCFPi3dEupKM9BM3WWCim-wEhTTNGXsn3ornzpR80YtHbpeqCzLAWOLS8XJf-eQ6c03YOPAo7EgPet5jgxmsNXOD748jTik6UUstVlXWd3fwVLK-uJThY4m9EZxcg1c1uxlO4b0iKaFOqij4WzLxSww1s5vo5V7qgi4uQi34bZ8cvkEkmcg/s995/1.png "wireshark screenshot")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg82R-fMmR3ChZdO3eploIVOgUF7GVX-pkg_mg1sj2mUGnTsUCseGCE4Ztkx2vpvKje75Nc3zN0Q5Rb0uTSRvzcUjkg4Ti_AU82sjnr0STc_ADF4KTv9DI2Gi1eY-My3vrpKU7pty72er0fbVOVpND01lv9Wq_Otsrv7eG8Pl5Vln0SlWlDZl6_qQFsLg/s612/2.png "wireshark screenshot")

To remove the space between each character & get the right flag, we can use [Cyberchef](https://gchq.github.io/CyberChef/) like that:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmicKkuLmVgZ9SLzFLl_EINV6MQvrEbjyhnOPEAoGCwOO6OsFEuERWCH05zx4DYojmF20k_xbKLbCCTda6rrxieoLxVxZAPq6VQQRvTWCUXMFVgNnkKuO_10yTCMjxPf7plalnfc9Lv3eExA5tk8GJbmf80VSrdLWYK1eCUswzoRVVxLSL7VaQCIKBDA/s1007/3.png "wireshark screenshot")
> **Flag:** picoCTF{p4ck37_5h4rk_b9d53765}

## Challenge [2] | Shark on wire 1
- **Hint:** Follow follow follow follow
- **Flag format:** picoCTF{some words here}

Again, hint is saying follow !! so we have a udp stream here.. lets follow it:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRNg6rxcTIBkJc_BGsp9_3UbTryBgWlzBZYUi0q2M3PNjeBhXrUczLaSBUW5bhGH-LZh3_2zNS3qlYmL0HEaSGiRQCFGJSBWzxxCBV2TP7NPTIucPELbcikNisiaslc2qbdCtDRKXrUTeYXrytQ4WNXmzO87d8I23Hkm49lmyt-Bj8qzN3d_JNl1ue0g/w640-h388/4.png "wireshark screenshot")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgA7tBVCvy1g3yKpGWBLWopUTE_tmYLQSoQvMCUEIG1lbiui3msEfXrcYxXTq4QHhuOoCzwfJMQzKf0FUQxkXhB1uRles4HD90bp5VGTM2L_TWkaHLijUF_IaH1v5dVdcjh9iDkV_SHJPJwmz9lpf-ubLSk1fhGNbrXfmbgg3WM8yL89pZZCezJlBp0OA/w640-h442/5.png "wireshark screenshot")

Nothing useful in the first stream, so let's check next streams:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEixQKmRC4yVtFNmS9HsZoftdSOeT5zO-k_jstLyJe97rEDO-S1gX4NlLFOLSoQGmkPwLqPBHk6pJCYI6wqGB3pmMzoKfc_Jaepvh5vrqffqLb4toFX1z621vuTLh42pw63XkUSDf_NKmthce2E8QUzZ7nePm6Viv09N6hYOXZIBrYwnoSho9pSR5rQDlg/w640-h448/6.png "wireshark screenshot")

I found the right flag after 5 streams ( in stream n. 6 )
> **Flag:** picoCTF{StaT31355_636f6e6e}

## Challenge [3] | Wireshark doo dooo do doo
- **Hint:** They said that ROT13 cipher is simple & the Chef know it
- **Flag format:** picoCTF{some words here}

First thing, we don't have a clear point to start... So I decided to check the exists protocols first:
`Statistics > Protocol Hierarchy`:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjT_rPxulsDAA9BzB4PG0TrVslm4LXolwHtxuN1BdPE-znbBm6TsXQJE9uDfo2jkqfGa6vGhBjom44eeMjSzAQS9R5k9l3eGM95hcBs-MbkDZuAA6Zyxe3-e411lk6FJQK3fhNLimNJo_3KTT8ok1sY-KxLU8Z9SElP4pvLh5M9O1kQmyasmtP73AeCKg/w640-h416/7.png "wireshark screenshot")

I noticed that TCP have 99.8% of packets:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7el1IJYU3rHXVGM9OXrF997vQ7t7DuJLjjX4FkSVADI3EexmOjjZUcnzyh7IvDOeoSb_mbOdqRkXq4FEej7bzuB6gCIDjxixEd7vtQ7JN0hf6MiZ__a9OoUHowgGyncuHzXfPWtiCnPaeMSySx1-a0lMqN8TRTmuw_PxoBlOSj8Hq6xTDk7jJKq4PlQ/w640-h195/8.png "wireshark screenshot")

So let's go to analyze TCP packets
Just add `tcp` filter to show only TCP packets, go to packet number 1 and follow tcp streams like that:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh53fTdg3RExqRJ_9Ou1wHFAWOo1L6WouovZ9mC29uyvJGLcByBd1_DqvUCz2kv9EczSF58jmQqx3OGVd8xf4oUSL37lT4MZ7z3PFZ2lhTyzyN8iXS4iKYZgy959FkvtnxPaMoI1LwijC_OGI4yfIZB_2QCwJu6TnMX6E8RrBX-i5qeuPzUmMFMLceh3A/w640-h330/9.png "wireshark screenshot")

The first stream has no interesting data... it is just POST HTTP Request with some encrypted data about Kerberos (network protocol) :

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhW8jTgJ9aANObdf22pLlHW8BqpPtsV4JRKmZv-qEUzr4KO5XTtw4iXR_qkul6BqJ7om7KO4T-GdvQ5yP8MS_gZq3KBIv86S-MS3qIoC-mHbFNwymsuHS8ewaeTpC-v78rVwgYUmIZF8VyR8xj3JJVFYybNDvEe7lrjpOtGD8XbtBQ9fE98AeLJ3bUSCQ/w640-h424/10.png "wireshark screenshot")

Going through next streams we will get a rot13 data seems to be the flag in stream number 5 :

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMLpiHNFsPeB6TnhPt3h-9g124aCLcB1GZik-_5AIs4XxaMvMD_fPSyZGe5BxMjSUxqtj4g7cOoci05O4AZQFAc9DkrXa0gts-lRjiRkCqhtHEtsRz5wADaejK0eaujjFt4klneWgUh8kyidDUiBEqo-iozA8u387OHOYa42vRNPqbDpDAMfuuji5m-Q/w640-h422/11.png "wireshark screenshot")

After decrypting it using Cyberchef You will get the flag:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVVsB6WO8cwKrrcvNjwlbpudXJDDx_33saVC1dxcLshU0XHFMwgYwYGxGNX7chVU1aF6fjCSYHtOB5Dzh96pq5MTZSKPJLmKo-Hx4iULBLVBbnZx6l3VkMghZb3f1uggoJTqoMihB0052tsfz3G3UFgYMNZu5Bc1DQNTJt1TW3il9sDleWOBeClQcuVw/w640-h324/12.png "wireshark screenshot")
> **Flag:** picoCTF{p33kab00_1_s33_u_deadbeef}

## Challenge [4] | Easy 101
- **Hint:** It is the same challenge that we solved in fitst task in session 3, but the flag need a Chef ! (Our Chef rotated the flag with 13 & then encoded it by Base64)
- **Flag format:** JISCTF{some words here}

From the hint, I remember that we could solve this challenge by many ways such as (follow stream) or (export objects)  ... etc.
So, when exporting the http objects, I noticed that there is a file name encoded in base64 !!

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjd4N6Kv2bjk-ZTmRlq5vbcVzQdODaTr82DxMd6MZYYlLJTxa46ZKKlFT4Hon-dKyhtC4rqPkP9BqZjA2pMMKa635zAVp5k5t6azrmVw2pRJjLtsjoN7HeJvQqOB8dt_s4Hk2p9jVvMhYH8JoGFaFEFBmMsqxi2tEkwsOBG7RA5Yam9W380QM5W8jb61Q/w640-h372/13.png "wireshark screenshot")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg5HUvJ44qyatywHoFg6TMN30bxBfLuIuvPLSi9GkTFCgmrhxyx1PGG7j6kPzRVqE8dsZm2oQVsup7mvLNlBnQgRTUU5gUdyDrN0-tzODfqOCjhSZhBDZepGd7NV0V4kvwosZ7pvK_W_HBiEbIM-HpEP9MO5Yb9DEQKZxXbIOGkSV6GUki9jSrSAIWnvA/w640-h302/14.png "wireshark screenshot")

Just 1 click on these data will bring us to the packet that contains the base 64 values:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh99tR6XR5eBlEWH6HHvaRV5HSgVcBpRBacg4jJbHIbHU77sTkxD6EkM9-dVuvxp0C3Ka3C4cZbCHFxgdUIyX0aT93mUK_Y1Djp50WbaMZR92EgFLp0ojOvAXtpRrJobgTI1Jnc_6Dnh4OPTz9BGu1L7DmZpHciuA367sUuRsqwDKYg7pbQh-Z7prqlrw/w640-h283/15.png "wireshark screenshot")

You can get the value by (show packet bytes) like that:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgL5V5q_xpvwkJcryc6weX3ThVnA7xaZljMHnm6xgLYkEI9iEDsKTHk2iXuHC2yzDDNEbi943qTJIMSwnQ_mbDf-OMsGXtbQfis9OYx6qCdKpRzSkTzflg1XI6eVSDBGAcR-s8lbM1blRWfCj-qpxVazLsxd411Yf3mr1t6slYiq4IZ90kOPcE8uQ29tA/w640-h338/16.png "wireshark screenshot")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh5RmG6hUGFScm2ZIwUeTy-9QuQChnT2NkVM_0SDuX2nwea9Z89DHrT-rY46VVv006WIaRbZTfTNjN17l4Ws7yG51E2v2i2jtkxPIOM7SyVst4-BVlZxXAmR84wDnKLPQADSUYX3THa0XX9pXBvzlWfUgu0sUfrFyBY8pmdB8No2dbg1LmWElj8d3Rpmw/w640-h334/17.png "wireshark screenshot")

After using cyberchef on it I got this rot13, so by decrypting it from Rot13 again I got the flag:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjaIX2kVzNV0GvLv35KKNZ3fi0QJE9QWyQOX9tX17gZDiHLlPtg_YQmNTt2qqT2j9S_sqG3KD0zj6aDrJJD17rkmhjZpdxmkROUgoJqCi2G47lSG8KvPLS8B54xNOSgaqBQTI6inXCxCGNp5O2C_TKp2mOgO46uyEUXegQ_RXoNNBFg8-3kEsrMlaVzYw/w640-h292/18.png "wireshark screenshot")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiRBzp10E5OoBlp01j1TWwq7iHsUCsJjk3k7MzJPV_DIYNXfrnHJ6YbENghKw8kDhMnGZWcCrOQvefHgssuBHb4OdBcCuRf5z9GCr_3zH1j1iIoalLVDeckb9K4F_MMXGP8XsrfPj4Wl0QwyG7TXQzWM6WPE5cIsXZx8d4WrZwyySUq3lm3byCA3d-iAQ/w640-h326/19.png "wireshark screenshot")
> **Flag:** JISCTF{JUST_S1MPL3_PC4P_F1L3_4N4LYS1S}

## Challenge [5] | Filteration-01
- **Hint:** ICMP packets maybe useful, use a filter to display them
- **Flag format:** JISCTF{some words here}

From the hint, adding `icmp` filter to show icmp traffic only:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEik_CGPW6qCOo3me-gd2Mbevm2YWnoQ4kWzG0qd8BZCgi1o9h0Dt0N7A9CjV6rLmpuklOU95WEmcvBf7zVY7lKKiZbxE8DYguRWMFOD0rDJrQldFvZZhqrH-PlgGouBQ_MKXzPcUApnnDbmtEg0WmbTemPyrzhCIm9_BTbEgLslvws19ZE8pFgxnHIc5Q/w640-h360/20.png "wireshark screenshot")

As we know the flag format starts with **JISCTF**,  If we click on the first packet (Ping Request), We can notice that there is a `J` character in the packet bytes area

The second one which is (Ping reply) has a different syntax which contains `@` instead:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgkbBLODC2KQqGF3Ihp6roHzuV6DtFBs5IgscL3MlzEV6QghCy7T_6MMHY0xSPS_624A2ujtJtNvWXMNnkXVL9MOvrraQz0jj5rtlcq3TTcwCfhYtMflcdIR8ZQjuOYIoNcmXajQJ-ePTWot9hB7bunc6apTJHhKMcu8vPl-9i5XYOIb1DBfpSstkTsEg/w640-h314/21.png "wireshark screenshot")

The 3rd one has the same syntax of the first packet but with `I` character... So, there is a 1 character of flag in every (Ping request) packet!

You can filter out (ping response) to decrease the number of the displayed packets and then follow packets 1 by 1 to get every character of the flag, (OR) without filtering, you can get it also!

`icmp.type eq 8` or `icmp.type == 8` filters will show only ping request packets

The final flag with all characters will be:
> **Flag:** JISCTF{M4LW4R3_3XF1LT3R4T10N_US1NG_1CMP_TTL}

## Challenge [6] |  Wireshark twoo twooo two twoo
- **Hint:** Attackers can use Amazon servers maliciously, don't waste your time on Google DNS it is official
- **Flag format:** picoCTF{some words here}

From the hint, I'm dealing with DNS packets so first thing I added `dns` filter, then I filterd out **Google DNS** because it is official (as hint said!!) using
`!(ip.addr == 8.8.8.8)` filter

So, our filter will be like that: `dns && !(ip.addr == 8.8.8.8)`

After adding the 2 filters we will get only 42 displayed packets from 4831.. Amazing !!

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0JEmT7bICbM2zblUQP3wwPe5vLPKt6ziqvLviIdunrr7YcNQkRRmRNyncLPoJwecJAdM9tpW_Ms2yDejDjsyvCpHmi2cUkKH2s4Ggw_udXBhTKMIhMbFR9GwLM5nUa4sdUrnIXSKpsFdVN4dYjitx0TLwhy8KDYqw_JEnraDvQh7GhfPOMUJJMZheNQ/w640-h224/22.png "wireshark screenshot")

As we see there are too many domains, but if you back to the hint you will see "Attackers can use Amazon servers" !! so we can just add one more filter to show only packets that contains "amazon" with this syntax: `&& frame contains "amazon"`

**Note:** The final filter will be `dns && !(ip.addr == 8.8.8.8) && frame contains "amazon"`

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLvMGtki44YLf0K77Pdun6TuryqAX7fpoJaJxZJHhjFReYaj2hPMPqxcuIy1GfKoh9x9ODu3U2q0aWOsuJhXNgNs7mgnxRD7MOuHgbe5RTAMEUHElIiD6zSBH0BSTgFNdJJwifCDO2cslsfoDPqGHDtjoaIQPJ34CbJWxYZTN-KpbKZpqCX8AZqkljGQ/w640-h210/23.png "wireshark screenshot")

As we can see in the last image there are base64 data placed as subdomains, ignore the other packets which are "query response" and extract these values:
`cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ==`

Just decode it from base64 and you will get the flag !
> **Flag:** picoCTF{dns_3xf1l_ftw_deadbeef}

## Challenge [7] |  ARP Storm
- **Hint:** An attacker in the network is trying to poison the arp table
- **Flag format:** flag{some words here}

The given pcap file only contains ARP traffic, no (follow stream) or (export objects), there is just some information in info column that say `Uknown ARP opcode 0xvalue...`
If u noticed the packet bytes you will see this opcode in ASCII format which is "Z" for the first packet and "=" for the last packet, so it seems to be Base64 !!

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjhRqSkPlbP5qWQpTjQlHFT_nQ8LEIXcGVtM4mhm8gej_pPf1R80_tRHETmCJxlNge0U7UMaE4ngfwR-lPPdDguMWc1DjAKh4kHRoC1PlF3qdi9JGZ3mREMM0oqah2VIzfWPMO9PIK8fRlf_Yt6VVzQ3V2Rk2x46y0C8t29lzvkA2ln_pbgB32zSeMM6Q/w640-h416/24.png "wireshark screenshot")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgNT9a4cul-zr9VLvy9vjVHOBHuRTX3C7ShHr3IDrR7xaKBYC0eh978xhgKjReZOiQpKrIcDk8whRXFdwj8qtZYyRiHd26owmJiOFuk0WpPkm2fjFIUJtf3L3c7MJyi2mBJMjh3f0p9Jib3jHztvUn6lAsmcCWFe1ORMJQDRI0GiLYypkCJGc2yaaO-xQ/w640-h222/25.png "wireshark screenshot")

You can extract these values by each packet ... and then decode it with Cyberchef to get the flag.
> **Flag:** flag{gr@tuit0us_0pcOde_1s_Alw@ys_A6uSed_t0_p01s0n}

**Note:** You can extract data from ARP Storm & Filteration-01 by using **Tshark** tool easly.
