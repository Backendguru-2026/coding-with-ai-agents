# NVIDIA NIM Kurulumu

> NVIDIA NIM (NVIDIA Inference Microservices), NVIDIA'nın bulut üzerinden ücretsiz sunduğu çıkarım (inference) platformudur. Llama, Mistral gibi güçlü açık kaynak modellere ücretsiz erişim sağlar. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | NVIDIA NIM (NVIDIA AI) |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı + opencode.json (özelleştirme için) |
| **Maliyet** | $0 — ücretsiz tier |
| **Kredi Kartı** | Hayır, gerekmiyor |
| **Rate Limit** | ~40 RPM (dakikada istek) |
| **Resmi Site** | [build.nvidia.com](https://build.nvidia.com) |

**Not:** NVIDIA NIM, NVIDIA GPU altyapısı üzerinde çalışan açık kaynak modellere ücretsiz erişim sunar. Özellikle Llama ve Mistral ailesi modeller yüksek performansla çalışır.

---

## 2. Hesap Oluşturma

### Adım 1 — NVIDIA Hesabı

1. [build.nvidia.com](https://build.nvidia.com) adresine gidin
2. Sağ üst köşedeki **Sign In** / **Join** butonuna tıklayın
3. E-posta adresi ile kayıt olun veya Google/GitHub hesabıyla giriş yapın
4. E-posta doğrulamasını tamamlayın

### Adım 2 — API Anahtarı Oluşturma

1. [build.nvidia.com](https://build.nvidia.com) üzerinde herhangi bir modelin sayfasına gidin
2. **Get API Key** veya **Generate Key** butonuna tıklayın
3. Oluşturulan `nvapi-...` anahtarını kopyalayın ve güvenli bir yerde saklayın

> **Uyarı:** API anahtarı yalnızca bir kez gösterilir. Kaybederseniz yeni bir tane oluşturmanız gerekir.

### İşletim Sistemi Notları

- **Windows / macOS / Linux:** Fark yok. Hesap oluşturma tarayıcı üzerinden yapılır, OpenCode CLI bağlantısı tüm platformlarda aynıdır.

---

## 3. Kimlik Doğrulama

NVIDIA NIM, **API anahtarı** yöntemiyle kimlik doğrulaması yapar.

### Akış

1. NVIDIA Build portalından API anahtarı oluşturursunuz
2. OpenCode CLI'de `/connect` komutunu çalıştırırsınız
3. Sağlayıcı listesinden NVIDIA'yı seçersiniz (veya "Other" ile `nvidia` ID'si girersiniz)
4. API anahtarınızı yapıştırırsınız
5. Bağlantı tamamlanır

Kimlik bilgileri `~/.local/share/opencode/auth.json` dosyasında saklanır.

---

## 4. OpenCode CLI Bağlantısı

NVIDIA, OpenCode CLI'de yerleşik sağlayıcı olarak bulunur. Temel kullanım için `/connect` yeterlidir. Özel model veya endpoint tanımlamak isterseniz `opencode.json` kullanabilirsiniz.

### Yöntem 1 — /connect ile Bağlantı (Önerilen)

1. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

2. `/connect` komutunu yazın:
   ```
   /connect
   ```

3. Sağlayıcı listesinden **NVIDIA** seçeneğini seçin:
   ```
   ┌  Add credential
   │
   ◆  Select provider
   │  ...
   │  ● NVIDIA
   │  ...
   └
   ```

4. API anahtarınızı girin:
   ```
   ┌  Add credential
   │
   ◇  Enter your API key
   │  nvapi-...
   └
   ```

### Yöntem 2 — opencode.json ile Özelleştirme

Projenizin kök dizininde `opencode.json` dosyası oluşturun. Bu yöntem, yerleşik sağlayıcıda bulunmayan ek modelleri tanımlamak veya özel baseURL ayarlamak için kullanışlıdır:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "nvidia": {
      "options": {
        "baseURL": "https://integrate.api.nvidia.com/v1"
      },
      "models": {
        "meta/llama-3.3-70b-instruct": {
          "name": "Llama 3.3 70B"
        },
        "mistralai/mistral-large-2-instruct": {
          "name": "Mistral Large 2"
        }
      }
    }
  }
}
```

> **Not:** Yerleşik sağlayıcıyı kullanırken `npm` alanı gerekmez. Yalnızca `options` veya `models` ile mevcut sağlayıcıyı genişletebilirsiniz.

---

## 5. Kodlama İçin Önerilen Modeller

Bağlantı tamamlandıktan sonra `/models` komutuyla model seçebilirsiniz.

### Llama 3.3 70B Instruct

| Özellik | Detay |
|---------|-------|
| **Model ID** | `nvidia/meta/llama-3.3-70b-instruct` |
| **Neden?** | Meta'nın en güçlü açık kaynak modellerinden biri. Kod üretimi, hata ayıklama ve refactoring'de yüksek kalite sunar. 70B parametre sayısı sayesinde karmaşık görevlerde tutarlı sonuçlar verir. |
| **En iyi kullanım** | Kod yazma, hata ayıklama, mimari kararlar, kod inceleme |

### Mistral Large 2 Instruct

| Özellik | Detay |
|---------|-------|
| **Model ID** | `nvidia/mistralai/mistral-large-2-instruct` |
| **Neden?** | Mistral'in en büyük modeli. Çok dilli destek (Türkçe dahil) ve güçlü talimat takibi. Uzun kod blokları üretmede başarılıdır. |
| **En iyi kullanım** | Çok dosyalı düzenlemeler, dokümantasyon üretimi, karmaşık refactoring |

> **Not:** NVIDIA NIM üzerinde sunulan model listesi zamanla değişebilir. `/models` komutuyla güncel listeyi kontrol edin.

---

## 6. Limitler ve İpuçları

### Kullanım Limitleri

| Limit Türü | Detay |
|------------|-------|
| Rate limit | ~40 RPM (dakikada istek) |
| Maliyet | $0 (ücretsiz tier) |
| Token limiti | Model başına değişken |

### İpuçları

- **Rate limit'e dikkat:** 40 RPM sınırı, aktif kodlama seanslarında yeterli olsa da çok hızlı art arda istek gönderirseniz kısa süreli bekleme gerekebilir
- **Model keşfi:** [build.nvidia.com](https://build.nvidia.com) üzerinde yeni eklenen modelleri düzenli olarak kontrol edin; NVIDIA sürekli yeni modeller ekliyor
- **Bağlamı küçük tutun:** Gereksiz dosyaları bağlama eklemeyin; bu hem hız kazandırır hem rate limit'e takılma riskinizi azaltır
- **Yedek sağlayıcı:** Rate limit'e takıldığınızda başka bir ücretsiz sağlayıcıya (Groq, Cerebras) geçebilirsiniz

---

## 7. Sorun Giderme

### "Invalid API key" veya "Unauthorized" hatası

- API anahtarının doğru kopyalandığından emin olun (`nvapi-` ile başlamalı)
- [build.nvidia.com](https://build.nvidia.com) adresinden anahtarın aktif olduğunu doğrulayın
- Anahtarı yeniden oluşturmayı deneyin

### "Rate limit exceeded" hatası

- 40 RPM sınırına ulaştınız; 1-2 dakika bekleyin
- Daha az sıklıkta istek gönderin
- Alternatif olarak geçici süreliğine başka bir sağlayıcı kullanın

### Model listesinde NVIDIA modelleri görünmüyor

- `/connect` ile bağlantıyı yenileyip tekrar deneyin
- `opencode models --refresh` komutuyla model önbelleğini temizleyin
- `opencode.json` kullandıysanız JSON sözdizimini kontrol edin

### Sağlayıcı paketi hatası (AI_APICallError)

OpenCode sağlayıcı paketlerini dinamik olarak indirir. Hata alırsanız önbelleği temizleyin:

**Windows:**
`WIN+R` → `%USERPROFILE%\.cache\opencode` klasörünü silin.

**macOS/Linux:**
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın — paketler otomatik indirilecektir.

---

## 8. Doğrulama

Bağlantının başarılı olduğunu doğrulamak için aşağıdaki adımları izleyin:

1. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

2. Model seçin:
   ```
   /models
   ```
   Listede `nvidia/meta/llama-3.3-70b-instruct` gibi modeller görünmelidir.

3. Test mesajı gönderin:
   ```
   Bir listedeki en büyük sayıyı bulan Python fonksiyonu yaz
   ```

4. Çalışan bir Python fonksiyonu üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir max-bulma fonksiyonu. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
