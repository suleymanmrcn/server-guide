# Deploy Geri Alma

Bu runbook, hatali deploy sonrasinda hizli geri donus adimlarini icerir.

## Hazirlik
- Etkilenen servis ve surumu belirle.
- Son saglam surum etiketini not et.

## Mudahale adimlari
1. Yeni deployu durdur.
2. Bir onceki stabil surume geri don.
3. Servisi yeniden baslat.
4. Loglari incele ve hatayi kaydet.

## Ornek komutlar
```bash
systemctl stop app
# ornek: git tag ile geri donus
git checkout v1.2.3
systemctl start app
```

## Dogrulama
- Servis saglik kontrolu basarili mi?
- Kullanici hatalari normale dondu mu?

