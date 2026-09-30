# سیستم جستجوی رمزنگاری هومورفیک (FHE Search System)

![Alt text](image.jpg)
یک موتور جستجوی پیشرفته و چندزبانه با قابلیت حفظ حریم خصوصی که از تکنیک‌های رمزنگاری هومورفیک کامل (FHE) و پردازش زبان طبیعی استفاده می‌کند.

## ویژگی‌های کلیدی

🔐 **رمزنگاری هومورفیک**: امکان جستجو روی داده‌های رمزشده بدون نیاز به رمزگشایی  
🌍 **پشتیبانی چندزبانه**: فارسی، عربی، انگلیسی و متون مختلط  
🔍 **جستجوی معنایی**: استفاده از TF-IDF برای یافتن محتوای مرتبط  
📊 **خوشه‌بندی هوشمند**: گروه‌بندی خودکار اسناد مشابه  
💾 **ذخیره‌سازی پایدار**: پایگاه داده SQLite برای نگهداری اطلاعات  
⚡ **عملکرد بالا**: پردازش دسته‌ای و بهینه‌سازی سرعت  
📈 **آنالیتیک کامل**: آمارگیری و نظارت بر عملکرد سیستم

## نصب و راه‌اندازی

### پیش‌نیازها

- Python 3.8 یا بالاتر
- حداقل 4GB RAM
- فضای ذخیره‌سازی: 1GB

### نصب وابستگی‌ها

```bash
pip install -r requirements.txt
```

### راه‌اندازی سریع

```python
from fhe_search import FHESearchEngine

# ایجاد نمونه سیستم
engine = FHESearchEngine()

# افزودن اسناد
documents = [
    "یادگیری ماشین شاخه مهمی از هوش مصنوعی است",
    "Machine learning is a subset of artificial intelligence",
    "امنیت داده‌ها در فضای ابری اهمیت دارد"
]

result = engine.add_documents(documents)
print(f"✅ {result['processed_documents']} سند پردازش شد")

# جستجو
search_result = engine.search("هوش مصنوعی", top_k=5)
for i, result in enumerate(search_result['results'], 1):
    score = result['similarity_score']
    text = result['metadata']['original_text']
    print(f"{i}. امتیاز: {score:.3f} - {text}")
```

## راهنمای استفاده

### 1. راه‌اندازی با فایل پیکربندی

```python
# ایجاد فایل config.json
config = {
    "max_features": 1500,
    "ngram_range": [1, 3],
    "enable_clustering": True,
    "cluster_count": 8
}

engine = FHESearchEngine(config_file='config.json')
```

### 2. افزودن اسناد با متادیتا

```python
documents = ["متن سند اول", "متن سند دوم"]
metadata = [
    {"category": "technology", "priority": "high"},
    {"category": "research", "priority": "medium"}
]

result = engine.add_documents(documents, metadata)
```

### 3. جستجوی پیشرفته با فیلتر

```python
# جستجو فقط در اسناد فارسی
result = engine.search(
    "رمزنگاری", 
    top_k=10,
    filters={'language': 'persian'}
)

# جستجو در خوشه خاص
result = engine.search(
    "security", 
    filters={'cluster_id': 2}
)
```

### 4. مدیریت داده‌ها

```python
# صادرات اطلاعات سیستم
engine.export_system_data("backup.json", include_vectors=True)

# وارد کردن داده‌ها
engine.import_system_data("backup.json")

# دریافت آمار کامل
status = engine.get_system_status()
print(f"تعداد اسناد: {status['system_info']['total_documents']}")
```

## معماری سیستم

### کلاس‌های اصلی

- **`FHESearchEngine`**: موتور اصلی جستجو
- **`PersianTextProcessor`**: پردازش متون چندزبانه
- **`DatabaseManager`**: مدیریت پایگاه داده
- **`FHEManager`**: مدیریت رمزنگاری هومورفیک

### فرآیند پردازش

1. **تشخیص زبان**: شناسایی خودکار زبان متن
2. **پیش‌پردازش**: نرمال‌سازی و پاک‌سازی
3. **استخراج ویژگی**: تبدیل به بردار TF-IDF
4. **رمزنگاری**: امن‌سازی داده‌ها با CKKS
5. **خوشه‌بندی**: گروه‌بندی اسناد مشابه
6. **ذخیره‌سازی**: نگهداری در پایگاه داده

## مثال‌های کاربردی

### سیستم مدیریت اسناد

```python
# افزودن مجموعه اسناد اداری
office_docs = [
    "گزارش فروش ماه جاری شرکت",
    "دستورالعمل امنیت سایبری",
    "برنامه توسعه محصولات جدید"
]

engine.add_documents(office_docs)

# جستجو در اسناد
results = engine.search("گزارش فروش", top_k=3)
```

### سیستم جستجوی علمی

```python
# افزودن مقالات علمی
papers = [
    "Deep learning applications in medical diagnosis",
    "کاربرد یادگیری عمیق در تشخیص پزشکی",
    "Quantum computing and cryptography"
]

engine.add_documents(papers)

# جستجوی متقابل انگلیسی-فارسی
en_results = engine.search("medical diagnosis")
fa_results = engine.search("تشخیص پزشکی")
```

## تنظیمات پیشرفته

### پارامترهای رمزنگاری

```json
{
  "encryption": {
    "poly_modulus_degree": 16384,
    "coeff_modulus": [60, 40, 40, 40, 60],
    "scale": 1099511627776
  }
}
```

### بهینه‌سازی عملکرد

```json
{
  "max_features": 2000,
  "batch_size": 100,
  "search": {
    "min_similarity_threshold": 0.02,
    "default_top_k": 10
  }
}
```

## آزمایش عملکرد

برای اجرای تست‌های عملکرد:

```bash
python example_usage.py
```

نتایج معمول:
- سرعت پردازش: ~50 سند/ثانیه
- زمان جستجو: <50 میلی‌ثانیه
- دقت جستجو: >85%

## عیب‌یابی

### مشکلات رایج

**خطای نصب TenSEAL:**
```bash
# روی سیستم‌های Linux
sudo apt-get install build-essential cmake
pip install tenseal --no-cache-dir
```

**مشکل encoding فارسی:**
```python
# اطمینان از UTF-8
import locale
locale.setlocale(locale.LC_ALL, 'fa_IR.UTF-8')
```

**کندی در رمزنگاری:**
- پارامتر `poly_modulus_degree` را کاهش دهید
- `max_size` در رمزنگاری بردارها را محدود کنید

### لاگ‌گیری

سیستم به صورت خودکار لاگ‌های مفصل در `fhe_search.log` ذخیره می‌کند:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## مشارکت در توسعه

### ساختار پروژه

```
FHE_Search_System/
├── fhe_search.py          # کد اصلی سیستم
├── example_usage.py       # مثال‌های استفاده
├── config.json           # فایل پیکربندی
├── requirements.txt      # وابستگی‌ها
└── README.md            # مستندات
```

### اضافه کردن ویژگی جدید

1. Fork کردن پروژه
2. ایجاد branch جدید
3. پیاده‌سازی ویژگی
4. نوشتن تست
5. ارسال Pull Request

## مجوز و حقوق

این پروژه تحت مجوز MIT منتشر شده است. استفاده، تغییر و توزیع آزاد است.

## تماس و پشتیبانی

برای گزارش مشکل یا پیشنهاد، از بخش Issues همین مخزن استفاده کنید.

## تغییرات نسخه‌ها

### v1.0.0 (2024-01-15)
- پیاده‌سازی اولیه سیستم FHE
- پشتیبانی از زبان‌های فارسی، عربی، انگلیسی
- رمزنگاری CKKS و جستجوی معنایی
- خوشه‌بندی خودکار و پایگاه داده SQLite

---

**⭐ اگر این پروژه برایتان مفید بود، لطفاً ستاره بدهید**
