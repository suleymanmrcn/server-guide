# DNS Sorunlari

Bu runbook, domain resolve edilemiyorsa teshis adimlarini kapsar.

## Hazirlik
- Domain ve etkilenen servisleri belirle.

## Mudahale adimlari
1. DNS kayitlarini kontrol et.
2. Resolver ayarlarini dogrula.
3. Gecici olarak alternatif DNS kullan.

## Ornek komutlar
```bash
dig example.com
cat /etc/resolv.conf
nslookup example.com 1.1.1.1
```

## Dogrulama
- DNS sorgulari dogru yanit veriyor mu?

