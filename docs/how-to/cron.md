# Zamanlanmış Görevler (Cron) ⏰

Linux sunucularında periyodik işlemler (yedekleme, log temizleme, rapor alma vb.) için **Cron** kullanılır. En temel ve güvenilir otomasyon aracıdır.

---

## 1. Cron Sözdizimi (Syntax)

Bir Cron işi (job) tanımlarken şu 5 zaman dilimini kullanırız:

```text
* * * * * komut
│ │ │ │ │
│ │ │ │ └─── Haftanın Günü (0 - 7) (Pazar = 0 veya 7)
│ │ │ └───── Ay (1 - 12)
│ │ └─────── Ayın Günü (1 - 31)
│ └───────── Saat (0 - 23)
└─────────── Dakika (0 - 59)
```

> [!TIP] > **Yardımcı Araç:** Zamanlamayı görselleştirmek için [crontab.guru](https://crontab.guru/) sitesini kullanabilirsiniz.

---

## 2. Temel Komutlar

Kendi kullanıcınız (root değilse `sudo` kullanmayın) için zamanlanmış görevleri yönetmek:

```bash
# Mevcut görevleri listele
crontab -l

# Görevleri düzenle (Editör açar)
crontab -e

# Tüm görevleri sil (Dikkat!)
crontab -r
```

---

## 3. Yaygın Kullanım Örnekleri

| Zamanlama              | Kod           | Açıklama                                         |
| :--------------------- | :------------ | :----------------------------------------------- |
| **Her Dakika**         | `* * * * *`   | Test amaçlı veya çok sık gereken işler.          |
| **Her 5 Dakikada Bir** | `*/5 * * * *` | `*/` operatörü "her X'te bir" demektir.          |
| **Her Saat Başı**      | `0 * * * *`   | Dakika 0 olduğunda çalışır.                      |
| **Her Gece Yarısı**    | `0 0 * * *`   | Günlük yedekler için idealdir. (00:00)           |
| **Her Pazar 04:00**    | `0 4 * * 0`   | Haftalık bakım işlemleri için.                   |
| **Açılışta (Reboot)**  | `@reboot`     | Sunucu yeniden başladığında **bir kez** çalışır. |

### Örnek Bir Satır:

```bash
# Her gece 03:30'da yedek al
30 03 * * * /home/kullanici/scripts/yedek_al.sh
```

---

## 4. Kullanıcı Crontab vs Sistem Crontab

İki farklı yerde tanımlama yapabilirsiniz. Aralarındaki fark, **"Kullanıcı"** sütunudur.

### A. Kullanıcı Crontab (`crontab -e`)

Sadece o kullanıcının yetkisiyle çalışır. Komut satırında **kullanıcı adı yazılmaz**.

```bash
# Doğru (crontab -e)
0 1 * * * /script.sh
```

### B. Sistem Crontab (`/etc/crontab` veya `/etc/cron.d/*`)

Tüm sistem için geçerlidir. Hangi kullanıcı yetkisiyle çalışacağı **belirtilmelidir**.

```bash
# /etc/cron.d/yedekleme
# Dakika Saat Gun Ay Gun Kullanici Komut
0 1 * * * root /script.sh
```

> [!CAUTION]
> Prodüksiyon ortamlarında `/etc/cron.d/` içine her uygulama için ayrı dosya oluşturmak (Örn: `/etc/cron.d/app-backup`) daha düzenli ve yönetilebilir bir yöntemdir.

---

## 5. Çıktı Yönetimi ve Loglama (Önemli) 📝

Varsayılan olarak Cron, bir çıktı (output) üretirse bunu **mail** atmaya çalışır (çoğu sunucuda mail ayarlı olmadığı için bu mesajlar kaybolur).

### Yöntem 1: Sessize Alma (Silent)

Eğer çıktı önemli değilse (çöp kutusu):

```bash
* * * * * /komut.sh > /dev/null 2>&1
```

### Yöntem 2: Dosyaya Loglama (Önerilen)

Yaptığı işi ve hataları görmek için:

```bash
# >> ile dosyanın sonuna ekle (append)
* * * * * /komut.sh >> /var/log/ozel-is.log 2>&1
```

---

## 6. Sık Yapılan Hatalar (Tuzaklar) 🪤

Cron çalışmıyorsa %90 sebebi şunlardır:

### 1. Çevre Değişkenleri (PATH Sorunu)

Cron, sizin terminalinizdeki `PATH` değişkenlerini (örn. `.bashrc`) bilmez.
**Hata:** `node: command not found`
**Çözüm:** Komutların **tam yolunu** yazın.

- Yanlış: `python3 script.py`
- Doğru: `/usr/bin/python3 /home/user/script.py`

### 2. Yüzde İşareti (`%`)

Cron dosyasında `%` karakteri "yeni satır" anlamına gelir. Eğer komutunuzda `%` varsa (örn: `date +%F`), ters slash ile kaçmalısınız: `\%`.

### 3. Dosya İzinleri

Çalıştırıcak scripti executable yapmayı unutmayın:

```bash
chmod +x /home/user/script.sh
```

### 4. Yeni Satır (Newline)

Cron dosyasının (**özellikle `/etc/cron.d/` altındakilerin**) son satırı boş olmalıdır. Dosya bir komutla bitiyorsa ve enter'a basılmamışsa çalışmayabilir.
