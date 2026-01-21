# İleri Seviye Güvenlik (SIEM & EDR) 🚀

Bu rehberde anlatılan araçlar (Lynis, UFW, Fail2Ban) **tekil sunucular (Host-Based)** için mükemmeldir. Ancak sunucu sayınız arttığında veya kurumsal bir yapıya geçtiğinizde, merkezi bir yönetim ve daha derin analiz gerekir.

Bu sayfa, projenizin bir sonraki aşamasında (enterprise-ready) kullanabileceğiniz araçları tanıtır.

## 1. Wazuh (SIEM & XDR)

**"Açık Kaynaklı Güvenlik Platformu"**

Wazuh, sunucularınızdan logları toplar, analiz eder ve bir saldırı anında alarm üretir.

- **Özellik:** Merkezi Log Yönetimi, Dosya Bütünlüğü (FIM), Malware Tespiti, Docker İzleme.
- **Mimari:** Her sunucuya bir "Wazuh Agent" kurulur, merkezi bir "Wazuh Manager" sunucusuna veri gönderir.
- **Maliyet:** Open Source (Bedava).
- **Ne Zaman Kullanılmalı?** 3+ sunucunuz olduğunda ve merkezi log yönetimi gerektiğinde.

## 2. Osquery (EDR)

**"SQL ile Sistem Sorgulama"**

Facebook tarafından geliştirilen bu araç, işletim sistemini **ilişkisel bir veritabanı** gibi görmenizi sağlar.

- **Özellik:** Sistem durumunu SQL sorgularıyla analiz etme.
- **Örnek Sorgu:**
  ```sql
  SELECT pid, name, path FROM processes WHERE name = 'nginx';
  SELECT * FROM listening_ports WHERE port = 22;
  ```
- **Ne Zaman Kullanılmalı?** Filo yönetimi (Fleet Management) ve derinlemesine sistem analizi için.

## 3. Nessus / OpenVAS (Vulnerability Scanner)

**"Saldırgan Gözüyle Tarama"**

Lynis "içeriden" bakar, Nessus ise "dışarıdan" veya "ağ üzerinden" bakar.

- **Özellik:** Bilinen güvenlik açıklarını (CVE), eski yazılımları ve yanlış konfigürasyonları tarar.
- **Nessus:** Endüstri standardıdır ama ücretlidir (Essentials sürümü sınırlı ücretsiz).
- **OpenVAS (Greenbone):** Tamamen açık kaynaklı alternatifidir.
- **Ne Zaman Kullanılmalı?** PCI-DSS uyumluluğu veya Pen-Test (Sızma Testi) süreçlerinde.

## 4. Karşılaştırma Tablosu

| Araç        | Kategori   | Odak Noktası                     | Kurulum                          |
| :---------- | :--------- | :------------------------------- | :------------------------------- |
| **Lynis**   | Audit      | Hardening & Best Practice        | Çok Kolay                        |
| **Wazuh**   | SIEM / XDR | Log Analizi & Tehdit Avı         | Orta (Merkezi Sunucu Gerektirir) |
| **Osquery** | EDR        | Sistem Envanteri & Sorgulama     | Orta                             |
| **OpenVAS** | Scanner    | Zafiyet Taraması (Vulnerability) | Zor (Ağır bir araçtır)           |

> [!TIP]
> Başlangıç için **Lynis** yeterlidir. Sunucu sayınız 5'i geçtiğinde **Wazuh** kurmayı düşünebilirsiniz.
