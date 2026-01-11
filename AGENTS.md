# Server Setup Handbook Agent Guide

This repository contains a production-ready server setup handbook built with MkDocs.
Follow the structure below when adding or expanding content.

## Scope
- Linux server setup (Ubuntu/Debian baseline).
- Security, networking, deployment basics, monitoring, and maintenance.
- Keep instructions provider-agnostic unless a step requires a specific tool.

## Writing rules
- Default language: Turkish.
- Prefer short, actionable steps with commands.
- Mark assumptions and requirements explicitly.
- Avoid long theory blocks; link or summarize when needed.
- Keep headings consistent with the section map below.

## Section map
- docs/index.md
  - Amac, hedef kitle, kapsam, nasil kullanilir
- docs/architecture/index.md
  - Sistem tasarimi genel gorunum
- docs/architecture/overview.md
  - Mimari katmanlar ve bagimliliklar
- docs/architecture/network.md
  - Ag tasarimi ve erisim matrisi
- docs/architecture/operations.md
  - Operasyonel mimari ve gozlenebilirlik
- docs/how-to/index.md
  - Kurulum rehberleri genel giris
- docs/how-to/base-os.md
  - Temel OS kurulumu ve ilk ayarlar
- docs/how-to/nginx.md
  - Nginx kurulumu ve servis dogrulama
- docs/how-to/tls.md
  - TLS sertifikasi kurulumu
- docs/how-to/monitoring.md
  - Izleme ve alarm kurulumlari
- docs/how-to/backups.md
  - Yedekleme kurulumu
- docs/how-to/reverse-proxy.md
  - Reverse proxy konfigurasyonu
- docs/how-to/systemd-service.md
  - Systemd servis tanimi
- docs/how-to/logrotate.md
  - Log rotasyon kurallari
- docs/how-to/monitoring-stack.md
  - Monitoring stack plani
- docs/how-to/port-checks.md
  - Ag ve port kontrolleri
- docs/security/index.md
  - Hardening genel giris
- docs/security/ssh.md
  - SSH hardening
- docs/security/firewall.md
  - Firewall kurallari
- docs/security/fail2ban.md
  - Brute force korumasi
- docs/security/sshguard.md
  - Fail2ban alternatifi
- docs/security/crowdsec.md
  - Fail2ban alternatifi
- docs/security/ufw-rate-limit.md
  - UFW rate limit alternatifi
- docs/security/lynis.md
  - Guvenlik denetimi
- docs/security/updates.md
  - Otomatik guvenlik guncellemeleri
- docs/runbooks/index.md
  - Prod mudahale rehberleri genel giris
- docs/runbooks/incident-response.md
  - Olay yonetimi adimlari
- docs/runbooks/deploy-rollback.md
  - Deploy geri alma adimlari
- docs/runbooks/disk-full.md
  - Disk dolu mudahalesi
- docs/runbooks/service-down.md
  - Servis kesintisi mudahalesi
- docs/runbooks/db-down.md
  - Veritabani down mudahalesi
- docs/runbooks/cpu-spike.md
  - CPU spike mudahalesi
- docs/runbooks/tls-renewal.md
  - TLS yenileme sorunu
- docs/runbooks/disk-io.md
  - Disk IO krizi
- docs/runbooks/oom-kill.md
  - OOM kill / memory leak
- docs/runbooks/inode-full.md
  - Inode dolu mudahalesi
- docs/runbooks/dns-issues.md
  - DNS sorunlari
- docs/runbooks/network-latency.md
  - Network latency spike
- docs/runbooks/lb-healthcheck.md
  - Load balancer healthcheck fail
- docs/runbooks/ddos-rate-limit.md
  - DDoS / rate limit
- docs/runbooks/cert-expired.md
  - Sertifika suresi doldu
- docs/runbooks/replication-lag.md
  - Replication lag
- docs/runbooks/backup-restore-fail.md
  - Backup restore basarisiz
- docs/runbooks/config-deploy-fail.md
  - Config deploy hatasi
- docs/runbooks/ssh-lockout.md
  - SSH erisim kesildi
- docs/scripts/index.md
  - Script repo aciklamalari
- docs/scripts/conventions.md
  - Script standartlari ve dokumantasyon
- docs/scripts/execution.md
  - Script calistirma rehberi
- docs/scripts/safety.md
  - Script guvenligi
- docs/checklists/server-first-setup.md
  - Ilk kurulum kontrol listesi
- docs/checklists/prod-ready.md
  - Production readiness kontrol listesi
- docs/troubleshooting/index.md
  - Troubleshooting genel giris
- docs/troubleshooting/common-issues.md
  - Yaygin sorunlar

## Expansion plan (parcalar)
- Parca 1: Runbooks ayrintilari (incident, deploy rollback, disk dolu, service down)
- Parca 2: How-to ayrintilari (nginx, tls, monitoring, backups)
- Parca 3: Security ayrintilari (fail2ban, ssh, ufw, updates)
- Parca 4: Checklists ayrintilari (server-first-setup, prod-ready)
- Parca 5: Architecture + Scripts derinlestirme

## Content checklist
- Her bolumde amac + adimlar + dogrulama yer almali.
- Komutlar icin acik kod bloklari kullan.
- Riskli adimlar icin uyarı/admonition ekle.
