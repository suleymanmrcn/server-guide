# Troubleshooting ve Checklist

```

### Keystore Bilgilerini Görüntüleme

```bash
# Keystore içeriğini listele
keytool -list -v -keystore my-upload-key.keystore

# Alias'ları göster
keytool -list -keystore my-upload-key.keystore

# Sertifika detayları
keytool -list -v -keystore my-upload-key.keystore -alias my-key-alias
```

---

## Checklist: Production Release 📋

- [ ] Keystore oluşturuldu ve güvenli yerde saklandı
- [ ] Keystore şifreleri kaydedildi (password manager)
- [ ] `.gitignore`'a `*.keystore` eklendi
- [ ] `gradle.properties` konfigüre edildi
- [ ] `build.gradle` signing config eklendi
- [ ] `versionCode` ve `versionName` güncellendi
- [ ] ProGuard/R8 aktif (`minifyEnabled true`)
- [ ] Release AAB/APK build edildi
- [ ] İmza doğrulandı (`jarsigner -verify`)
- [ ] Test cihazında çalıştırıldı
- [ ] Google Play Console'da uygulama oluşturuldu
- [ ] App Signing by Google Play aktif
- [ ] AAB upload edildi
- [ ] Release notes eklendi

---

## Güvenlik Best Practices 🔐

1. **Keystore Güvenliği:**
   - Asla Git'e commit etmeyin
   - Birden fazla yerde yedekleyin
   - Şifreli cloud storage kullanın

2. **Şifre Yönetimi:**
   - Güçlü şifreler kullanın (16+ karakter)
   - Password manager kullanın
   - Şifreleri kimseyle paylaşmayın

3. **CI/CD:**
   - Secrets'ı environment variables olarak saklayın
   - Base64 encode edin
   - Logs'da şifreleri expose etmeyin

4. **Google Play App Signing:**
   - Mutlaka aktif edin
   - Upload key ile signing key ayrı olsun
   - Keystore kaybı riskini minimize edin

---

## Eksik Kritik Konular ⚠️

### SHA-256 Fingerprint (Firebase/Google Services İçin)

Firebase, Google Sign-In, Google Maps kullanıyorsanız **mutlaka** gerekli:

```bash
# Release keystore'un SHA-256 fingerprint'ini alın
keytool -list -v -keystore android/app/my-upload-key.keystore -alias my-key-alias

# Çıktıda şunu arayın:
# SHA256: AB:CD:EF:12:34:56:78:90:AB:CD:EF:12:34:56:78:90:...
```

**Firebase Console'a Ekleme:**

1. Firebase Console → Project Settings → Your apps
2. Android app seçin
3. **Add fingerprint** → SHA-256'yı yapıştırın
4. `google-services.json` dosyasını yeniden indirin
5. `android/app/google-services.json` dosyasını güncelleyin

> [!IMPORTANT]
> **Debug ve Release için ayrı fingerprint'ler:**
>
> - Debug: `~/.android/debug.keystore` (otomatik)
> - Release: `my-upload-key.keystore` (sizin oluşturduğunuz)
>
> **Her ikisini de** Firebase'e eklemelisiniz!

### 64-bit Gereksinimi (2019'dan beri zorunlu)

Google Play, 64-bit desteği **zorunlu** kılmıştır:

**android/app/build.gradle:**

```gradle
android {
    defaultConfig {
        ...
        ndk {
            abiFilters "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
        }
    }

    // Split APKs (önerilen)
    splits {
        abi {
            enable true
            reset()
            include "armeabi-v7a", "arm64-v8a"
            universalApk false
            enable gradle.startParameter.taskNames.any { it.contains("Release") }
        }
    }
}
```

**Kontrol:**

```bash
# AAB içeriğini kontrol et
unzip -l android/app/build/outputs/bundle/release/app-release.aab | grep "lib/"

# Şunları görmeli:
# lib/arm64-v8a/    ← 64-bit ARM (zorunlu!)
# lib/armeabi-v7a/  ← 32-bit ARM
```

### Internal Testing Track (İlk Upload İçin)

**Production'a direkt yükleme yapmamalısınız!**

**Doğru Süreç:**

1. **Internal Testing** → Takım içi test (20 kişiye kadar)
2. **Closed Testing** → Beta testerlar (sınırsız)
3. **Open Testing** → Herkese açık beta
4. **Production** → Canlı yayın

**Internal Testing Kurulumu:**

```
Play Console → Testing → Internal testing
→ Create new release
→ Upload AAB
→ Testers → Create email list
→ Add emails (max 100)
→ Save → Review release → Start rollout
```

**Test Linki:**

```
https://play.google.com/apps/internaltest/XXXXXXXX
```

### App Size Optimization

**AAB Boyutunu Küçültme:**

```gradle
// android/app/build.gradle
android {
    buildTypes {
        release {
            // R8 full mode (daha agresif)
            minifyEnabled true
            shrinkResources true

            // Proguard optimization
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    // Unused resources'ları kaldır
    packagingOptions {
        exclude 'META-INF/*.kotlin_module'
        exclude '**/kotlin/**'
        exclude '**/*.txt'
        exclude '**/*.version'
        exclude '**/*.properties'
    }
}
```

**Hermes Engine (React Native 0.70+):**

```javascript
// android/app/build.gradle
project.ext.react = [
    enableHermes: true  // Varsayılan olarak true
]
```
## Hızlı Referans 🚀

| Komut                                   | Açıklama         |
| --------------------------------------- | ---------------- |
| `./gradlew bundleRelease`               | AAB oluştur      |
| `./gradlew assembleRelease`             | APK oluştur      |
| `./gradlew clean`                       | Cache temizle    |
