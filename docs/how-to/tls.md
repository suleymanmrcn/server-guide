# TLS (Let's Encrypt)

Bu rehber, Nginx uzerinde TLS sertifikasi kurulumunu kapsar.

## Gereksinimler
- DNS kaydi hazir
- Nginx calisir durumda

## Kurulum
```bash
apt install -y certbot python3-certbot-nginx
certbot --nginx -d example.com
```

## Otomatik yenileme
```bash
systemctl status certbot.timer
```

## Dogrulama
- HTTPS sertifikasi dogru mu?
- Yenileme zamanlayicisi aktif mi?

## Sorun olursa
- Yenileme problemleri icin: [TLS Yenileme Sorunu](../runbooks/tls-renewal.md)
