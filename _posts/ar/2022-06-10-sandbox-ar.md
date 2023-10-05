---
date: 2022-06-10T13:00:00.000Z
layout: post
title: Sandbox Overview (Ar)
subtitle: Sandbox Overview (in Arabic)
description: >-
  An article about Sandboxes in Arabic Language
image: >-
  /assets/img/uploads/ar/sandbox-ar.png
optimized_image: >-
  /assets/img/uploads/ar/sandbox-ar.png
category: article
tags:
  - arabic
  - article
  - basics
  - malware_analysis
author: Abdullah Aiman
paginate: true
---
<h2 dir="rtl">
<span>
ايه هو الsandbox؟ انواعه؟ اهميته؟ ايه علاقته بالbehavior analysis؟
ليه مينفعش نعتمد على الsandboxes لوحدها في الanalysis؟
</span>
</h2>
<p dir="rtl">
<span>
خلينا نعرف الاول ان فيه طريقتين للBehavior Analysis:
</span>
</p>
<p dir="rtl">
<span>
1-أولاً Monitoring the Malware: وهنا بنراقب الmalware وسلوكه وهو شغال من حيث 4 محاور رئيسية وهم Process, File System, Registry, Network.
</span>
</p>
<p dir="rtl">
<span>
2-ثانياً Examining the system: وهنا بنشغل الmalware ولما يخلص بنشوف ايه التغييرات اللي حصلت في الsystem.
</span>
</p>

<p dir="rtl">
<span>
الـSandbox هو عبارة عن سيستم بياخد الـ Unknown sample اللي معانا يعملها Run جوا isolated lab ويراقبها ويشوف الbehavior بتاعها وفي الآخر بيطلعلنا Report فيه كل المعلومات اللي وصلّها بناءً على ال4 محاور الرئيسية (Processes - FileSystem - Registry - Network) ، دا غير انه بيدينا معلومات تانية بتفيدنا في الStatic Analysis، بالإضافة لل MITRE ATT&CK Techniques (هسيبلك لينك ليها في الآخر لو عايز تعرف هي ايه)
</span>
</p>
<p dir="rtl">
<span>
وطبعا يُعتبر الSandbox اسهل واسرع طريقة لعمل Dynamic/Behavioral Analysis.
</span>
</p>

<h2 dir="rtl">
<span>
أنواع الsandboxes بناءً على الArchitecture وازاي بتحاكي الBehavioral Analysis:
</span>
</h2>

<h3 dir="rtl">
<span>
1-النوع الأول Full-System Emulation:
</span>
</h3>
<p dir="rtl">
<span>
 بيحاكي كل حاجة في الphysical hardware زي المعالج والرام..الخ وبيقدملنا اكبر قدر من الرؤية على سلوك الmalware أو تأثيره، لكن ليه عيوب؛ فمثلاً: (بطيء جدا - محتاج ماشين قوية عشان تشغله - أصعب في التطوير وهيحتاج عدد مطورين أكبر)
</span>
</p>

<h3 dir="rtl">
<span>
2-النوع التاني Emulation of OS:
</span>
</h3>
<p dir="rtl">
<span>
المطورين اتجهو للخيار التاني وهو محاكاة نظام التشغيل او جزء منه بس، يعني مش هيعملو محاكاة للهاردوير او الماشين كلها..
وهنا لما بنشغل الـ Malware Sample جوا الـ Sandbox وتبدء تستعمل Windows APIs مثلا وتطلب منه يمسح File معين فاحنا اللي هنمسح الFile بدل الwindows وهنقدر نعمل Logging لل حصل،
</span>
</p>
<p dir="rtl">
<span>
من عيوبه انه متعب جدا في الـ development لاننا هنحتاج نهندل كل الWindows APIs ولو فيه API او Function نسيناها مثلا فهتبقى مشكلة لاننا مش هنشوف كل الMalware Behavior.
</span>
</p>

<h3 dir="rtl">
<span>
3-النوع التالت Virtualization:
</span>
</h3>
<p dir="rtl">
<span>
هنا هنستعمل Virtual Machine لتشغيل ومراقبة سلوك الMalware بعيداً عن مشاكل الأنواع السابقة عشان كدة أغلب الsandboxes بتستخدم النوع دا.
</span>
</p>

<p dir="rtl">
<span>
فيه Online Sandboxes مجانية بنرفع عليها الـ sample وهي تدينا report جاهز زي (VirusTotal و AnyRun و HybridAnalysis و JoeSandbox...)
</span>
</p>

<p dir="rtl">
<span>
وفيه Offline Sandboxes بنقدر نحطها على lab او machine عندنا زي (Cuckoo Sandbox و Cape Sandbox و Sandboxie...)
</span>
</p>

<h3 dir="rtl">
<span>
ليه مينفعش نعتمد على الSandbox لوحده في مرحلة الbehavior analysis؟
</span>
</h3>
<p dir="rtl">
<span>
لان الـsandboxes ليها عيوب وفيه Evasion Techniques بيستعملها الـMalware Authors لتخطّيها.
</span>
</p>

<p dir="rtl">
<span>
من أمثلة الـEvasion Techniques وهي ((Delaying Execution)) و فيها الmalware مش بيعمل الـMalicious Activity غير بعد مدة معينة من الوقت بعد ساعة او اتنين مثلا والمفروض ان الـSandbox بيشتغل دقيقة واتنين ويفصل فبالتالي مش هيكتشف الـMalicious Activity اللي في الـMalware.
</span>
</p>

<p dir="rtl">
<span>
مثال: لو الmalware بيستخدم sleep function عشان يشتغل بعد مدة او في ساعة معينة من اليوم فأغلب الsandboxes بتحاول تكتشف الsleep function دي لكن للاسف فيه طرق جديدة بيكتشفها ويستخدمها الmalware authors كل يوم بالتالي صعب على الsandboxes انها تكتشف كل الطرق ودا اللي بيخلينا منعتمدش عليها كتير في الanalysis.
</span>
</p>

<p dir="rtl">
<span>
مثال تاني: لما الmalware يكون مستني Network Packet من سيرفر الAttacker قبل ما يعمل اي Malicious Activity ومفيش اتصال بالNetwork وقتها مش هيطلع اي Malicious Activity بالتالي الSandbox مش هيكتشفه.
</span>
</p>

<p dir="rtl">
<span>
الخلاصة: نتعامل مع الSandbox انه Good Positive بمعنى اننا هنصدقه لما يقول ان الsample دي malicious بنسبة 10/10 مثلا، لكن لو قال انها benign مش malicious واداها مثلا 5/10 مش هنصدقه وهنتعامل على انها malicious لحد ما نعمل الanalysis ونتأكد بنفسنا.
</span>
</p>

### Online Sandboxes:

- [VirusTotal](http://virustotal.com/)

- [Any.Run](https://app.any.run/)

- [Hybrid Analysis](http://hybrid-analysis.com/)

- [Joe sandbox](http://joesandbox.com/)

### Offline Sandboxes:

- [Cuckoo sandbox](http://cuckoosandbox.org/)

- [CAPE sandbox v2](https://github.com/kevoreilly/CAPEv2)

- [Sandboxie](http://www.sandboxie.com/)

-[Buster Sandbox Analyzer](http://bsa.isoftware.nl/)

-

- [Mitre att&ck](http://attack.mitre.org/) 