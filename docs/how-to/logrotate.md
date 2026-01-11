# Logrotate Kurulumu

Bu rehber, log dosyalarinin rotasyonunu kapsar.

## Ornek konfigurasyon
`/etc/logrotate.d/app`:
```
/var/log/app/*.log {
  daily
  rotate 7
  compress
  missingok
  notifempty
  copytruncate
}
```

## Dogrulama
```bash
logrotate -d /etc/logrotate.d/app
```

## Dogrulama
- Rotasyon kurali dogru calisiyor mu?
- Disk kullanimi kontrol altinda mi?

