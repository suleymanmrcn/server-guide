# Monitoring Kurulumu

Bu rehber, temel servis izleme ve alarmlari kapsar.

## Gereksinimler
- Izlenecek servis listesi
- Uptime/alert servisi (ornek: Uptime Kuma, PagerDuty, Opsgenie)

## Kurulum adimlari
1. Kritik endpointleri listele.
2. Healthcheck URL veya port kontrolu ekle.
3. Alarm esikleri belirle (latency, hata orani).

## Temel metrikler ve loglar
- Sistem loglari: `journalctl`
- Web sunucusu loglari: `/var/log/nginx/`
- Disk: `df -h`, bellek: `free -m`
- Servis durumu: `systemctl status <servis>`

## Dogrulama
- Alarm tetikleniyor mu?
- Sahiplik ve oncall dogru mu?
