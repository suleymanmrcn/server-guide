# Temel OS Kurulumu

Bu bolum, Ubuntu/Debian icin temel kurulum ve ilk ayarlari kapsar.

## Kurulum
- Minimal kurulum sec.
- Disk bolumleme planini belirle.
- Root yerine yonetici kullanici olustur.

## Ilk giris
```bash
ssh root@SUNUCU_IP
```

## Yonetici kullanici ve sudo
```bash
adduser deploy
usermod -aG sudo deploy
```

## Hostname ve /etc/hosts
```bash
hostnamectl set-hostname ornek-sunucu
```

`/etc/hosts` icine IP ve hostname eslemesini ekle.

## Zaman senkronizasyonu
```bash
timedatectl set-timezone Europe/Istanbul
systemctl enable --now systemd-timesyncd
```

## Temel guncellemeler
```bash
apt update
apt -y upgrade
```

## Dogrulama
- `deploy` kullanicisi ile SSH girisi yapilabiliyor mu?
- Sistem guncel mi?
- `hostnamectl` ve `timedatectl status` dogru mu?
