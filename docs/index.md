# Sunucu Kurulum El Kitabı

<section class="hero">
  <div class="hero__title">Production-Ready Sunucu Mimarisi: <br>Sıfırdan Canlıya.</div>
  <div class="hero__subtitle">
    Bu rehber, rastgele blog yazılarının birleşimi değildir. 
    <strong>Güvenlik, Performans ve Sürdürülebilirlik</strong> odaklı, 
    savaşta test edilmiş (battle-tested) bir kurulum standardıdır.
  </div>
  <div class="hero__meta">
    <span class="hero__pill">Ubuntu/Debian LTS</span>
    <span class="hero__pill">Security-First</span>
    <span class="hero__pill">No-Nonsense</span>
  </div>
</section>

<section class="highlight-banner">
  <strong>🔥 30 Saniyede Özet:</strong> Modern bir Linux sunucusu sadece "çalışan" değil, "kendini savunan" ve "ne yaptığını anlatan" bir yapıda olmalıdır.
</section>

## Hızlı Başlangıç (Örnek)

Bu rehberdeki standartları uyguladığınızda, sunucu kurulumunuz şu kadar net ve tekrarlanabilir hale gelir:

```bash
# Sunucuyu "Production" seviyesine getiren standart prosedür
./init-server.sh --hostname "web-01" --user "deploy" --secure

# Çıktı:
# [OK] SSH Hardening (Port 2222, Key-only)
# [OK] Firewall (UFW) Configured (Allow: 80, 443, 2222)
# [OK] Fail2Ban & CrowdSec Active
# [OK] Auto-Updates Enabled
# [OK] Monitoring Agent Installed
# -> Sunucu kullanıma hazır.
```

## Manifesto: Neden Bu Rehber?

İnternet üzerindeki "Nginx nasıl kurulur?" makalelerinin %90'ı eksik veya güvensizdir. Bu el kitabı şu prensiplere dayanır:

1.  **Varsayılan Olarak Güvenli (Secure by Default):** "Firewall'u kapatıp deneyelim" yok. Güvenlik bir eklenti değil, temeldir.
2.  **Gereksiz Yük Yok (Zero Bloat):** Sadece işe yarayan, kanıtlanmış araçlar. Grafik arayüzler yok, karmaşık dashboardlar yok.
3.  **Dokümante Edilmiş Kararlar:** "Bunu neden böyle yaptık?" sorusunun cevabı her zaman bellidir.

## Yolculuk Haritası

Sisteminizi kurarken izlemeniz gereken tavsiye edilen yol:

<section class="cards">
  <a href="architecture/" class="card">
    <h3>1. Temel & Mimari</h3>
    <p>Yanlış temelin dönüşü olmaz. Disk yapılandırması, ağ topolojisi ve işletim sistemi seçimi.</p>
  </a>
  <a href="how-to/" class="card">
    <h3>2. Kurulum & Hardening</h3>
    <p>Sunucuyu dış dünyaya kapat, sadece gerekli kapıları aç. SSH, Firewall ve Fail2ban.</p>
  </a>
  <a href="how-to/nginx/" class="card">
    <h3>3. Servis & Yayın</h3>
    <p>Uygulamanı dünyaya aç. Nginx, Reverse Proxy, TLS ve modern web standartları.</p>
  </a>
  <a href="runbooks/" class="card">
    <h3>4. Day 2 Operasyon</h3>
    <p>Kurmak kolaydır, peki ya yaşatmak? Loglama, İzleme, Backup ve Acil Durum (Runbook) planları.</p>
  </a>
</section>

## Katkıda Bulunun

Bu yaşayan bir dokümandır. Hatalı gördüğünüz veya geliştirmek istediğiniz bir nokta varsa, lütfen Pull Request gönderin.
