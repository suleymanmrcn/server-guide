# Lynis

Bu bolum, guvenlik denetimi icin Lynis kullanimini kapsar.

## Kurulum
```bash
apt update
apt install -y lynis
```

## Tarama
```bash
lynis audit system
```

## Rapor ve oneriler
- `lynis report show` ile ozet goruntule.
- Onerileri uygulamadan once etki analizi yap.

## Dogrulama
- Kritik bulgular listelendi mi?
- Uygulanan oneriler dokumante edildi mi?

