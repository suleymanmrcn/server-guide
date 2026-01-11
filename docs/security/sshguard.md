# SSHGuard

Bu bolum, Fail2ban alternatifi olarak SSHGuard kurulumunu kapsar.

## Kurulum
```bash
apt update
apt install -y sshguard
systemctl enable --now sshguard
```

## Varsayilan log kaynaklari
- `/var/log/auth.log`
- SSH denemeleri ve yetkisiz erisimler

## Dogrulama
- `systemctl status sshguard` calisiyor mu?
- `/var/log/sshguard/sshguard.log` loglari yaziyor mu?

