# UFW Rate Limiting

Bu bolum, ek arac kurmadan UFW ile temel brute force sinirlamayi kapsar.

## Uygulama
```bash
ufw limit OpenSSH
```

## Dogrulama
```bash
ufw status verbose
```

## Notlar
- Bu yontem temel seviyede koruma saglar.
- Fail2ban/SSHGuard/CrowdSec kadar ayrintili degildir.

