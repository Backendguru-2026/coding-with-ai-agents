# Google Gemini (AI Studio) Kurulumu

> Google'in Gemini modelleri, en cömert ücretsiz katmana sahip sağlayıcıdır. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | Google Gemini (AI Studio) |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı |
| **Maliyet** | Ücretsiz |
| **Kredi Kartı** | Gerekmiyor |
| **Kayıt Süresi** | ~2 dakika (Google hesabınız varsa) |
| **En İyi Yönü** | En cömert ücretsiz katman, güçlü kodlama performansı |
| **Resmi Site** | [aistudio.google.com](https://aistudio.google.com) |

Google AI Studio, Gemini modellerine ücretsiz erişim sağlar. Hem hızlı hem de yüksek kaliteli kodlama modelleri sunar. OpenCode CLI'de yerleşik (built-in) sağlayıcı olarak desteklenir.

---

## 2. Hesap Oluşturma

1. [aistudio.google.com](https://aistudio.google.com) adresine gidin
2. **Google hesabınızla** giriş yapın (Gmail yeterli)
3. Kullanım koşullarını kabul edin
4. Sol menüden **"Get API key"** sekmesine tıklayın
5. **"Create API key"** butonuna tıklayın
6. Bir Google Cloud projesi seçin veya otomatik oluşturulan projeyi kullanın
7. API anahtarınız ekranda görünecektir — kopyalayın

> **Not:** Google hesabınız yoksa [accounts.google.com](https://accounts.google.com) adresinden oluşturabilirsiniz.

---

## 3. Kimlik Doğrulama

Google Gemini, OpenCode'da yerleşik sağlayıcıdır. API anahtarınızı `/connect` komutu ile kaydedin.

API anahtarı formatı:
```
AIzaSy...
```

Anahtarınız `~/.local/share/opencode/auth.json` dosyasında güvenli şekilde saklanır.

> **Uyarı:** API anahtarınızı asla Git reposuna veya paylaşılan dosyalara yazmayın.

---

## 4. OpenCode CLI Bağlantısı

### Yöntem 1 — TUI ile (Önerilen)

```bash
opencode
```

TUI açıldığında:

```
/connect
```

```
┌  Add credential
│
◆  Select provider
│  ● Google
│
◇  Enter your API key
│  AIzaSy...
└
```

### Yöntem 2 — CLI ile

```bash
opencode auth login
```

Sağlayıcı olarak **Google** seçin ve API anahtarınızı girin.

### Yöntem 3 — opencode.json ile Manuel Yapılandırma

Proje kök dizininde `opencode.json` oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "google": {
      "npm": "@ai-sdk/google",
      "name": "Google Gemini",
      "options": {
        "apiKey": "{env:GOOGLE_GENERATIVE_AI_API_KEY}"
      },
      "models": {
        "gemini-2.5-flash": {
          "name": "Gemini 2.5 Flash",
          "limit": {
            "context": 1048576,
            "output": 65536
          }
        },
        "gemini-2.5-pro": {
          "name": "Gemini 2.5 Pro",
          "limit": {
            "context": 1048576,
            "output": 65536
          }
        }
      }
    }
  }
}
```

Bu yöntemde ortam değişkenini ayarlayın:

```bash
# Windows (PowerShell)
$env:GOOGLE_GENERATIVE_AI_API_KEY = "AIzaSy..."

# macOS/Linux
export GOOGLE_GENERATIVE_AI_API_KEY="AIzaSy..."
```

---

## 5. Kodlama İçin Önerilen Modeller

Model seçimi TUI'de:
```
/models
```

### Gemini 2.5 Flash (Birincil Tercih)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `gemini-2.5-flash` |
| **Bağlam Penceresi** | 1M token |
| **Ücretsiz Limit** | ~1.000 istek/gün |
| **Hız** | Çok hızlı |

**Neden:** Hız/kalite dengesi en iyi model. Günlük kodlama görevleri, hata ayıklama, refactoring ve kod üretimi için ideal. Yüksek günlük istek limiti sayesinde sürekli kullanıma uygun.

### Gemini 2.5 Pro (Zor Görevler İçin)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `gemini-2.5-pro` |
| **Bağlam Penceresi** | 1M token |
| **Ücretsiz Limit** | Sınırlı (Flash'tan az) |
| **Hız** | Orta |

**Neden:** En yüksek kodlama kalitesi. Karmaşık mimari kararlar, büyük refactoring ve zor debug senaryoları için kullanın. Sınırlı ücretsiz kotası nedeniyle sadece gerçekten ihtiyaç duyduğunuzda tercih edin.

> **Strateji:** Günlük işler için Flash, zorlu görevler için Pro kullanın. Bu şekilde ücretsiz kotanızı en verimli kullanırsınız.

---

## 6. Limitler ve İpuçları

### Ücretsiz Katman Limitleri

| Limit | Değer |
|-------|-------|
| **RPM (dakikadaki istek)** | 15 |
| **Günlük istek (Flash)** | ~1.000 |
| **Günlük istek (Pro)** | Daha az (değişken) |
| **Bağlam penceresi** | 1M token |

### Kotayı Verimli Kullanma İpuçları

- **Mesajlarınızı birleştirin:** Tek bir mesajda birden fazla istek gönderin, her cümle için ayrı mesaj yazmayın
- **`/compact` kullanın:** Uzun konuşmalarda bağlam penceresini sıkıştırarak token tasarrufu yapın
- **Flash'ı varsayılan yapın:** Günlük işlerin %90'ı için Flash yeterlidir
- **Pro'yu saklayın:** Sadece karmaşık mimari ve zor hata ayıklama için Pro'ya geçin
- **Büyük dosyaları parçalayın:** 1M token bağlam penceresi cömert olsa da, gereksiz dosya eklemeyin

### Rate Limit Aşıldığında

Rate limit hatasıyla karşılaşırsanız 1 dakika bekleyin. 15 RPM limiti genellikle normal kullanımda aşılmaz.

---

## 7. Sorun Giderme

### "API key not valid" hatası

- AI Studio'da anahtarın aktif olduğunu doğrulayın: [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
- Anahtarı yeniden kopyalayıp `/connect` ile tekrar girin
- Anahtarın başında/sonunda boşluk olmadığından emin olun

### "RESOURCE_EXHAUSTED" veya "429" hatası

- Günlük kota dolmuş olabilir — ertesi gün sıfırlanır
- RPM limiti aşılmış olabilir — 1 dakika bekleyin
- Alternatif: Groq veya Mistral'e geçiş yapın (`/models`)

### Model listede görünmüyor

```bash
opencode models --refresh
```

Bu komut model önbelleğini yeniler. Yeni eklenen modeller bu şekilde görünür hale gelir.

### Sağlayıcı paketi hatası (AI_APICallError)

OpenCode önbelleğini temizleyin:

**Windows:** `WIN+R` ile `%USERPROFILE%\.cache\opencode` klasörünü silin.

**macOS/Linux:**
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın.

### Bağlantı zaman aşımı

- İnternet bağlantınızı kontrol edin
- VPN veya proxy kullanıyorsanız geçici olarak devre dışı bırakın
- Kurumsal ağlarda `aistudio.google.com` erişiminin engelli olmadığını doğrulayın

---

## 8. Doğrulama

Kurulumun başarılı olduğunu doğrulamak için:

```bash
opencode
```

TUI içinde sırasıyla:

```
/models
```

Listede `google/gemini-2.5-flash` veya benzer bir model görünmelidir. Modeli seçin, ardından:

```
Merhaba! Basit bir Python fonksiyonu yaz: verilen bir listenin tekrar eden elemanlarını bulan bir fonksiyon.
```

Çalışan bir Python kodu üretiliyorsa kurulum başarılıdır.

**CLI ile hızlı test:**

```bash
opencode run "Fibonacci dizisinin ilk 10 elemanını yazdıran bir Python scripti yaz"
```

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
