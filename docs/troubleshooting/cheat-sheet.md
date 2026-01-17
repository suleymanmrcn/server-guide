# Advanced Cheat Sheet (Senior SRE) 🛠️

`top` komutundan fazlasına ihtiyacınız olduğunda.

## 🧠 CPU & Process Forensics

| Senaryo                 | Senior Komut                               | Açıklama                                                                     |
| :---------------------- | :----------------------------------------- | :--------------------------------------------------------------------------- |
| **Process ne yapıyor?** | `strace -p <PID> -f -e trace=file,network` | Programın kernel ile konuşmasını izle. Dosya açıyor mu? Ağ isteği atıyor mu? |
| **Core dağılımı**       | `mpstat -P ALL 1`                          | Tek bir çekirdek mi %100 yoksa hepsi mi dengeli?                             |
| **Process ağacı**       | `ps fax`                                   | Hiyerarşik process görünümü (Zombie'nin babasını bulmak için).               |
| **IO Bekleyenler**      | `vmstat 1`                                 | `b` (blocked) sütunu yüksekse CPU değil, Disk sorunu vardır.                 |

## 💾 Memory Forensics

| Senaryo            | Senior Komut                                | Açıklama                                                                                          |
| :----------------- | :------------------------------------------ | :------------------------------------------------------------------------------------------------ | ------------------------------ |
| **Kim RAM yiyor?** | `ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -10`                                                                                         | En çok RAM tüketenleri sırala. |
| **Swap kullanımı** | `vmstat 1`                                  | `si` (swap in) ve `so` (swap out) > 0 ise sistem RAM yetmezliğinden can çekişiyordur.             |
| **Cache analizi**  | `free -h`                                   | `buff/cache` yüksekse korkma, Linux RAM boş kalmasın diye kullanıyordur. Önemli olan `available`. |

## 🌐 Network Forensics

| Senaryo                | Senior Komut                       | Açıklama                                                                |
| :--------------------- | :--------------------------------- | :---------------------------------------------------------------------- | ------------------------------------ |
| **Paket Analizi**      | `tcpdump -nni eth0 port 80 -A -s0` | HTTP paketlerinin içeriğini (Body/Header) canlı izle. `-A` ASCII basar. |
| **Portu kim tutuyor?** | `ss -tulpn                         | grep :80`                                                               | `netstat` yerine modern `ss` kullan. |
| **DNS Sorunu**         | `dig +trace google.com`            | DNS sorgusunu root serverlardan başlayarak adım adım izle.              |
| **Detaylı Trace**      | `mtr -zbwc 100 8.8.8.8`            | Hangi hop'ta paket kaybı var? (Ping + Traceroute kombosu).              |
| **Açık Dosyalar**      | `lsof -iTCP -sTCP:ESTABLISHED`     | Sadece kurulu (established) TCP bağlantılarını dök.                     |

## 💿 Disk & IO Forensics

| Senaryo                    | Senior Komut           | Açıklama                                                                   |
| :------------------------- | :--------------------- | :------------------------------------------------------------------------- | ---------------------------- |
| **Anlık IO**               | `iostat -xz 1`         | `%util` %100'e yakınsa disk darboğazdadır. `await` süresi tepki süresidir. |
| **Hangi process yazıyor?** | `iotop -oPa`           | Diski yoran suçu process bazlı bul.                                        |
| **Büyük dosyalar**         | `du -h --max-depth=1 / | sort -hr`                                                                  | Klasör klasör boyut analizi. |
| **Inode bitmiş mi?**       | `df -i`                | Disk boşta olsa bile Inode biterse dosya yazamazsın.                       |

## 🚨 Kernel & Logs

```bash
# OOM Killer (RAM bitince kimi öldürdü?)
dmesg -T | grep -i "kill"

# Son 10 dakika içinde olan kritik hatalar
journalctl -p 3 -xb --since "10 minutes ago"

# Dosya kim tarafından silindi? (Auditd kuruluysa)
ausearch -f /etc/passwd
```
