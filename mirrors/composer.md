# میرور Composer (Packagist) – راهنمای تنظیم در ایران

## میرورهای ایرانی موجود

| میرور | آدرس | وضعیت |
|-------|------|-------|
| چابکان | `https://mirror2.chabokan.net/composer` | ✅ فعال |
| لیارا | `https://package-mirror.liara.ir/repository/composer/` | ✅ فعال |
| مگان | `https://hub.megan.ir/composer` | ✅ فعال |
| پردیسکو | `https://mirrors.pardisco.co/composer` | ✅ فعال |
| هایپر کلودز | `https://packagist.hyperclouds.ir` | ✅ فعال |
| جمکو | `https://composer.jamko.ir` | ✅ فعال |
| کرگدن | `https://mirror.kargadan.ir/nexus/repository/composer-jamko` | ✅ فعال |
| مخزن ملی منابع متن باز | `https://archive.ito.gov.ir/composer` | ✅ فعال |
| پارس پک (ابرها)|` https://mirror.abrha.net/repository/composer/`| ✅ فعال |
| ایران سرور |`https://composer.iranserver.com/repository/composer/`| ✅ فعال |
| رانفلر | `https://mirror-composer.runflare.com` | ⚠️ محدودیت در پلن رایگان |


## روش ۱: تنظیم تکی برای یک پروژه

در مسیر ریشه پروژه (کنار `composer.json`) دستور زیر را اجرا کنید:

```bash
composer config repo.packagist composer https://mirrors.pardisco.co/composer/
```

این دستور آدرس میرور را فقط در `composer.json` همان پروژه ذخیره می‌کند.

## روش ۲: تنظیم سراسری (Global) – پیشنهادی

```bash
composer config --global repo.packagist composer https://mirrors.pardisco.co/composer/
```

### بررسی تنظیمات فعلی

```bash
composer config --global --list | grep repositories
```

## روش ۳: ویرایش دستی `composer.json`

در فایل `composer.json` پروژه، بخش `repositories` را اضافه کنید:

```json
{
  "repositories": [
    {
      "type": "composer",
      "url": "https://mirrors.pardisco.co/composer/",
      "options": {
        "ssl": {
          "verify_peer": false
        }
      }
    },
    {
      "packagist.org": false
    }
  ],
  "require": {
    "laravel/framework": "^11.0"
  }
}
```

> **توضیح:** خط `"packagist.org": false` مخزن اصلی Packagist را غیرفعال می‌کند تا فقط از میرور استفاده شود.

## تنظیم در Dockerfile

```dockerfile
FROM docker.arvancloud.ir/php:8.3-fpm

COPY --from=docker.arvancloud.ir/composer:latest /usr/bin/composer /usr/bin/composer

RUN composer config --global repo.packagist composer https://mirrors.pardisco.co/composer/

WORKDIR /var/www
COPY composer.json composer.lock ./
RUN composer install --no-dev --optimize-autoloader
```

## بازگشت به مخزن اصلی

```bash
# حذف تنظیم سراسری
composer config --global --unset repos.packagist

# یا تنظیم مجدد به آدرس اصلی
composer config --global repo.packagist composer https://repo.packagist.org
```

## عیب‌یابی

| مشکل | راه‌حل |
|------|--------|
| `The "https://repo.packagist.org/..." file could not be downloaded` | میرور داخلی تنظیم نشده؛ دستورات بالا را اجرا کنید |
| `SSL certificate problem` | گزینه `"verify_peer": false` را اضافه کنید یا `composer config --global disable-tls true` |
| پکیج در میرور پیدا نمی‌شود | میرور هنوز سینک نشده؛ منتظر بمانید یا میرور دیگری تست کنید |
| `Composer could not find a composer.json` | مطمئن شوید در مسیر صحیح پروژه هستید |
| `proc_open(): fork failed` | حافظه PHP کافی نیست؛ `memory_limit` را افزایش دهید |
