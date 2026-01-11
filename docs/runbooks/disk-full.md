# Disk Dolu Mudahalesi

Bu runbook, disk dolulugu durumunda hizli stabilizasyonu kapsar.

## Hazirlik
- Hangi bolumun dolu oldugunu belirle.
- Buyuk dosya ve dizinleri tespit et.

## Mudahale adimlari
1. Disk kullanimini kontrol et.
2. Buyuk dosyalari listele.
3. Gereksiz log ve cache dosyalarini temizle.
4. Log rotasyonunu dogrula.

## Ornek komutlar
```bash
df -h
sudo du -sh /var/log/* | sort -h
sudo journalctl --vacuum-time=7d
```

## Dogrulama
- Disk dolulugu kritik esik altina indi mi?
- Servisler tekrar stabil mi?

## Ilgili runbook
- Inode sorunu icin: [Inode Dolu](inode-full.md)
