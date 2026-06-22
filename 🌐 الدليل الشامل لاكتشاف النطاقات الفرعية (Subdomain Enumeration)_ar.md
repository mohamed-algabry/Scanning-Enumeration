# 🌐 الدليل الشامل لاكتشاف النطاقات الفرعية (Subdomain Enumeration)

## مرجع احترافي للـ Bug Bounty واختبارات الاختراق

> **الإصدار:** 2026
>
> تم إعداد هذا الدليل لصائدي الثغرات (Bug Bounty Hunters) ومختبري الاختراق (Pentesters) وفرق Red Team.
>
> متوافق مع GitHub Markdown و Obsidian.

---

# 📖 فهرس المحتويات

1. 📝 المقدمة
2. 🗺️ منهجية الـ Enumeration
3. 🔍 الاستكشاف السلبي للـ Subdomains
4. ⚡ الاستكشاف النشط للـ Subdomains
5. 🌐 التحقق من سجلات DNS
6. 🚀 اكتشاف الأنظمة والخدمات الحية
7. 📸 الاستطلاع البصري وأخذ اللقطات
8. 📚 الاستطلاع عبر الأرشيفات والمصادر التاريخية
9. 🏗️ تحليل البنية التحتية
10. 🤖 بناء Pipelines احترافية للأتمتة
11. ⚠️ الأخطاء الشائعة
12. 🎯 سير العمل الاحترافي للـ Bug Bounty

---

# 🎯 المقدمة

تُعد عملية **Subdomain Enumeration** من أهم مراحل الاستطلاع (Reconnaissance)، وتهدف إلى اكتشاف جميع النطاقات الفرعية المرتبطة بالدومين المستهدف.

## الأهداف الرئيسية

* 🎯 توسيع سطح الهجوم (Attack Surface)
* 🔍 اكتشاف الأصول والخدمات المنسية
* 🧪 العثور على بيئات الاختبار (Staging Environments)
* 👨‍💻 اكتشاف أنظمة التطوير (Development Systems)
* 🕵️‍♂️ الوصول إلى التطبيقات والخدمات المخفية
* 🏆 زيادة فرص العثور على ثغرات ضمن برامج الـ Bug Bounty

### مثال

الدومين المستهدف:

```text
target.com
```

قد يتم اكتشاف:

```text
api.target.com
dev.target.com
admin.target.com
vpn.target.com
staging.target.com
```

---

# 🗺️ منهجية الـ Enumeration

يعتمد المحترفون على منهجية واضحة للوصول إلى أفضل النتائج:

1. الاستكشاف السلبي (Passive Enumeration)
2. الاستكشاف النشط (Active Enumeration)
3. التحقق من سجلات DNS
4. اكتشاف الخدمات الحية
5. التعرف على التقنيات المستخدمة
6. اكتشاف المحتوى والمسارات
7. البحث عن الثغرات

### مخطط العمل

```text
Passive → Active → Resolve → Live → Fingerprint → Hunt
```

---

# 🔍 الاستكشاف السلبي للـ Subdomains

في هذه المرحلة لا يتم إرسال أي طلبات مباشرة إلى البنية التحتية الخاصة بالهدف.

### المميزات

* آمن
* سريع
* قليل الضوضاء
* يصعب اكتشافه

---

## 🔹 Subfinder

الأداة المفضلة لدى الكثير من الباحثين الأمنيين.

### التثبيت

```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

### الاستخدام الأساسي

```bash
subfinder -d target.com
```

### أهم الخيارات

```bash
-all
-recursive
-silent
-o output.txt
-timeout 30
```

### مثال احترافي

```bash
subfinder -d target.com -all -recursive -silent
```

---

## 🔹 Assetfinder

أداة خفيفة وسريعة لجمع النطاقات الفرعية.

```bash
assetfinder --subs-only target.com
```

حفظ النتائج:

```bash
assetfinder --subs-only target.com > assetfinder.txt
```

---

## 🔹 Findomain

```bash
findomain -t target.com
```

حفظ النتائج:

```bash
findomain -t target.com -u output.txt
```

---

## 🔹 Amass (Passive Mode)

```bash
amass enum -passive -d target.com
```

مع عرض مصدر كل نتيجة:

```bash
amass enum -passive -src -d target.com
```

---

# 🏆 جمع البيانات من شهادات SSL

---

## 🔹 crt.sh

يستخرج النطاقات الفرعية من شهادات SSL العامة.

يدويًا:

```text
https://crt.sh/?q=%25.target.com
```

باستخدام curl:

```bash
curl -s "https://crt.sh/?q=%25.target.com&output=json"
```

استخراج النطاقات فقط:

```bash
curl -s "https://crt.sh/?q=%25.target.com&output=json" | jq -r '.[].name_value'
```

---

## 🔹 Chaos Dataset

قاعدة بيانات ضخمة مقدمة من ProjectDiscovery.

```bash
chaos -d target.com
```

---

# ⚡ الاستكشاف النشط للـ Subdomains

في هذه المرحلة يتم التفاعل مباشرة مع خوادم DNS الخاصة بالهدف.

---

## 🔹 Amass Active

```bash
amass enum -active -d target.com
```

وضع أكثر شراسة:

```bash
amass enum -active -brute -d target.com
```

---

## 🔹 Shuffledns

تنفيذ DNS Bruteforce.

```bash
shuffledns -d target.com -w words.txt -r resolvers.txt
```

باستخدام قوائم SecLists:

```bash
shuffledns -d target.com -w subdomains-top1million-5000.txt -r resolvers.txt
```

---

## 🔹 Gobuster DNS

```bash
gobuster dns -d target.com -w words.txt
```

زيادة عدد الـ Threads:

```bash
gobuster dns -d target.com -w words.txt -t 100
```

---

# 🌐 التحقق من سجلات DNS

ليس كل Subdomain يتم اكتشافه يكون صالحًا أو موجودًا فعليًا.

---

## 🔹 DNSX

فحص السجلات:

```bash
cat subs.txt | dnsx
```

عرض النتائج بالتفصيل:

```bash
cat subs.txt | dnsx -resp
```

فحص A Records:

```bash
cat subs.txt | dnsx -a -resp
```

---

# 🚀 اكتشاف الخدمات الحية

الكثير من النطاقات الفرعية لا تستضيف خدمات فعلية.

---

## 🔹 HTTPX

الاستخدام الأساسي:

```bash
cat resolved.txt | httpx
```

الاستخدام الاحترافي:

```bash
cat resolved.txt | httpx \
-title \
-tech-detect \
-status-code \
-follow-redirects \
-silent
```

### أهم المميزات

* اكتشاف Status Codes
* اكتشاف التقنيات المستخدمة
* اكتشاف CDN
* استخراج عنوان الصفحة
* تتبع التحويلات

---

# 🔬 التعرف على التقنيات المستخدمة

يمكن استخدام HTTPX لمعرفة التقنيات المستخدمة.

```bash
httpx -tech-detect
```

أمثلة:

* Nginx
* Apache
* IIS
* Cloudflare
* WordPress
* React
* NextJS

---

# 📸 الاستطلاع البصري وأخذ اللقطات

مراجعة الصور تساعد على اكتشاف لوحات الإدارة والأنظمة بسرعة.

---

## 🔹 Gowitness

```bash
gowitness file -f live.txt
```

المميزات:

* أخذ Screenshots
* إنشاء تقرير HTML
* تسريع المراجعة اليدوية

---

## 🔹 Eyewitness

```bash
eyewitness --web -f live.txt
```

يوفر:

* لقطات شاشة
* تصنيف المواقع
* تقارير مفصلة

---

# 📚 الاستطلاع عبر الأرشيفات

البيانات التاريخية قد تكشف أصولًا لم تعد ظاهرة حاليًا.

---

## 🔹 GAU

جمع الروابط المؤرشفة.

```bash
gau target.com
```

حفظ النتائج:

```bash
gau target.com > gau.txt
```

---

## 🔹 Waymore

```bash
waymore -i target.com
```

يستخدم من أجل:

* الروابط التاريخية
* الأصول القديمة
* البارامترات المخفية

---

# 🏗️ تحليل البنية التحتية

بعد جمع النطاقات الفرعية يتم تحليل البنية التحتية.

استخراج الـ IPs:

```bash
cat subs.txt | dnsx -resp
```

الحصول على الـ IP فقط:

```bash
cat subs.txt | dnsx -resp-only
```

فحص ASN:

```bash
amass intel -asn
```

الأهداف:

* اكتشاف أصول Cloud
* معرفة مزودي الخدمة
* العثور على خوادم منسية

---

# 🤖 سير عمل احترافي

### الخطوة الأولى

```bash
subfinder -d target.com -all -silent
```

### الخطوة الثانية

```bash
assetfinder --subs-only target.com
```

### الخطوة الثالثة

```bash
amass enum -passive -d target.com
```

### الخطوة الرابعة

```bash
sort -u
```

### الخطوة الخامسة

```bash
dnsx
```

### الخطوة السادسة

```bash
httpx
```

### الخطوة السابعة

```bash
gowitness
```

---

# 🏆 Pipeline احترافي كامل

```bash
subfinder -d target.com -all -recursive -silent > subs.txt

assetfinder --subs-only target.com >> subs.txt

amass enum -passive -d target.com >> subs.txt

sort -u subs.txt > unique.txt

cat unique.txt | dnsx -silent > resolved.txt

cat resolved.txt | httpx \
-title \
-tech-detect \
-status-code \
-silent > live.txt

gowitness file -f live.txt
```

---

# ⚠️ الأخطاء الشائعة

1. الاعتماد على أداة واحدة فقط
2. تجاهل التحقق من DNS
3. تجاهل البيانات المؤرشفة
4. عدم مراجعة Screenshots
5. نسيان Recursive Enumeration
6. تجاهل تحليل ASN
7. عدم إزالة النتائج المكررة

---

# 🎯 سير العمل الاحترافي للـ Bug Bounty

## مرحلة Recon

```bash
subfinder
assetfinder
amass
chaos
```

## التحقق

```bash
dnsx
```

## اكتشاف الخدمات الحية

```bash
httpx
```

## المراجعة البصرية

```bash
gowitness
```

## اكتشاف المحتوى

```bash
katana
ffuf
```

## البحث عن الثغرات

```bash
nuclei
manual testing
```

---

# 📚 الأدوات الموصى بها

| الأداة      | الاستخدام            |
| ----------- | -------------------- |
| Subfinder   | Passive Enumeration  |
| Assetfinder | اكتشاف سريع          |
| Amass       | Enumeration متقدم    |
| Chaos       | قواعد بيانات جاهزة   |
| DNSX        | التحقق من DNS        |
| HTTPX       | اكتشاف الخدمات الحية |
| Shuffledns  | DNS Bruteforce       |
| Gau         | الروابط المؤرشفة     |
| Waymore     | البيانات التاريخية   |
| Gowitness   | Screenshots          |
| Katana      | Crawling             |
| Nuclei      | فحص الثغرات          |

---

# 🏁 ملاحظات أخيرة

* لا تعتمد على أداة واحدة فقط.
* اجمع النتائج من أكثر من مصدر.
* قم بإزالة التكرارات باستمرار.
* جودة الـ Enumeration تؤثر بشكل مباشر على نجاحك في الـ Bug Bounty.
* كل Subdomain جديد قد يكون نقطة دخول لثغرة مهمة.
