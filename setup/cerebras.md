# Cerebras Kurulumu

> Cerebras, özel silikon (wafer-scale) çipleri sayesinde son derece hızlı AI çıkarımı sunan bir sağlayıcıdır. Ücretsiz API erişimi sağlar ve kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Maliyet** | Ücretsiz |
| **Kredi Kartı** | Gerekmiyor |
| **Resmi Site** | [cerebras.ai](https://cerebras.ai) |
| **API Platformu** | [cloud.cerebras.ai](https://cloud.cerebras.ai) |
| **Avantaj** | Olağanüstü hızlı çıkarım — özel donanım sayesinde saniyede binlerce token |

Cerebras, dünyanın en büyük çipini (Wafer-Scale Engine) kullanarak AI çıkarımı yapar. Bu özel silikon, geleneksel GPU tabanlı çıkarıma kıyasla çok daha düşük gecikme ve yüksek token/saniye hızı sunar. Kodlama sırasında yanıt bekleme süresi neredeyse sıfıra iner.

---

## 2. Hesap Oluşturma

1. [cloud.cerebras.ai](https://cloud.cerebras.ai) adresine gidin
2. **Sign Up** veya **Get Started** butonuna tıklayın
3. Google, GitHub veya e-posta ile kayıt olun
4. E-posta doğrulamasını tamamlayın
5. Hesabınız hazır — kredi kartı gerekmez

---

## 3. Kimlik Doğrulama

### API Anahtarı Oluşturma

1. [cloud.cerebras.ai](https://cloud.cerebras.ai) adresinde giriş yapın
2. Sol menüden **API Keys** bölümüne gidin
3. **Create API Key** butonuna tıklayın
4. Anahtarınıza bir isim verin (ör. `opencode-kurs`)
5. Oluşturulan `csk-...` ile başlayan anahtarı kopyalayın

> **Uyarı:** API anahtarınızı kimseyle paylaşmayın ve versiyon kontrol sistemine (git) eklemeyin.

---

## 4. OpenCode CLI Bağlantısı

Cerebras, OpenCode'da `opencode.json` yapılandırma dosyası ile eklenir. Projenizin kök dizininde bu dosyayı oluşturun.

### Adım 1: opencode.json Oluşturma

Projenizin kök dizininde `opencode.json` dosyası oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "cerebras": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Cerebras",
      "options": {
        "baseURL": "https://api.cerebras.ai/v1"
      },
      "models": {
        "qwen-3-235b": {
          "name": "Qwen3 235B",
          "limit": {
            "context": 131072,
            "output": 65536
          }
        },
        "llama-3.3-70b": {
          "name": "Llama 3.3 70B",
          "limit": {
            "context": 131072,
            "output": 65536
          }
        }
      }
    }
  }
}
```

### Adım 2: API Anahtarını Bağlama

```
opencode
```

TUI açıldıktan sonra:

```
/connect
```

```
┌  Add credential
│
◆  Select provider
│  ● Other
└
```

"Other" seçeneğini seçin. OpenCode, `opencode.json` dosyasındaki `cerebras` sağlayıcısını tanıyacaktır:

```
┌  Add credential
│
▲  This only stores a credential for cerebras - you will need to configure it in opencode.json, check the docs for examples.
│
◇  Enter your API key
│  csk-...
└
```

### CLI ile Bağlantı (Alternatif)

```bash
opencode auth login
# "Other" → cerebras → API anahtarınızı girin
```

### Bağlantıyı Doğrulama

```bash
opencode auth list
```

Çıktıda `cerebras` görünmelidir.

> **Not:** Sağlayıcı ID'si (`cerebras`) hem `opencode.json` dosyasında hem de `/connect` komutunda aynı olmalıdır.

---

## 5. Kodlama İçin Önerilen Modeller

Model seçimi için TUI içinde `/models` komutunu kullanın.

### Qwen3 235B (En Güçlü Ücretsiz Kodlama Modeli)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `qwen-3-235b` |
| **Parametreler** | 235 milyar |
| **Bağlam Penceresi** | 128K token |
| **Neden** | Ücretsiz olarak erişilebilen en büyük kodlama modeli. 235B parametre ile kod üretimi, hata ayıklama, refaktörleme ve çok adımlı problem çözümünde güçlü performans gösterir. Cerebras'ın hızlı çıkarımı sayesinde bu dev model bile saniyeler içinde yanıt verir. |

### Llama 3.3 70B

| Özellik | Detay |
|---------|-------|
| **Model ID** | `llama-3.3-70b` |
| **Parametreler** | 70 milyar |
| **Bağlam Penceresi** | 128K token |
| **Neden** | Meta'nın güvenilir açık kaynak modeli. Daha az token tüketir ve basit görevlerde Qwen3 kadar hızlı çalışır. Token limiti konusunda endişeniz varsa günlük işler için iyi bir alternatiftir. |

> **Tavsiye:** Cerebras'ın hız avantajını kullanın — **Qwen3 235B** ile başlayın. Bu boyuttaki bir modelin bu kadar hızlı yanıt vermesi yalnızca Cerebras'a özgüdür. Günlük token limitine dikkat etmeniz gereken durumlarda **Llama 3.3 70B**'ye geçin.

---

## 6. Limitler ve İpuçları

### Ücretsiz Katman Limitleri

| Limit | Değer |
|-------|-------|
| **Dakika başı istek (RPM)** | 30 |
| **Günlük token limiti** | 1.000.000 token/gün |
| **Eşzamanlı istek** | Sınırlı |

### Kullanımı Optimize Etme İpuçları

- **Hız avantajını değerlendirin:** Cerebras'ın en büyük farkı hızdır — 235B model bile milisaniyeler içinde yanıt verir. Hızlı iterasyon gerektiren geliştirme döngülerinde idealdir.
- **Token limitine dikkat edin:** 1M token/gün yeterli görünse de büyük kod tabanlarıyla çalışırken hızla tükenebilir. Bağlam penceresini küçük tutun.
- **`/compact` kullanın:** Uzun sohbetlerde bağlam penceresini sıkıştırarak günlük token limitinizi koruyun
- **Birden fazla sağlayıcı bağlayın:** Cerebras limitine ulaştığınızda `/models` ile OpenRouter veya DeepSeek'e geçiş yapın

---

## 7. Sorun Giderme

### "Rate limit exceeded" hatası

Dakikalık (30 RPM) veya günlük (1M token) limite ulaştınız.

**Çözüm:**
- RPM limiti için 1-2 dakika bekleyin
- Günlük limit için ertesi güne kadar başka bir sağlayıcıya geçin:
  ```
  /models → openrouter veya deepseek modeli seçin
  ```

### "Invalid API key" veya "Unauthorized" hatası

**Çözüm:**
- [cloud.cerebras.ai](https://cloud.cerebras.ai) adresinden anahtarınızın aktif olduğunu doğrulayın
- Anahtarın `csk-` ile başladığından emin olun
- `opencode.json` dosyasındaki sağlayıcı ID'sinin `cerebras` olduğunu kontrol edin
- Yeniden bağlanın:
  ```
  /connect → Other → cerebras → yeni API anahtarı yapıştırın
  ```

### Model bulunamadı hatası

`opencode.json` dosyasındaki model ID'leri Cerebras API'sinin beklediği isimlerle eşleşmelidir.

**Çözüm:**
- `opencode.json` dosyasındaki model ID'lerini kontrol edin (`qwen-3-235b`, `llama-3.3-70b`)
- Model önbelleğini yenileyin:
  ```bash
  opencode models --refresh
  ```

### Sağlayıcı paketi hatası (AI_APICallError)

**Çözüm — önbelleği temizleyin:**

Windows: `WIN+R` → `%USERPROFILE%\.cache\opencode` klasörünü silin.

macOS/Linux:
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın. `@ai-sdk/openai-compatible` paketi otomatik olarak indirilecektir.

> **İpucu:** OpenCode dokümanlarında Cerebras için `@ai-sdk/cerebras` paketi de referans gösterilir. Ancak `@ai-sdk/openai-compatible` ile `baseURL` belirtmek daha esnektir ve ek paket kurulumu gerektirmez.

---

## 8. Doğrulama

Kurulumun başarılı olduğunu doğrulamak için:

```bash
opencode auth list
# Çıktıda "cerebras" görünmeli
```

TUI içinde test edin:

```
opencode
# /models → qwen-3-235b seçin
# Test mesajı: "Binary search algoritmasını iteratif ve rekürsif olarak implement eden bir Python modülü yaz"
```

Çalışan bir Python kodu görüyorsanız — ve yanıt hızının diğer sağlayıcılara kıyasla farkını hissediyorsanız — Cerebras kurulumu tamamdır.

---

*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
