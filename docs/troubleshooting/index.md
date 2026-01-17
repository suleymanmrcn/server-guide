# Metodolojik Sorun Giderme 🕵️‍♂️

Bir sorun anında (Incident) rastgele komutlar yazmak (`reboot`, `systemctl restart`) amatörlüktür. Senior bir mühendis **bilimsel yöntem** izler.

Bu bölümde, Google SRE ve Netflix mühendislerinin kullandığı iki temel metodolojiyi esas alacağız.

## 1. The USE Method (Kaynak Analizi) 🧱

Donanım ve işletim sistemi kaynaklarını analiz etmek için Brendan Gregg tarafından geliştirilmiştir. Her kaynak için 3 soruyu sorarız:

- **Utilization (Kullanım):** Kaynak ne kadar meşgul? (%100 dolu mu?)
- **Saturation (Doygunluk):** Bekleyen iş var mı? (Kuyruk oluştu mu?)
- **Errors (Hatalar):** Hata üretiliyor mu?

| Kaynak      | Utilization           | Saturation                   | Errors                |
| :---------- | :-------------------- | :--------------------------- | :-------------------- |
| **CPU**     | `mpstat -P ALL 1`     | `vmstat 1` (r > cpu count)   | `perf`, `dmesg`       |
| **RAM**     | `free -m`             | `vmstat 1` (si/so - swap)    | `dmesg` (OOM Kill)    |
| **Disk**    | `iostat -x 1` (%util) | `iostat -x 1` (avgqu-sz > 1) | `dmesg` (IO Error)    |
| **Network** | `sar -n DEV 1`        | `ip -s link` (dropped)       | `ip -s link` (errors) |

## 2. The RED Method (Servis Analizi) 🚦

Mikroservisler ve uygulamalar için Tom Wilkie tarafından geliştirilmiştir. Kullanıcı deneyimine odaklanır.

- **Rate (Hız):** Saniyede gelen istek sayısı (RPS).
- **Errors (Hatalar):** Başarısız isteklerin oranı (HTTP 500).
- **Duration (Süre):** İsteklerin cevaplanma süresi (Latency - p99).

> [!TIP] > **Önce RED, Sonra USE:**  
> Bir sorun olduğunda önce **RED** ile "Kullanıcı etkileniyor mu?" diye bakın. Eğer etkileniyorsa, **USE** ile "Hangi kaynak darboğazda?" sorusunu cevaplayın.

## 3. Bilimsel Yaklaşım Döngüsü 🔬

1.  **Gözlem:** "Site yavaş" (Vague) -> "Latency p99 2 saniyeye çıktı" (Specific).
2.  **Hipotez:** "Database CPU'su şişmiş olabilir."
3.  **Test:** Database CPU metriklerine bak.
4.  **Kanıt:** Evet, CPU %100. Veya Hayır, %10.
5.  **Aksiyon:** Hipotez doğruysa çözüm uygula, yanlışsa 2. adıma dön.
