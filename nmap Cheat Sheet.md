# 🛰️ Nmap Cheat Sheet – شرح كامل بالعربي

> **Nmap** أداة قوية لفحص الشبكات (Network Scanning) ومعرفة:
> 
> - الأجهزة المتصلة
>     
> - البورتات المفتوحة
>     
> - الخدمات والإصدارات
>     
> - أنظمة التشغيل
>     
> - إعدادات الحماية والجدران النارية
>     

---

## 🧩 الصيغة العامة لأمر Nmap

```bash
nmap [Scan Type] [Options] {Target}
```

**مثال:**

```bash
nmap -sS -p 80,443 192.168.1.1
```

---

## 🎯 تحديد الهدف (Target Specification)

```bash
nmap 192.168.1.1              # فحص IP واحد
nmap 192.168.1.1 192.168.1.2  # فحص أكثر من IP
nmap 192.168.1.1-100          # فحص رينج
nmap 192.168.1.0/24           # فحص شبكة باستخدام CIDR
nmap example.com              # فحص دومين
nmap -iL targets.txt          # فحص Targets من ملف
nmap --exclude 192.168.1.5    # استثناء IP معين
```

---

## 🔢 تحديد البورتات (Port Specification)

```bash
-p 22              # فحص بورت محدد
-p 22-100          # فحص رينج بورتات
-p 80,443          # فحص بورتات متعددة
-p-                # فحص كل البورتات (1–65535)
-F                 # فحص سريع (Top Ports)
--top-ports 1000   # أشهر 1000 بورت
-p http,https      # فحص حسب اسم الخدمة
```

---

## 🔍 أنواع الفحص (Scanning Types)

### 🟢 TCP Scans

```bash
-sS   # SYN Scan (سريع وشائع – Stealth)
-sT   # TCP Connect Scan
-sA   # ACK Scan (لتحديد الفايروول)
-sX   # Xmas Scan
-sF   # FIN Scan
```

### 🔵 UDP Scan

```bash
-sU   # فحص UDP (بطيء)
```

### 📡 Discovery Scans

```bash
-sP   # Ping Scan فقط (قديم)
-sn   # Ping Scan فقط (حديث)
-sL   # List Scan بدون فحص
-sI   # Idle Scan
-sO   # IP Protocol Scan
```

---

## 🧭 اكتشاف الأجهزة (Host Discovery)

```bash
-sn   # بدون فحص بورتات
-Pn   # تجاهل Ping (مفيد مع Firewall)
-PS   # TCP SYN Ping
-PA   # TCP ACK Ping
-PU   # UDP Ping
-PR   # ARP Scan (داخل الشبكة)
-n    # بدون DNS Resolution
```

---

## 🧪 كشف الإصدارات والخدمات (Version Detection)

```bash
-sV                     # معرفة نسخة الخدمة
--version-intensity 9   # قوة الفحص (0–9)
--version-light         # فحص خفيف
--version-all           # فحص كامل
-A                      # OS + Version + Scripts + Traceroute (enum)
-O                      # كشف نظام التشغيل
```

---

## 📜 استخدام سكربتات Nmap (NSE)

```bash
--script default              # سكربتات افتراضية آمنة
--script vuln                 # فحص ثغرات
--script=http-*               # سكربتات HTTP
--script-help=script-name     # مساعدة سكربت
--script-updatedb             # تحديث السكربتات
```

**مثال:**

```bash
nmap --script vuln 192.168.1.1
```

---

## 🧱 تجاوز الجدران النارية (Firewall Evasion)

```bash
-f                    # Fragment Packets
--mtu 24              # تحديد MTU
--source-port 53      # تغيير Source Port
--data-length 20      # إضافة Data عشوائية
--randomize-hosts     # ترتيب عشوائي
--badsum              # Bad Checksum
```

---

## ⏱️ التحكم في سرعة الفحص (Timing Options)

```bash
-T0   # بطيء جدًا (Stealth)
-T1   # بطيء
-T2   # متوسط
-T3   # افتراضي
-T4   # سريع
-T5   # سريع جدًا (خطر)
```

---

## 📤 حفظ النتائج (Output Formats)

```bash
-oN scan.txt   # Normal
-oX scan.xml   # XML
-oG scan.grep  # Grepable
-oA scan       # كل الصيغ
```

---

## 🧰 أوامر مفيدة إضافية

```bash
-6             # فحص IPv6
--open         # عرض البورتات المفتوحة فقط
--traceroute   # تتبع المسار
--proxies      # استخدام Proxy
```

---

## ✅ مثال عملي شامل

```bash
nmap -sS -sV -A -T4 -p- 192.168.1.1
```

**النتيجة:**

- 🔹 فحص كامل
    
- 🔹 كل البورتات
    
- 🔹 كشف خدمات + OS
    
- 🔹 سرعة عالية
