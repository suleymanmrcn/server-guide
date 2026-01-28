# Sunucu Güvenliği (Hardening) 🛡️

Tebrikler! Bu rehberdeki adımları tamamladıysanız, sunucunuz artık sıradan bir Linux kutusu değil, **Katmanlı Savunma (Defense in Depth)** ile korunan bir kaledir.

Aşağıdaki liste, uyguladığımız tüm güvenlik katmanlarının özetidir.

## 🧱 Katmanlı Güvenlik Mimarisi (Defense in Depth)

Modern sunucu güvenliği, tek bir önleme değil, **birbirini tamamlayan çok katmanlı savunma** stratejisine dayanır. Bir katman aşılsa bile, diğer katmanlar saldırganı durdurur.

### Temel Güvenlik Katmanları

**Sistem Temeli:**  
Güvenlik, temiz bir sistemle başlar. [Gereksiz servisleri temizleyerek](services.md) saldırı yüzeyini küçültün, [otomatik güncellemelerle](updates.md) yazılım açıklarını kapatın ve [kernel parametrelerini](sysctl.md) sıkılaştırarak network saldırılarını engelleyin.

**Erişim Kontrolü:**  
[SSH'ı sertleştirin](ssh.md) (port değiştirme, root girişi kapatma, anahtar kullanımı), [2FA ekleyin](2fa.md) ve [firewall kurallarıyla](firewall.md) sadece gerekli portları açın. [CrowdSec](crowdsec.md) veya [Fail2ban](fail2ban.md) ile brute-force saldırılarını otomatik engelleyin.

**Dosya Sistemi ve Bütünlük:**  
[/tmp dizinini sertleştirerek](tmp-hardening.md) zararlı script çalıştırılmasını önleyin, [AIDE ile dosya bütünlüğünü](fim.md) izleyin ve yetkisiz değişiklikleri tespit edin.

**Kaynak ve Kısıtlamalar:**  
[Derleyicileri kısıtlayarak](compilers.md) sunucuda zararlı yazılım derlenmesini engelleyin, [CPU/RAM limitleriyle](resource-limits.md) crypto miner gibi kaynak tüketen saldırıları boğun.

**Uygulama Güvenliği:**  
[Şifreleri güvenli yönetin](secrets.md) (`.env`, Vault, Docker Secrets), [Docker konteynerlerini](docker.md) izole edin (non-root, read-only FS, UserNS) ve [monitoring ile](monitoring.md) anomalileri tespit edin.

**Malware Koruması:**  
[ClamAV ve Rkhunter](malware.md) ile düzenli zararlı taraması yapın.

### İleri Seviye Katmanlar

Temel güvenlik sağlandıktan sonra, [Bastion Host](bastion.md) ile erişim noktalarını merkezileştirin, [Honeypot ve Tarpit](tarpit.md) ile saldırganları tuzağa düşürün, [OpenSCAP](compliance.md) ile uyumluluk standartlarını kontrol edin ve [SIEM/EDR](advanced-tools.md) araçlarıyla kurumsal düzeyde tehdit analizi yapın.

Tüm katmanların etkinliğini düzenli olarak [Lynis](lynis.md) ile test edin.

---

## 🚀 Uygulama Sırası (Roadmap)

Sıfırdan kuruluma başladıysanız bu sırayı takip edin:

### Aşama 1: Temel (İlk Kurulum)

1.  [Servisleri Temizle](services.md)
2.  [Güncellemeleri Aç](updates.md)
3.  [SSH Ayarla](ssh.md)
4.  [Firewall Aç](firewall.md)

### Aşama 2: Sıkılaştırma (Hardening)

5.  [Kernel Ayarları](sysctl.md)
6.  [Fail2ban/Crowdsec Kur](crowdsec.md)
7.  [/tmp Hardening](tmp-hardening.md)
8.  [Derleyicileri Kısıtla](compilers.md)

### Aşama 3: İleri Seviye (Paranoya Modu)

9.  [SSH 2FA Ekle](2fa.md)
10. [Docker Hardening Uygula](docker.md)
11. [Monitoring Scriptlerini Kur](monitoring.md)
12. [Her Şeyi Tara (Lynis)](lynis.md)

---

## Hangisini Seçmeliyim: CrowdSec mi Fail2ban mi?

| Kriter        | [Fail2ban](fail2ban.md) 🐍           | [CrowdSec](crowdsec.md) 🐹                                   |
| :------------ | :----------------------------------- | :----------------------------------------------------------- |
| **Teknoloji** | Python (Eski, Güvenilir)             | Go (Modern, Bulut Tabanlı)                                   |
| **Koruma**    | **Reaktif:** Size saldırırsa banlar. | **Proaktif:** Dünyada birine saldıranı size gelmeden banlar. |
| **Öneri**     | 512MB RAM altı sunucular için.       | Modern, 1GB+ RAM sunucular için (**Önerilen**).              |

> [!TIP]
> Güvenlik bir varış noktası değil, yolculuktur. Haftalık [Lynis](lynis.md) taramalarınızı aksatmayın!
