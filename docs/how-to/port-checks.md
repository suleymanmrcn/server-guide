# Ag ve Port Kontrolleri

Bu rehber, acik portlar ve baglantilari kontrol etmeyi kapsar.

## Beklenen portlar (ornek)
- 22/tcp: SSH
- 80/tcp: HTTP
- 443/tcp: HTTPS

## Acik portlari goster
```bash
ss -tulpen
```

## Belirli portu dinleyen sureci bul
```bash
ss -tulpen | rg ":443"
```

## Dosya descriptor ile kontrol
```bash
lsof -i -P -n | rg LISTEN
```

## Firewall durumu
```bash
ufw status verbose
```

## Dogrulama
- Gerekli portlar acik mi?
- Beklenmeyen servisler dinliyor mu?
