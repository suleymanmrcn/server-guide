# TLS Yenileme Sorunu

Bu runbook, TLS sertifikasi yenileme hatalarinda takip edilecek adimlari icerir.

## Hazirlik
- Etkilenen domainleri belirle.
- Son certbot loglarini kontrol et.

## Mudahale adimlari
1. Certbot timer durumunu kontrol et.
2. Manuel yenileme dene.
3. Nginx konfig ve DNS dogrulugunu kontrol et.

## Ornek komutlar
```bash
systemctl status certbot.timer
certbot renew --dry-run
nginx -t
```

## Dogrulama
- Yenileme basarili mi?
- HTTPS sertifikasi guncellendi mi?

## Not
- Kurulum adimlari icin: [TLS (Let's Encrypt)](../how-to/tls.md)
