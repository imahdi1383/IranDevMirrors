# محدودیت اتصال اینترنت در ایران – راهنمای جامع

## مشکل

به دلیل محدودیت‌های اینترنت در ایران، دسترسی مستقیم به بسیاری از سرورهای بین‌المللی (مانند `npmjs.org`، `hub.docker.com`، `pypi.org`، `github.com` و حتی `google.com`) وجود ندارد. این یعنی:

- دستورات `npm install`، `pip install`، `docker pull`، `composer install` و مشابه آن‌ها با خطای **Connection Timeout** یا **ECONNREFUSED** مواجه می‌شوند.
- فقط سرورهای داخل ایران (اینترنت ملی) در دسترس هستند.
- سرویس‌های AI مانند OpenAI API مستقیماً قابل دسترسی نیستند؛ از جایگزین ایرانی **GapGPT API** (`api.gapgpt.app`) استفاده می‌شود.

## راه‌حل: میرورهای ایرانی

میرورها (Mirrors) کپی‌هایی از مخازن اصلی هستند که روی سرورهای داخل ایران میزبانی می‌شوند. با تنظیم ابزارهای خود برای استفاده از این میرورها، می‌توانید بدون نیاز به دسترسی بین‌المللی، پکیج‌ها و ایمیج‌های موردنیاز را دانلود کنید.

### راهنمای میرور هر ابزار

| ابزار | فایل راهنما | توضیح |
|-------|------------|-------|
| npm / yarn / pnpm | [mirrors/npm.md](mirrors/npm.md) | میرور مخازن Node.js |
| pip (Python) | [mirrors/pip.md](mirrors/pip.md) | میرور PyPI برای پکیج‌های Python |
| Docker | [mirrors/docker.md](mirrors/docker.md) | میرور Docker Hub برای ایمیج‌ها |
| Composer (PHP) | [mirrors/composer.md](mirrors/composer.md) | میرور Packagist برای پکیج‌های PHP |

### سایت‌های مرجع میرورهای ایرانی

| سایت | آدرس | توضیح |
|------|------|-------|
| چابکان | https://iran.chabokan.net/ و https://mirror2.chabokan.net/ | فهرست جامع میرورها |
| لیارا | https://liara.ir/mirrors/ | میرور NPM، Docker و غیره |
| HyperClouds | https://mirrors.hyperclouds.ir/ | میرور Docker، PyPI و غیره |
| سازمان فناوری اطلاعات | https://repo.ito.gov.ir/ | مخازن دولتی |
| جمکو | https://jamko.ir/ | میرور NPM، PyPI |
| رسانگار | http://mirror.rasanegaar.com | میرور عمومی |
| رانفلر | https://runflare.com/mirrors/ | ⚠️ پلن رایگان محدودیت دارد |

> **⚠️ هشدار:** میرورهای **رانفلر** (`runflare.com`) در پلن رایگان دارای محدودیت حجم و تعداد درخواست هستند. برای پروژه‌های حرفه‌ای، میرورهای چابکان، لیارا یا HyperClouds پیشنهاد می‌شوند.

## مثال عملی: Dockerfile با میرورهای ایرانی

نمونه‌ای از یک Dockerfile برای پروژه Python/Django که تمام وابستگی‌ها را از میرورهای داخلی دریافت می‌کند:

```dockerfile
FROM docker.arvancloud.ir/python:3.12-slim-bullseye

LABEL maintainer="your-email@example.com"

ENV PYTHONUNBUFFERED=1 \
    PIP_INDEX_URL=https://pypi.hyperclouds.ir/simple

# تنظیم مخازن APT به میرور ابرآروان
RUN sed -i 's|http://deb.debian.org/debian|http://mirror.arvancloud.ir/debian|g' /etc/apt/sources.list \
 && sed -i 's|http://security.debian.org/debian-security|http://mirror.arvancloud.ir/debian-security|g' /etc/apt/sources.list

# نصب Nginx از مخزن داخلی
RUN apt-get update -o Acquire::Check-Valid-Until=false \
    && apt-get install -y nginx \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app

# نصب پکیج‌های Python از میرور داخلی
COPY ./requirements.txt .
RUN pip install --upgrade pip \
    && pip install -r requirements.txt

COPY ./core .
COPY ./dockerfiles/prod/django/nginx/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

COPY ./dockerfiles/prod/django/entrypoint.sh .
RUN chmod +x ./entrypoint.sh
CMD ["./entrypoint.sh"]
```

### نکات کلیدی این Dockerfile

1. **ایمیج پایه:** به‌جای `python:3.12-slim-bullseye` از `docker.arvancloud.ir/python:3.12-slim-bullseye` استفاده شده.
2. **مخازن APT:** آدرس `deb.debian.org` و `security.debian.org` به `mirror.arvancloud.ir` تغییر یافته.
3. **PyPI:** متغیر `PIP_INDEX_URL` به میرور داخلی اشاره می‌کند.

## عیب‌یابی سریع

| خطا | علت احتمالی | راه‌حل |
|-----|------------|--------|
| `ECONNREFUSED` / `Connection Timeout` | دسترسی به سرور خارجی | میرور داخلی را تنظیم کنید |
| `CERT_HAS_EXPIRED` / `SSL Error` | گواهی SSL میرور منقضی شده | میرور دیگری امتحان کنید یا `--trusted-host` اضافه کنید |
| `404 Not Found` روی میرور | پکیج هنوز در میرور سینک نشده | میرور دیگری امتحان کنید |
| `429 Too Many Requests` | محدودیت نرخ درخواست میرور رایگان | به میرور دیگر تغییر دهید |
| `ENOTFOUND` | DNS داخلی آدرس میرور را نمی‌شناسد | DNS خود را بررسی کنید (مثلاً `403.online` یا `shecan.ir`) |
