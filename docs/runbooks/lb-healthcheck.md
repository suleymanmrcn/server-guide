# Load Balancer Healthcheck Fail

Bu runbook, load balancer healthcheck hatalarini kapsar.

## Hazirlik
- Etkilenen hedefleri ve healthcheck path'ini belirle.

## Mudahale adimlari
1. Uygulama healthcheck endpointini test et.
2. Nginx veya upstream konfigu kontrol et.
3. Gerekirse healthcheck timeoutlarini arttir.

## Ornek komutlar
```bash
curl -I http://127.0.0.1/healthz
nginx -t
```

## Dogrulama
- Load balancer hedefleri healthy mi?

