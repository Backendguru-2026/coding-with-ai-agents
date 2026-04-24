# Hugging Face Kurulumu

> Hugging Face, açık kaynak AI modellerin merkezi platformudur. Ücretsiz Inference API ile yüzlerce modeli deneyebilir, en son açık kaynak modellere anında erişim sağlayabilirsiniz. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | Hugging Face (Inference API) |
| **Bağlantı Türü** | opencode.json yapılandırması (`@ai-sdk/openai-compatible`) |
| **Maliyet** | $0 — ücretsiz tier |
| **Kredi Kartı** | Hayır, gerekmiyor |
| **Rate Limit** | Ücretsiz tier'da model ve yoğunluğa göre değişken |
| **Resmi Site** | [huggingface.co](https://huggingface.co) |

**Not:** Hugging Face, açık kaynak dünyasının "GitHub"ıdır. En yeni modeller genellikle ilk olarak burada yayımlanır. Deneysel modelleri denemek ve açık kaynak ekosistemini keşfetmek için ideal bir platformdur.

---

## 2. Hesap Oluşturma

### Adım 1 — Hugging Face Hesabı

1. [huggingface.co/join](https://huggingface.co/join) adresine gidin
2. E-posta adresi, kullanıcı adı ve şifre ile kayıt olun (veya GitHub/Google ile)
3. E-posta doğrulamasını tamamlayın

### Adım 2 — API Token Oluşturma

1. [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) adresine gidin
2. **Create new token** butonuna tıklayın
3. Token adı girin (örn. `opencode`)
4. Token türü olarak **Read** seçin (yazma yetkisi gerekmez)
5. **Generate** butonuna tıklayın
6. Oluşturulan `hf_...` tokenını kopyalayın ve güvenli bir yerde saklayın

> **Uyarı:** Token yalnızca oluşturulduğunda tam olarak gösterilir. Kaybederseniz yeni bir tane oluşturmanız gerekir.

### İşletim Sistemi Notları

- **Windows / macOS / Linux:** Fark yok. Hesap oluşturma tarayıcı üzerinden yapılır, OpenCode CLI yapılandırması tüm platformlarda aynıdır.

---

## 3. Kimlik Doğrulama

Hugging Face, **API token** yöntemiyle kimlik doğrulaması yapar.

### Akış

1. Hugging Face'ten API token oluşturursunuz
2. Token'ı ortam değişkeni olarak ayarlarsınız veya `opencode.json` içinde referans verirsiniz
3. OpenCode CLI'de `/connect` ile "Other" seçeneğinden `huggingface` ID'si ile bağlanırsınız
4. Token'ınızı yapıştırırsınız
5. Bağlantı tamamlanır

Kimlik bilgileri `~/.local/share/opencode/auth.json` dosyasında saklanır.

---

## 4. OpenCode CLI Bağlantısı

Hugging Face, `opencode.json` dosyası ile yapılandırılır. Projenizin kök dizininde bu dosyayı oluşturun.

### Adım 1 — /connect ile Kimlik Doğrulama

1. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

2. `/connect` komutunu yazın:
   ```
   /connect
   ```

3. Sağlayıcı listesinden **Other** seçeneğini seçin:
   ```
   ┌  Add credential
   │
   ◆  Select provider
   │  ...
   │  ● Other
   └
   ```

4. Sağlayıcı ID olarak `huggingface` girin:
   ```
   ┌  Add credential
   │
   ◇  Enter provider id
   │  huggingface
   └
   ```

5. API token'ınızı girin:
   ```
   ┌  Add credential
   │
   ◇  Enter your API key
   │  hf_...
   └
   ```

### Adım 2 — opencode.json Yapılandırması

Projenizin kök dizininde `opencode.json` dosyası oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "huggingface": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Hugging Face",
      "options": {
        "baseURL": "https://router.huggingface.co/v1"
      },
      "models": {
        "Qwen/Qwen3-235B-A22B": {
          "name": "Qwen3 235B (MoE)",
          "limit": {
            "context": 131072,
            "output": 8192
          }
        },
        "mistralai/Mistral-Small-3.2-24B-Instruct-2503": {
          "name": "Mistral Small 3.2 24B",
          "limit": {
            "context": 131072,
            "output": 8192
          }
        }
      }
    }
  }
}
```

> **Önemli:** `baseURL` olarak `https://router.huggingface.co/v1` kullanılır. Bu, Hugging Face'in OpenAI uyumlu Inference API endpoint'idir.

---

## 5. Kodlama İçin Önerilen Modeller

Bağlantı tamamlandıktan sonra `/models` komutuyla model seçebilirsiniz.

### Qwen3 235B (MoE)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `huggingface/Qwen/Qwen3-235B-A22B` |
| **Neden?** | Alibaba'nın en güçlü açık kaynak modeli. 235B parametre (22B aktif MoE) ile frontier modellere yakın kalitede kod üretir. Hugging Face üzerinde ücretsiz sunulması büyük avantajdır. |
| **En iyi kullanım** | Karmaşık kod üretimi, mimari tasarım, çok dosyalı düzenlemeler |

### Mistral Small 3.2 24B

| Özellik | Detay |
|---------|-------|
| **Model ID** | `huggingface/mistralai/Mistral-Small-3.2-24B-Instruct-2503` |
| **Neden?** | Boyutuna göre çok güçlü bir model. Hızlı yanıt süresi ve düşük kaynak tüketimi ile günlük kodlama görevleri için idealdir. Türkçe dahil çok dilli destek sunar. |
| **En iyi kullanım** | Günlük kodlama, hata ayıklama, birim test yazma, basit refactoring |

> **İpucu:** Hugging Face üzerindeki model listesi sürekli güncellenir. [huggingface.co/models](https://huggingface.co/models) adresinden en yeni modelleri keşfedebilir ve `opencode.json` dosyasına ekleyebilirsiniz.

---

## 6. Limitler ve İpuçları

### Kullanım Limitleri

| Limit Türü | Detay |
|------------|-------|
| Rate limit | Model ve yoğunluğa göre değişken |
| Maliyet | $0 (ücretsiz tier) |
| Kuyruk süresi | Yoğun saatlerde modele göre bekleme olabilir |

### İpuçları

- **En yeni modeller:** Hugging Face, açık kaynak modellerin ilk yayımlandığı yerdir. Yeni bir model çıktığında genellikle birkaç gün içinde Inference API'da kullanılabilir hale gelir
- **Model keşfi:** [huggingface.co/models?pipeline_tag=text-generation](https://huggingface.co/models?pipeline_tag=text-generation&sort=trending) üzerinde trending modelleri takip edin
- **opencode.json güncelleme:** Yeni bir modeli denemek için `opencode.json` dosyasındaki `models` bölümüne eklemeniz yeterli
- **Bağlamı küçük tutun:** Ücretsiz tier'da çıktı token limitleri olabilir; gereksiz dosyaları bağlama eklemeyin
- **Yedek sağlayıcı:** Kuyruk uzunsa başka bir ücretsiz sağlayıcıya (Groq, Cerebras) geçebilirsiniz

---

## 7. Sorun Giderme

### "Authorization" veya "Invalid token" hatası

- Token'ın doğru kopyalandığından emin olun (`hf_` ile başlamalı)
- [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) adresinden token'ın aktif olduğunu doğrulayın
- Token türünün en az **Read** yetkisine sahip olduğundan emin olun

### "Model is currently loading" mesajı

- Ücretsiz tier'da modeller talep üzerine yüklenir ve bu birkaç dakika sürebilir
- Birkaç dakika bekleyip tekrar deneyin
- Popüler modeller genellikle zaten yüklü durumda olur

### Model listesinde Hugging Face modelleri görünmüyor

- `opencode.json` dosyasının projenizin kök dizininde olduğundan emin olun
- JSON sözdizimini kontrol edin (virgüller, tırnak işaretleri)
- `/connect` ile kimlik doğrulamayı yaptığınızdan emin olun
- `opencode models --refresh` komutuyla model önbelleğini temizleyin

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
   Listede `huggingface/Qwen/Qwen3-235B-A22B` gibi `opencode.json` dosyasında tanımladığınız modeller görünmelidir.

3. Test mesajı gönderin:
   ```
   Bir string'i tersine çeviren Python fonksiyonu yaz
   ```

4. Çalışan bir Python fonksiyonu üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir string-tersine-çevirme fonksiyonu. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
