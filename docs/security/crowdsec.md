# CrowdSec

Bu bolum, Fail2ban alternatifi olarak CrowdSec kurulumunu kapsar.

## Kurulum (Debian/Ubuntu)
```bash
curl -s https://packagecloud.io/install/repositories/crowdsec/crowdsec/script.deb.sh | sudo bash
apt install -y crowdsec crowdsec-firewall-bouncer-iptables
systemctl enable --now crowdsec
```

## Temel kontrol
```bash
cscli decisions list
```

## Dogrulama
- `cscli metrics` cikti veriyor mu?
- Bouncer calisiyor mu?

