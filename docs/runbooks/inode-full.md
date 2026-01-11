# Inode Dolu

Bu runbook, inode tukendiginde teshis ve temizligi kapsar.

## Hazirlik
- Etkilenen dosya sistemi belirle.

## Mudahale adimlari
1. Inode kullanimini kontrol et.
2. Cok sayida kucuk dosya olusturan dizinleri bul.
3. Gereksiz dosyalari temizle.

## Ornek komutlar
```bash
df -i
sudo find /var/log -type f | wc -l
sudo find /var/log -type f -mtime +7 -delete
```

## Dogrulama
- Inode kullanimi kritik esik altina indi mi?

## Ilgili runbook
- Genel disk teshisi icin: [Disk Dolu Mudahalesi](disk-full.md)
