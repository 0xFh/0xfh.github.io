---
date: 2024-01-21T13:00:00.000Z
layout: post
title: imphash & sechash (Ar)
subtitle: using imphash & sechash
description: >-
  A small article about linking malware to their adversaries using imphash and sechash
image: >-
  /assets/img/uploads/ar/ishash-ar.jpg
optimized_image: >-
  /assets/img/uploads/ar/ishash-ar.jpg
category: article
tags:
  - arabic
  - article
  - malware_analysis
  - threat_hunting
author: Abdullah Aiman
paginate: true
---

<p dir="rtl" style="font-weight:600">
<span>
من ضمن المؤشرات اللي بتستخدمها الشركات في تتبع Threat Actor أو Adversary معين أثناء عمليات الـ Static Analysis هي الـ imphash و sechash عشان كدة هتلاقي موقع زي VirusTotal مثلاً بيعرضلك معلوماتهم في تفاصيل الفحص.. ولكن إزاي بيتم إستخدامهم في الـ Static Analysis و إيه هم أصلاً؟
</span>
</p>

<p dir="rtl">
<span>
مبدئياً الـ Threat Actors احيانا بيستخدمو نفس الكود والـ functions مع أكتر من Malware بدل ما يعملو مالوير جديد من الصفر ولكن مع تغييرات بسيطة فيه عشان يظهر كأنه مالوير جديد ويتخطى الحلول الأمنية اللي كشفته قبل كدة... 
</span>
</p>

<p dir="rtl">
<span>
أولاً imphash موجود في python module اسمه pefile من تنفيذ شركة FireEye وهو اختصار لـ Import Hash ودا عبارة عن hash خاص بالـ IAT أو Import Address Table، لو متعرفش الـ IAT فـ دا جدول فيه أسماء الـ DLLs والـ Functions اللي المالوير بيعملها import عشان يستخدمها.. الـ IAT ده بيتعمل تلقائياً من الـ Linker أثناء عملية الـ Compilation للكود بعد ما الـ Linker يشوف ترتيب الـ Functions المكتوبة الأول.. والـ imphash بيتحسب بعد كدة بناءً على محتويات الجدول اللي اتعمل.
</span>
</p>

<p dir="rtl">
<span>
اللي بيحصل هنا ان الـ Malware Author لو استخدم نفس الكود في مالوير أو نسخة جديدة منه هيكون الـ imphash هو نفسه الموجود في النسخة القديمة متغيرش ودا في حالة انه مغيرش في ترتيب الـ Functions مثلاً... بالتالي هيبقى سهل علينا نحدد إن العينة الجديدة دي متطابقة مع واحدة قديمة ومرتبطة بنفس الـ Threat Actor أو نفس الـ Malware Family بتاعت المالوير القديم .. ودا هيسهل شوية عملية تحليل أو تصنيف المالوير بالنسبة للمؤسسة.
</span>
</p>

<p dir="rtl">
<span>
بالعكس بردو لو ترتيب الـ Functions جوا المالوير اتغير مع الحفاظ على نفس الكود بالتالي هتلاقي الـ imphash اتغير واختلف.. فوقتها مش شرط يكون المالوير جديد او تبع threat actor مختلف لكن بشوية تحليل أكتر هنقدر نوصل للإختلاف في الـ IAT ونعرف ان المالوير دا هو نفس المالوير السابق لكن مع بعض التعديلات اللي أدت ان الهاش يكون مختلف.
</span>
</p>

<p dir="rtl">
<span>
في الصورة هتلاقي مثال للـ imphash قبل و بعد التعديل على ترتيب الـ Functions في الكود، هو نفس الكود لكن ترتيبهم اختلف بالتالي الهاش اختلف:
</span>
</p>

![](/assets/img/uploads/i-s-hash/1.png)

<p dir="rtl">
<span>
فيه ملاحظة مهمة هنا: الـ Packers ممكن تستخدم نفس الـ IAT للبرمجيات اللي بتعملها Packing بالتالي البرمجيات دي ممكن يكون ليها نفس الـ imphash بسبب استخدام نفس الـ packer بالتالي دا مش معناه انهم تابعين لنفس الـ Adversary ووقتها ممكن نحاول نعمل Unpacking الأول.
</span>
</p>

<p dir="rtl">
<span>
ثانياً بالنسبة للـ sechash دا اختصار لـ Section Hash و زي ماحنا عارفين أي PE File بيحتوي على Sections بيكون فيها الكود التنفيذي للملف والداتا والموارد بتاعته زي سكاشن .text و .data و .rsrc وغيرهم، الـ sechash عبارة عن الهاش الخاص بكل سكشن لوحده... وهنا بيتم التحقق من الـ hashes دي ومطابقتها ب sechashes لـ malware samples تانية بحيث لو فيه عينتين متطابقتين هيبقى فيه احتمال لتبعيتهم لنفس الـ Threat Actor.
</span>
</p>

<p dir="rtl">
<span>
عشان تشوف الـ imphash لأي PE File ممكن تستخدم VT علطول، وممكن تستخدم اداة pehash أو تستخدم الـ pefile module الخاص ببايثون، اما بالنسبة للـ section hashs فـ دي هتلاقيها في VT وبرامج تانية كتير زي PeStudio وغيره.
</span>
</p>

<p dir="rtl">
<span>
في الصورة الجاية هتلاقي مثال من VirusTotal للـ imphash و sechashes:
</span>
</p>

![](/assets/img/uploads/i-s-hash/2.png)

<p dir="rtl">
<span>
وفي النهاية هنقدر نربط بين كل العينات المتطابقة والـ malware families او الـ threat actors اللي عملوها !
</span>
</p>

### Resources

- [PE File Format - EN](https://0xfh.github.io/pe-file-format/)

- [PE File Format P1 - AR](https://0xbatx.blogspot.com/2022/06/PEFile1.html)

- [PE File Format P2 - AR](https://0xbatx.blogspot.com/2022/06/PEFile2.html)

- [Packing & Unpacking - AR](https://0xbatx.blogspot.com/2022/06/Packing.html)
