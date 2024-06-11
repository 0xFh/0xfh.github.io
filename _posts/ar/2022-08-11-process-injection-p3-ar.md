---
date: 2022-08-11T13:00:00.000Z
layout: post
title: Thread Execution Hijacking (Ar)
subtitle: Process Injection Techniques - Part 3
description: >-
  This is the 3rd article about process injection techniques
image: >-
  /assets/img/uploads/ar/process-injection-series/pi3.png
optimized_image: >-
  /assets/img/uploads/ar/process-injection-series/pi3.png
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
الجزء2 من سلسلة الـ Process Injection Techniques هشرح فيه الـ Thread Execution Hijacking ✍️
</span>
</p>

<p dir="rtl">
<span>
قلت في الجزء الأول ان طريقة Thread Execution Hijacking بتتم على 3 مراحل بيطلق عليهم مصطلح SIR وده اختصار Suspend - Inject - Resume، وعرفنا فكرته الاساسية انه بيعمل suspend لل thread الاول بعدين يعمل inject لل malicious code وبعدين يعمل resume للthread لتنفيذ الكود.
<br><br>
في الطريقة دي الMalware بيستهدف thread لprocess موجودة بحيث يوفر على نفسه التعامل مع process كاملة ويتفادى إنشاء threads جديدة ليها بالتالي يتخطى عمليات الdetection بشكل أفضل.
<br><br>
كـ Malware Analyst توقّع تشوف الـ Functions Calls دي أثناء عملية الThread Execution Hijacking:
<br>
<b>-CreateToolhelp32Snapshot:</b> من الجزء2 قلنا ان دي وظيفتها عمل snapshot من الprocesses اللي شغالة بالاضافة للthreads المستخدمة في كل process.
<br>
<b>-Thread32First:</b> وظيفتها استقبال المعلومات الخاصة بأول thread من أي process موجودة في الsnapshot السابقة.
<br>
<b>-OpenThread:</b> بتدي للمالوير handle على الthread عشان يقدر يتعامل معاه.
<br>
<b>-SuspendThread:</b> من اسمها؛ بتعمل suspend لل thread.
<br>
<b>-GetThreadContext:</b> بتستقبل المحتوى او الcontext الخاص بالthread.
<br>
<b>-SetThreadContext:</b> بتعدل الinstruction pointer (زي EIP أو RIP ..الخ) وتخليه يشاور على الmalicious/shell code اللي هيتنفذ بعد كدة (يعني بيغير الExecution).
<br>
<b>-VirtualAllocEx:</b> هنا بيحدد المكان اللي هيكتب فيه الshellcode في الmemory.
<br>
<b>-WriteProcessMemory:</b> هنا بيكتب الshellcode في المكان اللي تم تحديده.
<br>
<b>-ResumeThread:</b> وهنا بيعمل resume لل thread بحيث يكمل شغله.
<br><br>
ال Functions السابقة مش شرط تتواجد في الكود بنفس الترتيب المكتوب فوق، أنا بوضح بس وجود واستخدام كل دالة منهم.
<br><br>
خليك فاكر ان فيه Functions calls ثابتة في كل عمليات الProcess Injection ولكن اللي بيميز الـThread Execution Hijacking هم:<br>
<b>OpenThread, SuspendThread, SetThreadContext, ResumeThread</b>
<br>
فلما تشوفهم بالترتيب اللي قلناه (Open,Suspend,Set,Resume) هتعرف تميز الTechnique دي عن غيرها، وفي الصور التالية هتلاقي مثال من IDA من عينة بتنفذ الطريقة دي وبعدها مثال لتنفيذ الطريقة بـ C++:
</span>
</p>

![](/assets/img/uploads/ar/process-injection-series/pi3-1.png)

![](/assets/img/uploads/ar/process-injection-series/pi3-2.png)

<p dir="rtl">
<span>
لاحظ ان الThread Execution Hijacking شبيه جدا بال Process Hollowing ولكن بإختلاف انه بيستهدف thread جوا process موجودة بالفعل (يعني مش بيضطر يعمل process جديدة في الsuspended state زي الProcess Hollowing) وده هنشرحه بالتفصيل في الProcess Hollowing بعد كدة.
<br><br>
<b>ملاحظة:</b> في المصادر فيه أمثلة من Malware حقيقية بتستخدم الطريقة زي Gazer التابع لـ Turla Group و RAT اسمه Karagany Trojan تابع لـ Dragonfly Group، بالإضافة لمصادر عن الـ Thread Execution Hijacking.
</span>
</p>

### Resources - مصادر

- [Gazer Report](https://www.welivesecurity.com/wp-content/uploads/2017/08/eset-gazer.pdf)

- [Karagany RAT](https://www.secureworks.com/research/updated-karagany-malware-targets-energy-sector)

- [Thread Execution Hijacking - MITRE ATT&CK](https://attack.mitre.org/techniques/T1055/003)

- [10 Process Injection Techniques - Elastic](https://www.elastic.co/blog/ten-process-injection-techniques-technical-survey-common-and-trending-process)

- [Process Injection Techniques - Cynet](https://www.cynet.com/attack-techniques-hands-on/process-injection-techniques)

- [Code Injection via Thread Hijacking with simple c++ malware - cocomelonc blog](https://cocomelonc.github.io/tutorial/2021/11/23/malware-injection-6.html)