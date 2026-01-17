# Systemd Service Template

Linux servisi olarak uygulama çalıştırmak için (Docker kullanmıyorsanız).

1. Dosyayı oluştur: `/etc/systemd/system/myapp.service`
2. Aktif et: `sudo systemctl enable --now myapp`

```ini
[Unit]
Description=My Awesome Node.js App
Documentation=https://example.com
After=network.target

[Service]
# Hangi kullanıcı ile çalışacak?
User=deployer
Group=deployer

# Çevresel Değişkenler
Environment=NODE_ENV=production
Environment=PORT=3000

# Çalışma Dizini ve Komut
WorkingDirectory=/home/deployer/app
ExecStart=/usr/bin/node /home/deployer/app/server.js

# Restart Politikası
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```
