# Yaygin Sorunlar

## SSH baglanti sorunu
- IP, DNS ve firewall kurallarini kontrol et.
- `sshd_config` degisikliklerini geri al (gerekirse).
- Loglar: `journalctl -u ssh`
- Detayli akisin tamami icin: [SSH Lockout](../runbooks/ssh-lockout.md)

## Servis ayakta degil
- Hizli kontrol: servis durumu, loglar, port dinleme.
- Detayli akisin tamami icin: [Servis Kesintisi](../runbooks/service-down.md)

## Nginx 502
- Uygulama portu dinliyor mu?
- Reverse proxy upstream dogru mu?
- Loglar: `/var/log/nginx/error.log`
- Detayli akisin tamami icin: [Servis Kesintisi](../runbooks/service-down.md)

## Disk dolu
- Disk ve log hizli kontrol.
- Detayli akisin tamami icin: [Disk Dolu Mudahalesi](../runbooks/disk-full.md)

## TLS sertifikasi yenilenmiyor
- Certbot timer ve loglari kontrol et.
- Detayli akisin tamami icin: [TLS Yenileme Sorunu](../runbooks/tls-renewal.md)

## Dogrulama
- Sorun tekrarliyor mu?
- Loglar temiz mi?
