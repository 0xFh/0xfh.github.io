---
date: 2024-05-01T13:00:00.000Z
layout: post
title: Evading Windows EvtLogs (Ar)
subtitle: Disable and Clear Windows Event Logs Techniques
description: >-
  Defense Evasion using disable\clear windows evtlogs techniques
image: >-
  /assets/img/uploads/ar/evading-logs-ar.png
optimized_image: >-
  /assets/img/uploads/ar/evading-logs-ar.png
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
مش دايماً الـ Logs هتوضحلك كل حاجة لأن الـ Malware تقدر تقفلها، أو تحذفها خالص. ودا هيخليها تعدي من برامج الأنتي فايرس والـ IPS وهتصعّب عمليات الـ Log Analysis والـ Investigation لأن وقتها مش هيكون فيه Logs أصلا. <br> 
<br>في المقال دا هتكلم عن تقنيتين 2 بتستخدمهم البرمجيات الخبيثة للتهرب من الدفاعات الأمنية وهم Disable Windows Event Logging و Clear Windows Event Logs ✍️
</span>
</p>

<p dir="rtl">
<span>
مبدئياً الـ Event Logs عبارة عن سجلات لأحداث النظام بتتكون من تنبيهات وإعلامات للمساعدة في تشخيص المشكلات والأخطاء الخاصة بالنظام والبرامج والتعريفات والسيكيوريتي وما إلى ذلك، الأحداث ممكن تكون أخطاء (Errors)، تحذيرات (Warnings)، معلومات (Information)، أو تدقيقات (Audit) (نجاح/فشل)... وتقدر توصلها من خلال أداة Event Viewer الأساسية في الويندوز أو من خلال الـ Command-Line من Powershell، وطبعا فيه أدوات خارجية ممكن تستعملها لقراءة وفحص وتحليل السجلات أو الأحداث دي.
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
أولاً: Disable Windows Event Logging
</span>
</p>

<p dir="rtl">
<span>
خدمة EventLog هي المسؤولة عن حفظ الـ Logs من كل مكونات النظام والتطبيقات اللي عليه، وبتبدأ بشكل تلقائي مع تشغيل النظام (في الـ startup services)، الخدمة دي مش بتجمع السجلات بشكل عشوائي ولكن فيه Audit Policy تابعة للـ Local Security Policy في النظام هي اللي بتحدد لخدمة الـ EventLog أنهي Logs بالظبط اللي هتجمعها، وطبعا إعدادت الـ Security Audit Policy ممكن يتم تغييرها من خلال أداة secpol.msc أو أمر auditpol.
<br><br>
التقنية دي ممكن تتنفذ بطريقتين: 1-إيقاف خدمة EventLog عن العمل بالتالي ميحصلش Logging لأي حاجة، 2-إيقاف/حذف الـ Audit Policy المسؤولة عن السجلات اللي بتجمعها... وكل دا ممكن يتم بالطرق التالية:
</span>
</p>

<p dir="rtl">
<span>
1-ممكن الـ Malware يستعمل الأوامر دي من الـ Command-line لإيقاف خدمة EventLog:<br>
<code>Set-Service -Name EventLog -Status Stopped</code>
<br>
أو
<br>
<code>sc config eventlog start=disabled</code>
<br>
وبعدهم أمر <code>Stop-Service -Name EventLog</code> لإيقاف تشغيل الخدمة.<br>
أو بأداة <b>wevtutil.exe</b> اللي ممكن تقفل أو تحذف سجلات محددة.<br>
<br>
2-من خلال تعديل قيمة "Start" في الـ RegKeys دي:<br>
- <code>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog</code>
<br>(مع إعادة التشغيل بعدها)<br>
<br>
- <code>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\Autologger\EventLog-Security</code>
<br>(مع إعادة التشغيل بعدها، وبدون الحاجة لصلاحيات المسؤول)<br>
<br>
- <code>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\Autologger\EventLog-Application</code>
<br>أو<br>
- <code>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\Autologger\EventLog-System</code>
<br>(باستخدام صلاحيات المسؤول)<br>
<br>
- أو بإضافة الـkey التالي لمنع توليد الـ Security EventLog 4657 أو Sysmon EventLog 13 اللي بيسجّلو أي تغييرات في الـ Registry:<br>
<code>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\MiniNt</code><br><br>

3-بما إن فيه Audit Policy مسؤولة عن السجلات اللي هيتم تجميعها فممكن الـ Malware يوقّف الـ Auditing لأحداث معينة باستخدام أداة <b>auditpol.exe</b> في الويندوز، فمثلاً عشان يوقّف الـ auditing الخاص بالـ Account Logon ممكن يستخدم الأمر التالي:<br>
<code>auditpol /set /category:"Account Logon" /success:disable /failure:disable</code>
<br>
<br>
4-وممكن يحذف الـ Audit Policy من الأساس باستخدام أوامر:<br>
<code>auditpol /clear /y</code>
<br>أو<br>
<code>auditpol /remove /allusers</code>
<br>
<br>
التقنية دي تم استخدامها في هجمات مختلفة قبل كدة فمثلاً استخدمها APT29 أثناء اختراقات شركة SolarWinds عشان يمنع جمع الـ Audit Logs، واستخدمها فريق Sandworm أثناء الهجمات على قطاع الطاقة في أوكرانيا سنة 2016 عشان يقفل الـ Event Logging على الأجهزة المُخترقة، وغيرهم.
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
ثانياً: Clear Windows Event Logs
</span>
</p>

<p dir="rtl">
<span>
هنا الـ Malware بيحذف الـ Event Logs عشان يخفي الأنشطة اللي عملها، ويقدر يعمل كدة بكذا طريقة من ضمنهم:<br>
1-بصلاحيات مسؤول (Admin) وباستخدام أمر أداة wevtutil؛ فمثلاً ممكن يستخدم أمر <code>wevtutl cl system</code> لحذف سجلات النظام، أو <code>wevtutil cl application</code> لحذف سجلات التطبيقات، و <code>wevtutil cl security</code> لحذف سجلات الأمان.<br>
<br>
2-من خلال واجهة المُستخدم من أداة Event Viewer.<br>
<br>
3-من خلال الـ Powershell باستخدام أمر <code>Remove-EventLog -LogName Security</code> لو هيحذف سجلات الأمان مثلاً.<br>
<br>
4-من خلال حذف ملفات الـ Event Logs بشكل مباشر من مكان تخزينها في النظام <code>C:\Windows\System32\winevt\logs\</code>
<br>
<br>
التقنية دي استخدمها <b>BlackCat Ransomware</b> في حذف الـ Event Logs من خلال أمر:<br>
<code>“C:\Windows\system32\cmd.exe” /c “cmd.exe /c  for /F \”tokens=*\” %1 in (‘wevtutil.exe el’) DO wevtutil.exe cl \”%1\”</code><br>
<br>واستخدمها FinFisher من خلال APIs زي <code>OpenEventLog</code> و <code>ClearEventLog</code>
<br><br>واستخدمها كذا APT و Malware قبل كدة عموماً زي gh0st RAT و ZxShell و NotPetya و Olympic Destroyer و Lucifer وغيرهم.
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
الـ Mitigations للتقنيتين قريبين من بعض ونقدر نلخصهم في الآتي:
</span>
</p>
<p dir="rtl">
<span>
1-المراجعة الدورية لإعدادات حسابات المسؤولين، والتأكد من أذونات المستخدمين.<br>
2-التأكد من تشغيل خدمة EventLog والعمليات اللي بتعتمد عليها بشكل صحيح.<br>
2-التأكد من أذونات وصلاحيات الوصول للملفات والعمليات على النظام، والتأكد من الوصول المحدود لملفات .evtx وعدم التعديل عليها.<br>
3-التأكد من أذونات الوصول أو التعديل على الـ Registry خصوصاً الـ Keys اللي ذكرناها.<br>
4-تشفير أو تعمية الـ Event Files أثناء حفظها أو نقلها.<br>
5-تخزين الـ Event Logs بشكل تلقائي على cloud أو مكان آمن بعيد عن النظام المستخدم.<br>
6-التأكد من إضافة الأذونات وطرق المصادقة اللازمة للتعامل مع الـ Event Files.<br>
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
عشان نعمل Detection للاتنين ممكن نستخدم الطرق دي:
</span>
</p>

<p dir="rtl">
<span>
1-مراقبة تطبيقات وخدمات الطرف الثالث اللي ممكن تعمل Logging بديلاً عن خدمات النظام الأساسية.<br>
2-مراقبة الأوامر الممكن تنفيذها لقفل عملية الـ Logging زي auditpol و Wevtutil و sc والـ Tools اللي زي Mimikatz وهكذا.<br>
3-مراقبة العمليات والـ scripts اللي بيتم تنفيذها.<br>
4-مراقبة الـ Registry Keys المسؤولة عن تعطيل خدمة الـ EventLog واللي ذكرناها مسبقاً.<br>
5-مراقبة عمليات المسح لملفات الـ Event Logs لكشفها في الحال.<br>
6-مراقبة الـ API Calls الخاصة بالنظام واللي لها القدرة على حذف الـ Event Logs.<br>
</span>
</p>

### Resources - مصادر

- [Audit Policy - Microsoft](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/audit-policy)

- [Disable Windows Event Logging - Defender BlueBook Blog](https://ptylu.github.io/content/report/report.html?report=25)

- [Defense Evasion: Windows Event Logging](https://www.hackingarticles.in/defense-evasion-windows-event-logging-t1562-002/)

- [BlackCat Ransomware](https://www.microsoft.com/en-us/security/blog/2022/06/13/the-many-lives-of-blackcat-ransomware/)

- [FinFisher](https://cloudblogs.microsoft.com/microsoftsecure/2018/03/01/finfisher-exposed-a-researchers-tale-of-defeating-traps-tricks-and-complex-virtual-machines/)

- [Lucifer - Unit42](https://unit42.paloaltonetworks.com/lucifer-new-cryptojacking-and-ddos-hybrid-malware/)

- [NotPetya](https://blog.talosintelligence.com/2017/06/worldwide-ransomware-variant.html)

- [ZxShell](https://blogs.cisco.com/security/talos/opening-zxshell)

- [gh0st RAT - FireEye Threat Intelligence](https://cloud.google.com/blog/topics/threat-intelligence/demonstrating-hustle/)
