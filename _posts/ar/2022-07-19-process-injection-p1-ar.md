---
date: 2024-05-01T13:00:00.000Z
layout: post
title: Process Injection Techniques [1] - Intro (Ar)
subtitle: Process Injection Techniques - Part 1
description: >-
  This is the 1st article about process injection techniques
image: >-
  /assets/img/uploads/ar/process-injection-series/pi1.png
optimized_image: >-
 /assets/img/uploads/ar/process-injection-series/pi1.png
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
الـ Process injection من ضمن الـ Evasion Techniques اللي بتستخدمها الMalware للتخفي والهروب من عمليات الDetection وتخطي الدفاعات الأمنية، وبتُستخدم كمان في الـ Persistence.
<br>
اللي بيحصل في العملية دي ان الـ Malicious Process بتنفّذ code معين جوا Address space خاص ب Process تانية تكون شرعية وموثوق فيها بالتالي هتحقق الأهداف اللي قلنا عليهم وهم ال Stealth & Persistence.
<br> 
<br>
في المقال دا انا كاتب بشكل مختصر ايه هي الـ Process Injection و نبذة عن أشهر الـ Techniques فيها، وفيه أجزاء تانية هشرح فيها بالتفصيل كل تقنية منهم لوحدها وازاي نكتشفها واحنا بنعمل Malware Analysis ✍️
</span>
</p>

<p dir="rtl">
<span>
الأول خلينا نعرف مصطلحين مهمين هنستخدمهم في الكلام وهم <b>Process</b> و <b>Process Address Space</b> عشان هنحتاجهم في الكلام الجاي...

الـ Process معظمنا سمع عنها أو شافها في الويندوز، هي عبارة عن resource mechanism بتمثّل instance أو نسخة من برنامج شغال بالفعل (معموله Load في الميموري)..<br>
-كل process بيبقى ليها بيئة منعزلة خاصة بيها بتستخدمها في ادارة الـ resources او الموارد اللي بيحتاجها البرنامج.<br>
-كل process ليها على الأقل Thread واحد وده غالبا بيكون مسؤول عن تنفيذ الكود بدايةً من الـ entry point الخاصة بالـ executable file.<br>
(مش شرط أي process تبدأ من الـ entry point علطول لان فيه Malware بتبدأ من points مختلفة زي الـ TLS وغيره.. مش موضوعنا حاليا)
<br><br>
<b>ملاحظة</b>: فيه عمليات بدون threads بس بيكون ملهاش لازمة ولما الـ kernel تشوفها مبتعملش حاجة بتقفلها.
<br><br>
أما Process Address Space عبارة عن مجموعة العناوين اللي بتشير ليها الـ Process في الـ Code بتاعها... واللي يقدر يوصلها المعالج، سواء كانت Physical Addresses أو Virtual Addresses.
<br><br>
<b>بعض الـ Process Injection Techniques:</b>
<br>
1-Classic DLL Injection<br>
2-Reflective DLL Injection<br>
3-Thread Execution Hijacking<br>
4-PE Injection<br>
5-Process Hollowing<br>
6-Hooking via SetWindowsHookEx<br>
7-IAT & inline Hooking<br>
8-Injection via Registry<br>
9-Extra Window Memory Injection (EWMI)<br>
10-Asynchronous Procedure Calls (APC) Injection<br>
11-API Injection<br>
 ... وغيرهم.
<br>
<br>
<b>1-Classic DLL Injection:</b><br>
من التقنيات الشائعة والسهلة في الاستخدام، اللي بيحصل فيها ان الـ Malicious Process بتحقن الـ Path الخاص بالـ Malicious DLL بتاعت المالوير جوا Address Space خاص بـ legitimate process موثوق فيها زي svchost.exe مثلاً.. بعدين بتستدعي الـ DLL دي باستخدام remote thread عشان تتنفذ.
<br>
من عيوب الطريقة دي ان الـ Malicious DLL لازم تتحفظ او تبقى موجودة على الDisk، وكمان بتبقى ظاهرة في الـ Import Table (اتكلمنا عنه سابقاً في منشور الـ PE Format).
<br>
<br>
<b>2-Reflective DLL Injection:</b><br>
الطريقة دي بتعتمد على تحميل الـ DLL من الMemory بدلاً من الـ Disk بعكس الطريقة اللي فاتت، وبما ان الويندوز مش بيحتوي على LoadLibrary Function لتحميلها بالطريقة دي؛ فالمهاجمين هنا بيضطرو يكتبو Loader Function ودا بيساعدهم في تخطّي المراقبة الموجودة على عمليات الـ Loading.. والباقي معروف.
<br>
<br>
<b>3-Thread Execution Hijacking:</b><br>
الطريقة دي بتتم من خلال 3 مراحل اساسية بيطلق عليهم مصطلح SIR ودا اختصار Suspend, Inject, Resume<br>
اللي بيحصل هنا ان الـ Malware بيستهدف legitimate thread يكون موجود وشغال بالفعل وبيعمله suspend الاول، بعدها بيعمل inject لـ malicious code جواه، وبعدين يعمل resume للـ thread عشان ينفذ الـ code..<br>
الكود دا ممكن يحتوي على shellcode او path خاص بmalicious dll او address خاص بـLoadLibrary.
<br>
<b>ملاحظات:</b><br>
- لاحظ انه بيستخدم الEIP register عشان يتحكم في الـ execution زي انه يعمل resume او pause مثلاً.<br>
- لما يحصل suspend أو resume للـthread أحيانا بيسبب مشكلة للـ system ويخليه ي crash؛ عشان كدة بعض الـ malware المعقّدة مش بتعمل suspend علطول وممكن تكمل شغلها لحد ما تتأكد انه مش هيسبب مشكلة.
<br><br>
<b>4-PE Injection:</b><br>
الطريقة دي بتتميز ب ان الـmalware مش محتاج يحط malicious dll على الـdisk ودا بيشبه الReflective DLL Injection في النقطة دي، لكن الـ PE injection بيعتمد على حقن الـ malicious code في process موجودة ع السيستم وشغالة، بعدين بينفذ الكود عن طريق shellcode أو remote thread.
<br>
من المشاكل الموجودة في الطريقة دي ان لما بيحصل inject لل PE في Process تانية بيبقى ليها base address جديد مش معروف وبيحتاج لحساب عناوين الـ PE مرة تانية، وهنا الـ malware بيحتاج يبحث عن الـ relocation table address عشان يحسب عناوين الـ image من جديد.
<br><br>
<b>5-Process Hollowing:</b><br>
اللي بيحصل هنا ان الـMalware بيعمل unmapping أو hollowing (تفريغ) لل code من ذاكرة الـ legitimate process (زي svchost.exe مثلا) وبيستبدل الـ memory space الموجود مكانه ب malicious executable علطول (بدل حقن الcode في البرنامج زي ما شفنا في الdll injection قبل كدة).
<br><br>
<b>6-Hooking via setWindowsHookEx:</b><br>
الـHooking بشكل عام بيُستخدم عشان يعمل intercept أو اعتراض للـfunction calls؛<br>
فالMalware ممكن تستغله لتحميل Malicious DLLs خاصة بيها عند events معينة في الThread ودا باستخدام دالة <b>SetWindowsHookEx</b>؛ فبمجرد حقن الDLL بيقوم الmalware بتنفيذ الmalicious code بدل العملية الأصلية اللي اتبعت الthread id بتاعها لدالة setWindowsHookEx.
<br><br>
<b>7-IAT Hooking:</b><br>
في التقنية دي الmalware بيعتمد على تعديل الImport Address Table بحيث لما legitimate application موثوق منه يستدعي API من DLL معينة يقوم منفذ الReplaced Function بدلاً من الOriginal Function.
<br>
ملاحظة: فيه نوع تاني اسمه Inline Hooking بيعتمد على تعديل الAPI Function نفسها بدلاً من تعديل الIAT.
<br><br>
<b>8-Injection via Registry:</b><br>
فيه keys في الregistry بتقوم الmalware باستغلالها في الinjection والpersistence زي: Appinit_DLL, AppCertDlls, IFEO
<br>
مكان الKeys في المسارات دي:<br>
<code>HKLM\Software\Microsoft\Windows NT\CurrentVersion\Windows\Appinit_Dlls</code><br>
<code>HKLM\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Windows\Appinit_Dlls</code><br>
<code>HKLM\System\CurrentControlSet\Control\Session Manager\AppCertDlls</code><br>
<code>HKLM\Software\Microsoft\Windows NT\currentversion\IFEO</code><br>

و دول هنشرحهم بالتفصيل في جزء تاني.
</span>
</p>