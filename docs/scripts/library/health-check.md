# Health Check Script

Otomatik sistem sağlık kontrolü yapan ve raporlayan script.

## Kullanım

```bash
./scripts/health-check.sh
```

## Kontrol Edilenler

- **CPU/RAM:** %90 üzeri kullanımda uyarı
- **Disk:** %85 üzeri kullanımda uyarı
- **Servisler:** Docker, Nginx, SSH durumu
- **Portlar:** 80, 443 erişilebilirliği

## Örnek Çıktı

```text
[OK]  CPU Load: 0.45
[OK]  Memory Usage: 45%
[OK]  Disk Usage: 55%
[OK]  Docker Service is UP
```
