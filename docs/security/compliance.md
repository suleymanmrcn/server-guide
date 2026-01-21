# Uyumluluk ve Standartlar (OpenSCAP)

Sunucu güvenliğinde bir üst seviye, **Uyumluluk (Compliance)** standartlarını karşılamaktır.

Eğer bankacılık, sağlık veya e-ticaret (PCI-DSS) sistemleri kuruyorsanız, **Lynis** yetmez. Denetçiler sizden uluslararası standartlara (NIST, STIG, CIS) uygunluk raporu isterler. İşte burada **OpenSCAP** devreye girer.

## 1. Lynis vs OpenSCAP

Bu iki araç rakip değil, tamamlayıcıdır.

| Özellik         | Lynis                       | OpenSCAP                            |
| :-------------- | :-------------------------- | :---------------------------------- |
| **Amaç**        | Genel Sağlık Kontrolü       | Standartlara Uyumluluk (Compliance) |
| **Hedef Kitle** | Sistem Yöneticileri, DevOps | Denetçiler (Auditors), Security Ops |
| **Standartlar** | Genel Linux Best-Practice   | NIST, PCI-DSS, STIG, CIS, HIPAA     |
| **Çıktı**       | Öneri Listesi               | Puanlı Resmi Rapor (HTML/XML)       |
| **Kullanım**    | Kolay, Hızlı                | Detaylı, Yapılandırma Gerektirir    |

## 2. Kurulum (Ubuntu/Debian)

Ubuntu üzerinde OpenSCAP araçları ve güvenlik profilleri (`ssg-deb`) paketlerini kuracağız.

```bash
sudo apt update
sudo apt install libopenscap8 ssg-base ssg-deb ssg-applications ssg-gtK -y
```

> [!TIP] > **Ubuntu Pro** kullanıyorsanız `ubuntu-security-guide` (USG) aracını kullanmak çok daha kolaydır. Ancak standart sürümlerde `oscap` (OpenSCAP) komutunu kullanacağız.

## 3. Güvenlik Profillerini Keşfetme

OpenSCAP, sisteminizi belirli bir "Profil"e göre tarar. Önce sisteminiz için uygun profilleri listeleyin:

```bash
# Ubuntu 22.04 için profil dosyasını bul
oscap info /usr/share/xml/scap/ssg/content/ssg-ubuntu2204-ds.xml
```

**Yaygın Profiller:**

- **xccdf\_...\_profile_cis_level1_server:** CIS Level 1 (Temel Güvenlik - Önerilen)
- **xccdf\_...\_profile_cis_level2_server:** CIS Level 2 (Çok Sıkı - Özellikleri bozabilir)
- **xccdf\_...\_profile_pci_dss:** Kredi Kartı ödeme sistemleri uyumluluğu

## 4. Tarama Başlatma

Aşağıdaki komut, sisteminizi **CIS Level 1** standartlarına göre tarar ve size şık bir HTML raporu oluşturur.

```bash
# CIS Level 1 Taraması
sudo oscap xccdf eval \
 --profile xccdf_org.ssgproject.content_profile_cis_level1_server \
 --results-arf /tmp/results.xml \
 --report /tmp/report.html \
 /usr/share/xml/scap/ssg/content/ssg-ubuntu2204-ds.xml
```

_(Not: `ssg-ubuntu2204-ds.xml` dosya yolu Ubuntu sürümüne göre değişebilir. `/usr/share/xml/scap/ssg/content/` altına bakın.)_

## 5. Raporu Analiz Etme

Oluşan `/tmp/report.html` dosyasını kendi bilgisayarınıza indirin ve tarayıcıda açın.

```bash
# Kendi bilgisayarınızdan
scp root@sunucu-ip:/tmp/report.html ./Masaustu/
```

Raporda her başarısız (Fail) test için:

1.  **Rule ID:** Hangi kuralın ihlal edildiği.
2.  **Severity:** Ciddiyet seviyesi.
3.  **Remediation:** Nasıl düzeltileceği (Bazen hazır bash/ansible scripti bile verir).

> [!WARNING]
> OpenSCAP remediasyon scriptlerini körü körüne çalıştırmayın! Bazı kurallar (örn: `/tmp` noexec yapma) çalışan uygulamalarınızı bozabilir. Önce test ortamında deneyin.
