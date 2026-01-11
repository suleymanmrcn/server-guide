# Fail2ban

Bu bolum, brute force denemelerine karsi fail2ban kurulumunu kapsar.

## Kurulum
```bash
apt install -y fail2ban
systemctl enable --now fail2ban
```

## Basit jail ornegi
`/etc/fail2ban/jail.d/sshd.local`:
```
[sshd]
enabled = true
bantime = 1h
findtime = 10m
maxretry = 5
```

```bash
systemctl restart fail2ban
```

## Dogrulama
- `fail2ban-client status` calisiyor mu?
- `sshd` jail aktif mi?

## Ne zaman hangisi?
- Fail2ban: Basit kurulum, tek sunucu icin hizli koruma.
- SSHGuard: Hafif ve sade, minimum ayar isteyenler icin.
- CrowdSec: Merkezi politika ve topluluk verisi isteyen ekipler icin.
- UFW limit: Ek arac kurmadan temel koruma isteyenler icin.
