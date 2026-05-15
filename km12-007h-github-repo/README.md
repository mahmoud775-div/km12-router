# OpenWrt Builder for KT GiGA WiFi home ax (KM12-007H)

🤖 **مستودع جاهز للبناء التلقائي عبر GitHub Actions**

هذا المستودع يبني فيرموير OpenWrt مخصّص لراوتر KM12-007H **بدون أي معدّات**.
كل ما تحتاجه هو حساب GitHub مجاني.

---

## 🚀 طريقة الاستخدام (3 خطوات فقط)

### الخطوة 1: انشئ المستودع على GitHub

#### الطريقة الأ (سهلة):
1. اذهب إلى https://github.com/new
2. اسم المستودع: `openwrt-km12-007h` (أو أي اسم)
3. اجعله **Public** (مهم - الـ Actions مجاني للمستودعات العامة)
4. اضغط **Create repository**

### الخطوة 2: ارفع الملفات

من جهازك (Windows/Mac/Linux):

#### الطريقة الأولى (الأسهل - عبر متصفح):

1. في مستودعك الجديد، اضغط **uploading an existing file**
2. اسحب وأفلت **كل** الملفات من هذا المجلد (`km12-007h-github-repo`)
3. اضغط **Commit changes**

#### الطريقة الثانية (عبر git):

```bash
cd km12-007h-github-repo
git init
git add .
git commit -m "Initial: KM12-007H build setup"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/openwrt-km12-007h.git
git push -u origin main
```

### الخطوة 3: شغّل البناء

1. اذهب إلى تبويب **Actions** في مستودعك
2. اضغط على **Build OpenWrt for KM12-007H** (يساراً)
3. اضغط الزر الأخضر **Run workflow** (يميناً)
4. اختر الفرع: `openwrt-24.10` (موصى به)
5. اضغط الزر الأخضر **Run workflow** مرة أخرى

البناء سيستغرق **1-2 ساعة**. يمكنك إغلاق المتصفح والعودة لاحقاً.

### الخطوة 4: تنزيل الفيرموير

عند اكتمال البناء (✅ علامة خضراء):

1. اضغط على البناء المكتمل
2. مرّر للأسفل إلى قسم **Artifacts**
3. حمّل ملف `openwrt-km12-007h-...zip`
4. فك الضغط لتحصل على:
   - `openwrt-...-mercury_km12-007h-squashfs-factory.bin` ← للتومّض الأول عبر Breed
   - `openwrt-...-mercury_km12-007h-squashfs-sysupgrade.bin` ← للتحديث لاحقاً
   - `openwrt-...-mercury_km12-007h-initramfs-kernel.bin` ← للاختبار بدون كتابة

---

## ⚠️ تنبيهات مهمة قبل التومّض

### قبل أي شيء، يجب أن:

1. **تتأكد أن Breed bootloader يعمل**:
   - افصل الكهرباء
   - اضغط زر Reset
   - شغّل الكهرباء وأنت تضغط
   - استمر 10 ثوانٍ
   - افتح `http://192.168.1.1` على المتصفح
   - إذا ظهرت واجهة Breed = ✅ آمن للتومّض
   - إذا لم تظهر = **لا تومّض، الراوتر قد يصبح brick دائم**

2. **خذ نسخة احتياطية من Breed** قبل أي شيء:
   - في واجهة Breed → 备份固件 / Backup Firmware
   - اختر "Full Backup" (نسخة كاملة من الـ NAND)
   - احفظ الملف في مكان آمن

### للتومّض:

اقرأ ملف `FLASHING_INSTRUCTIONS.md` من الحزمة الأصلية بالكامل.

---

## 🔧 الملفات في هذا المستودع

```
.
├── .github/
│   └── workflows/
│       └── build.yml           ← الـ workflow الذي يبني الفيرموير تلقائياً
│
├── files/
│   └── target/linux/ramips/
│       ├── dts/
│       │   └── mt7621_mercury_km12-007h.dts   ← Device Tree المخصّص
│       └── image/
│           └── mt7621.mk.snippet               ← تعريف image build
│
└── README.md                   ← هذا الملف
```

---

## 🎯 إذا فشل البناء

GitHub Actions يحفظ logs عند الفشل. لتنزيلها:

1. اضغط على البناء الفاشل
2. مرّر للأسفل إلى **Artifacts**
3. حمّل `build-logs-*.zip`
4. افحص `logs/` للبحث عن الخطأ

شارك معي الخطأ وسأساعدك في حلّه.

---

## 💡 تخصيصات

### تغيير الحزم المُثبّتة

عدّل ملف `.github/workflows/build.yml` في قسم `Create .config`.

أضف:
```yaml
CONFIG_PACKAGE_openvpn-openssl=y
CONFIG_PACKAGE_adguardhome=y
# إلخ
```

### بناء فرع مختلف

عند تشغيل workflow، اختر:
- `openwrt-24.10` ← مستقر (موصى به)
- `openwrt-23.05` ← مستقر قديم
- `main` ← snapshot، أحدث ميزات لكن قد يكون به أخطاء

---

## 📊 ما اختُبر؟

| العنصر | الحالة |
|---|---|
| DTS file syntax | ✅ تحقّق Claude من صحته (defconfig قبله) |
| Device recognition | ✅ يظهر في `make menuconfig` |
| Build process | ⏳ ينتظر أول تشغيل في GitHub Actions |
| Flash & boot | ⏳ ينتظر اختبار على جهاز فعلي |

**هذا port غير مُختبر.** أنت أول من سيشغّله.

---

## 📞 موارد المساعدة

- منتدى OpenWrt: https://forum.openwrt.org/
- توثيق MT7621: https://openwrt.org/docs/techref/targets/ramips/mt7621
- مدوّنة كورية متخصصة في KT routers: https://manatails.net/blog/

---

## 📜 الترخيص

- DTS files: GPL-2.0-OR-LATER OR MIT
- Workflow & docs: MIT
