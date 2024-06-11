---
date: 2022-07-31T13:00:00.000Z
layout: post
title: DLL Injection (Ar)
subtitle: Process Injection Techniques - Part 2
description: >-
  This is the 2nd article about process injection techniques
image: >-
  /assets/img/uploads/ar/process-injection-series/pi2.png
optimized_image: >-
  /assets/img/uploads/ar/process-injection-series/pi2.png
category: article
tags:
  - arabic
  - article
  - malware_analysis
  - blue_teaming
author: Abdullah Aiman
paginate: true
---
<p dir="rtl" style="font-weight:600">
<span>
الجزء2 من سلسلة الـ Process Injection Techniques هشرح فيه الـ DLL Injection ببعض التفاصيل ✍️
</span>
</p>

<p dir="rtl">
<span>
<b>خطوات تنفيذ عملية الـ DLL Injection من خلال الـ Malware:</b><br><br>
<b>1-الخطوة الأولى:</b><br>
الـ Malware محتاج يحدد الـ Process اللي هيحقن فيها الكود؛ فبيستخدم 3 API Calls مشهورين عشان يبحث في العمليات الموجود ويختار منهم، ال3 Calls هم:<br>
<b>1-CreateToolhelp32Snapshot:</b> دي Function بتعمل snapshot من الProcesses اللي شغالة بالاضافة لل heaps والmodules والthreads اللي بيتم استخدامهم من خلال الProcesses دي.
<br>
<b>2-Process32First:</b> دي بتستقبل المعلومات الخاصة بأول process موجودة في الsnapshot اللي فاتت.
<br>
<b>3-Process32Next:</b> دي بقى بتعمل iteration loop على ال processes الموجودة بعد الprocess الأولى عشان الMalware يقدر يحدد بعد كدة الprocess المستهدفة.<br>
لحد هنا خلصنا أول خطوة (تحديد العملية المُستهدفة targeted process).
<br><br>
<b>2-الخطوة التانية:</b><br>
بعد تحديد العملية المستهدفة بيقوم الـ Malware يستدعي فانكشن <b>OpenProcess()</b> عشان يعمل handle أو مؤشر على الـ process اللي تم استهدافها.
<br><br>
<b>3-الخطوة التالتة:</b><br>
بيستدعي فانكشن <b>VirtualAllocEx()</b> عشان يحجز أو يخصص مساحة في الذاكرة (Memory) عشان ينفذ فيها عملية الحقن (injection).
<br><br>
<b>4-الخطوة الرابعة:</b><br>
بيستدعي فانكشن <b>WriteProcessMemory()</b> عشان يكتب مسار الMalicious DLL في المساحة اللي خصصها الخطوة اللي فاتت في الميموري.
<br><br>
<b>5-الخطوة الأخيرة:</b><br>
بيعمل Execution للMalicious Code/DLL اللي تم حقنه ودا يقدر يحققه الMalware من خلال كذا API بس في الآخر الفكرة العامة في الخطوة دي واحدة وهي تمرير الAddress الخاص بال LoadLibrary للAPI عشان يخلي الProcess تنفذ الMalicious DLL بدل الMalware ما ينفذها بنفسه، والAPIs الممكن استخدامها في المرحلة دي هم:<br>
<b>CreateRemoteThread()</b><br>
<b>NtCreateThreadEx()</b><br>
<b>RtlCreateUserThread()</b><br>
<b>SetThreadContext()</b><br>
<b>QueueUserAPC()</b><br>
<br>
المثال الموجود في الصور لدالة CreateRemoteThread() والباقي بيأدي نفس الغرض بس كل واحدة منهم بتختلف عن الباقي في طريقة تنفيذها وطريقة الdetection اللي ممكن تحصلها باستخدام الAVs وغيره بالتالي الكود هيبقى فيه اختلاف طبعا من واحدة لواحدة.
</span>
</p>

![](/assets/img/uploads/ar/process-injection-series/pi2-1.png)

![](/assets/img/uploads/ar/process-injection-series/pi2-2.png)

![](/assets/img/uploads/ar/process-injection-series/pi2-3.png)

### Resources - مصادر

- [10 Process Injection Techniques - Elastic](https://www.elastic.co/blog/ten-process-injection-techniques-technical-survey-common-and-trending-process)

- [Process Injection Techniques - Cynet](https://www.cynet.com/attack-techniques-hands-on/process-injection-techniques)

- [DLL Injection - haxtivities](https://haxtivitiez.wordpress.com/tag/dll-injection)

- [Inject All the Things - GitHub](https://github.com/milkdevil/injectAllTheThings)