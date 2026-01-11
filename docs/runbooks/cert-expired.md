# Sertifika Suresi Doldu

Bu runbook, TLS sertifikasi suresi doldugunda uygulanacak adimlari kapsar.

## Hazirlik
- Etkilenen domainleri belirle.

## Mudahale adimlari
1. Sertifika durumunu kontrol et.
2. Yenileme yap ve Nginx'i reload et.
3. Yenileme otomasyonunu dogrula.

## Ornek komutlar
```bash
openssl s_client -connect example.com:443 -servername example.com
certbot renew
systemctl reload nginx
```

## Dogrulama
- Sertifika tarihi guncellendi mi?

