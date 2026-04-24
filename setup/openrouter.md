# OpenRouter Kurulumu

> OpenRouter, 200+ modeli tek bir API arkasında toplayan bir yönlendirme (routing) katmanıdır. Birden fazla ücretsiz model sunar ve kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Maliyet** | Ücretsiz (birçok model $0) |
| **Kredi Kartı** | Gerekmiyor |
| **Resmi Site** | [openrouter.ai](https://openrouter.ai) |
| **API Dokümantasyon** | [openrouter.ai/docs](https://openrouter.ai/docs) |
| **Avantaj** | Tek API anahtarıyla onlarca ücretsiz modele erişim |

OpenRouter, farklı sağlayıcıların modellerini tek bir endpoint arkasında sunar. Ücretsiz katmanda kredi kartı bilgisi gerekmeden birçok güçlü kodlama modeli kullanabilirsiniz. Hesabınıza $10+ bakiye yüklerseniz günlük limitler önemli ölçüde artar.

---

## 2. Hesap Oluşturma

1. [openrouter.ai](https://openrouter.ai) adresine gidin
2. Sağ üstten **Sign Up** butonuna tıklayın
3. Google, GitHub veya e-posta ile kayıt olun
4. E-posta doğrulamasını tamamlayın
5. Hesabınız hazır — kredi kartı eklemenize gerek yok

---

## 3. Kimlik Doğrulama

### API Anahtarı Oluşturma

1. [openrouter.ai/keys](https://openrouter.ai/keys) adresine gidin
2. **Create Key** butonuna tıklayın
3. Anahtarınıza bir isim verin (ör. `opencode-kurs`)
4. Oluşturulan `sk-or-...` ile başlayan anahtarı kopyalayın

> **Uyarı:** API anahtarınızı kimseyle paylaşmayın ve versiyon kontrol sistemine (git) eklemeyin.

---

## 4. OpenCode CLI Bağlantısı

OpenRouter, OpenCode'un yerleşik sağlayıcılarından biridir. Yapılandırma dosyası gerekmez.

### Yöntem 1 — TUI ile (Önerilen)

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
│  ● OpenRouter
└
```

OpenRouter'ı seçin ve API anahtarınızı yapıştırın:

```
┌  Add credential
│
◇  Enter your API key
│  sk-or-...
└
```

### Yöntem 2 — CLI ile

```bash
opencode auth login
# OpenRouter'ı seçin → API anahtarınızı girin
```

### Bağlantıyı Doğrulama

```bash
opencode auth list
```

Çıktıda `openrouter` görünmelidir.

---

## 5. Kodlama İçin Önerilen Modeller

Model seçimi için TUI içinde `/models` komutunu kullanın.

### qwen3-coder (480B parametreli, MoE)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `qwen/qwen3-coder` |
| **Parametreler** | 480B (MoE — aktif parametre sayısı daha düşük) |
| **Bağlam Penceresi** | 256K token |
| **Neden** | OpenRouter'daki en güçlü ücretsiz kodlama modeli. Kod üretimi, hata ayıklama ve refaktörleme konusunda frontier modellerle yarışır. |

### DeepSeek R1

| Özellik | Detay |
|---------|-------|
| **Model ID** | `deepseek/deepseek-r1` |
| **Neden** | Karmaşık algoritmalar ve çok adımlı problem çözümü için idealdir. "Düşünce zinciri" (chain-of-thought) ile çalışır, adım adım mantık yürütür. |

### Llama 4 Maverick

| Özellik | Detay |
|---------|-------|
| **Model ID** | `meta-llama/llama-4-maverick` |
| **Neden** | Meta'nın en güçlü açık kaynak modeli. Genel amaçlı kodlama görevlerinde güvenilir bir alternatiftir. |

> **Tavsiye:** Günlük kodlama görevleri için **qwen3-coder**, karmaşık mimari kararlar ve zor algoritmalar için **DeepSeek R1** kullanın.

---

## 6. Limitler ve İpuçları

### Ücretsiz Katman Limitleri

| Limit | Değer |
|-------|-------|
| **Günlük istek** | ~200 istek/gün |
| **Dakika başı istek (RPM)** | 20 |
| **Bakiyeli hesap ($10+)** | 1.000 istek/gün |

### Kullanımı Optimize Etme İpuçları

- **`/compact` komutunu kullanın:** Uzun sohbetlerde bağlam penceresini sıkıştırarak token tüketimini azaltın
- **Kısa ve net talimatlar verin:** Gereksiz açıklamalardan kaçının, direkt olarak ne istediğinizi belirtin
- **Oturumları ayırın:** Her görev için ayrı bir sohbet başlatın, böylece bağlam penceresi temiz kalır
- **Modeller arası geçiş yapın:** Basit görevler için daha küçük modelleri, karmaşık görevler için R1'i tercih edin

### Dinamik Yönlendirme (İleri Seviye)

OpenRouter, model isteklerinizi farklı altyapı sağlayıcılarına yönlendirebilir. `opencode.json` dosyasında sağlayıcı tercihi ve fallback davranışı yapılandırabilirsiniz:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openrouter": {
      "models": {
        "qwen/qwen3-coder": {
          "options": {
            "provider": {
              "order": ["together"],
              "allow_fallbacks": true
            }
          }
        }
      }
    }
  }
}
```

Detaylı bilgi: [Dinamik Yönlendirme Rehberi](dynamic-routing.md)

---

## 7. Sorun Giderme

### "Rate limit exceeded" hatası

Ücretsiz katmanda günlük veya dakikalık limite ulaştınız.

**Çözüm:**
- Birkaç dakika bekleyin (RPM limiti için) veya ertesi gün tekrar deneyin (günlük limit için)
- Alternatif olarak başka bir ücretsiz sağlayıcıya geçin: `/connect` ile DeepSeek veya Cerebras bağlayın
- Hesabınıza $10+ bakiye yükleyerek limitleri artırın

### "Invalid API key" hatası

**Çözüm:**
- [openrouter.ai/keys](https://openrouter.ai/keys) adresinden anahtarınızın aktif olduğunu doğrulayın
- Anahtarın `sk-or-` ile başladığından emin olun
- Yeniden bağlanın:
  ```
  /connect → OpenRouter → yeni API anahtarı yapıştırın
  ```

### Model listesinde ücretsiz modeller görünmüyor

**Çözüm:**
```bash
opencode models --refresh
```

Bu komut model önbelleğini yeniler ve güncel ücretsiz modelleri listeler.

### Sağlayıcı paketi hatası (AI_APICallError)

**Çözüm — önbelleği temizleyin:**

Windows: `WIN+R` → `%USERPROFILE%\.cache\opencode` klasörünü silin.

macOS/Linux:
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın.

---

## 8. Doğrulama

Kurulumun başarılı olduğunu doğrulamak için:

```bash
opencode auth list
# Çıktıda "openrouter" görünmeli
```

TUI içinde test edin:

```
opencode
# /models → qwen/qwen3-coder seçin
# Test mesajı: "Fibonacci serisinin ilk 20 elemanını hesaplayan bir Python fonksiyonu yaz"
```

Çalışan bir Python kodu görüyorsanız OpenRouter kurulumu tamamdır.

---

*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
