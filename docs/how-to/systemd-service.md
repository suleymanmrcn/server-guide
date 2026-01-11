# Systemd Servis Tanimi

Bu rehber, uygulama icin systemd servis tanimlamayi kapsar.

## Servis dosyasi
`/etc/systemd/system/app.service`:
```
[Unit]
Description=App Service
After=network.target

[Service]
User=deploy
WorkingDirectory=/opt/app
ExecStart=/opt/app/start.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

## Etkinlestirme
```bash
systemctl daemon-reload
systemctl enable --now app
```

## Dogrulama
- `systemctl status app` calisiyor mu?
- Restart politikasi beklenen gibi mi?

