# میرور Docker – راهنمای تنظیم در ایران

## میرورهای ایرانی موجود

| میرور | آدرس | وضعیت |
|-------|------|-------|
| چابکان | `https://mirror2.chabokan.net` | ✅ فعال  |
| مگان | `hub.megan.ir` | ✅ فعال  |
| ابرآروان | `https://docker.arvancloud.ir` | ✅ فعال  |
| هایپر کلودز | `https://docker.hyperclouds.ir` | ✅ فعال  |
| هایو | `https://docker.haiocloud.com` | ✅ فعال |
| جمکو | `https://docker.jamko.ir` | ✅ فعال |
| CDN.ir | `https://mirror.cdn.ir` | ✅ فعال |
| پردیسکو | `https://mirrors.pardisco.co` | ✅ فعال |
| آتلانتیس کلود | `https://hub.atlantiscloud.ir` | ✅ فعال |
| کرگدن | `https://docker-mirror.kargadan.ir` | ✅ فعال |
| پارس پک (ابرها) | `https://docker.abrha.net` | ✅ فعال |
| رانفلر | `https://mirror-docker.runflare.com` | ⚠️ محدودیت در پلن رایگان |


## روش ۱: Pull مستقیم با پیشوند میرور (بدون تغییر تنظیمات)

اگر نمی‌خواهید تنظیمات داکر را تغییر دهید، نام میرور را قبل از نام ایمیج بگذارید:

```bash
# به‌جای:
docker pull python:3.12-slim

# از این استفاده کنید:
docker pull docker.arvancloud.ir/python:3.12-slim
```

> **نکته:** در `Dockerfile` هم می‌توانید از همین روش استفاده کنید:
> ```dockerfile
> FROM docker.arvancloud.ir/python:3.12-slim-bullseye
> ```

## روش ۲: تنظیم سراسری (Global) – پیشنهادی

با این روش، تمام دستورات `docker pull` به‌صورت خودکار از میرور استفاده می‌کنند و نیازی به تغییر نام ایمیج نیست.

### لینوکس

1. فایل `/etc/docker/daemon.json` را بسازید (یا ویرایش کنید):

```bash
sudo bash -c 'cat > /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": ["https://docker.arvancloud.ir"]
}
EOF'
```

2. داکر را ری‌استارت کنید:

```bash
sudo systemctl restart docker
```

3. حالا مثل همیشه pull کنید:

```bash
docker pull python:3.12-slim
```

### مک (macOS)

1. فایل زیر را ویرایش کنید:

```
~/.docker/daemon.json
```

2. محتوای زیر را اضافه کنید:

```json
{
  "registry-mirrors": ["https://docker.arvancloud.ir"]
}
```

3. Docker Desktop را ری‌استارت کنید.

### ویندوز

1. فایل زیر را بسازید یا ویرایش کنید:

```
C:\Users\<YourName>\.docker\daemon.json
```

2. محتوای زیر را اضافه کنید:

```json
{
  "registry-mirrors": ["https://docker.arvancloud.ir"]
}
```

3. Docker Desktop را ری‌استارت کنید.

> **روش جایگزین در Docker Desktop:** Settings → Docker Engine → فیلد `registry-mirrors` را ویرایش کنید.

## بررسی تنظیمات فعلی

```bash
docker info | grep -A 5 "Registry Mirrors"
```

## استفاده از `insecure-registries` (در صورت خطای SSL)

اگر میرور گواهی SSL معتبر ندارد:

```json
{
  "insecure-registries": ["https://docker.arvancloud.ir"],
  "registry-mirrors": ["https://docker.arvancloud.ir"]
}
```

> **⚠️ هشدار:** استفاده از `insecure-registries` امنیت اتصال را کاهش می‌دهد. فقط در محیط توسعه استفاده کنید.

## نکته مهم: Logout قبل از تغییر

قبل از ری‌استارت داکر، توصیه می‌شود ابتدا از رجیستری فعلی خارج شوید:

```bash
docker logout
sudo systemctl restart docker
```

## عیب‌یابی

| مشکل | راه‌حل |
|------|--------|
| `Error response from daemon: Get https://registry-1.docker.io/...` | میرور تنظیم نشده؛ `daemon.json` را بررسی کنید |
| `TLS handshake timeout` | میرور از کار افتاده؛ میرور دیگری تست کنید |
| `manifest unknown` | نسخه ایمیج در میرور سینک نشده؛ آدرس کامل ایمیج را با پیشوند میرور امتحان کنید |
| `permission denied` هنگام ویرایش `daemon.json` | دستور را با `sudo` اجرا کنید |
| تغییرات `daemon.json` اعمال نمی‌شود | حتماً داکر را ری‌استارت کنید (`systemctl restart docker`) |
