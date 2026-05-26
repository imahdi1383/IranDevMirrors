# میرور pip (PyPI) – راهنمای تنظیم در ایران

## میرورهای ایرانی موجود

| میرور | آدرس Host | آدرس Index (برای pip) | وضعیت |
|-------|----------|----------------------|-------|
| مخزن ملی منابع متن باز | `https://archive.ito.gov.ir/python/` |`https://pypi.hyperclouds.ir/simple`| ✅ فعال |
| هایپر کلودز | `pypi.hyperclouds.ir` | `https://pypi.hyperclouds.ir/simple` | ✅ فعال |
| مگان | `https://hub.megan.ir/pypi` | `https://hub.megan.ir/pypi/simple` | ✅ فعال |
| جمکو | `pypi.jamko.ir` | `https://pypi.jamko.ir/simple` | ✅ فعال |
| پردیسکو | `mirrors.pardisco.co` | `https://mirrors.pardisco.co/pip/simple/` | ✅ فعال |
| کرگدن | `mirror.kargadan.ir` | `https://mirror.kargadan.ir/nexus/repository/pypi-group/simple` | ✅ فعال |
| پارس پک (ابرها)|`mirror.abrha.net`|`https://mirror.abrha.net/repository/pypi/simple`| ✅ فعال |
| ایران سرور | `pypi.iranserver.com` | `https://pypi.iranserver.com/repository/pypi/simple` | ✅ فعال |
| رانفلر | `mirror-pypi.runflare.com` | `https://mirror-pypi.runflare.com/simple/` | ⚠️ محدودیت در پلن رایگان |

### میرورهای بین‌المللی (نیاز به دسترسی خارجی)

این میرورها فقط در صورت دسترسی به اینترنت بین‌المللی قابل استفاده‌اند:

| میرور | آدرس Index |
|-------|-----------|
| Tsinghua (چین) | `https://pypi.tuna.tsinghua.edu.cn/simple/` |
| Aliyun (چین) | `https://mirrors.aliyun.com/pypi/simple/` |
| USTC (چین) | `https://pypi.mirrors.ustc.edu.cn/simple/` |
| Huawei Cloud | `https://repo.huaweicloud.com/repository/pypi/simple/` |

## روش ۱: نصب تکی با میرور (بدون تغییر تنظیمات)

```bash
pip install \
  --trusted-host pypi.hyperclouds.ir \
  -i https://pypi.hyperclouds.ir/simple \
  django
```

> **توضیح:** فلگ `--trusted-host` باعث می‌شود pip بدون بررسی SSL به این هاست اعتماد کند. مقدار آن فقط **نام هاست** است (بدون `https://`).

## روش ۲: تنظیم سراسری (Global) – پیشنهادی

با اجرای دستورات زیر، تمام دستورات `pip install` به‌صورت خودکار از میرور داخلی استفاده می‌کنند. این دستورات در **تمام سیستم‌عامل‌ها** یکسان هستند:

```bash
pip config --user set global.index https://pypi.hyperclouds.ir/simple
pip config --user set global.index-url https://pypi.hyperclouds.ir/simple
pip config --user set global.trusted-host pypi.hyperclouds.ir
```

### بررسی تنظیمات فعلی

```bash
pip config get global.index-url
```

## روش ۳: تنظیم از طریق فایل `pip.conf`

### لینوکس و مک

فایل `~/.config/pip/pip.conf` (یا `~/.pip/pip.conf`) را بسازید:

```ini
[global]
index-url = https://pypi.hyperclouds.ir/simple
trusted-host = pypi.hyperclouds.ir
```

### ویندوز

فایل `%APPDATA%\pip\pip.ini` را بسازید:

```ini
[global]
index-url = https://pypi.hyperclouds.ir/simple
trusted-host = pypi.hyperclouds.ir
```

## تنظیم در Dockerfile

```dockerfile
ENV PIP_INDEX_URL=https://pypi.hyperclouds.ir/simple \
    PIP_TRUSTED_HOST=pypi.hyperclouds.ir

RUN pip install --upgrade pip \
    && pip install -r requirements.txt
```

## تنظیم در `requirements.txt`

می‌توانید آدرس میرور را مستقیماً در ابتدای فایل `requirements.txt` هم مشخص کنید:

```
--index-url https://pypi.hyperclouds.ir/simple
--trusted-host pypi.hyperclouds.ir
django>=4.2
requests
```

## بازگشت به مخزن اصلی

```bash
pip config --user set global.index-url https://pypi.org/simple
pip config --user unset global.trusted-host
```

## عیب‌یابی

| مشکل | راه‌حل |
|------|--------|
| `Could not find a version that satisfies the requirement` | پکیج هنوز در میرور سینک نشده؛ میرور دیگری امتحان کنید |
| `SSLError` / `CERTIFICATE_VERIFY_FAILED` | فلگ `--trusted-host` را اضافه کنید |
| `Connection Timeout` | میرور تنظیم نشده یا از کار افتاده؛ آدرس را بررسی و میرور دیگری تست کنید |
| `WARNING: pip is configured with locations that require TLS/SSL` | `--trusted-host` را به تنظیمات سراسری اضافه کنید |
