# Groq Kurulumu

> Groq, özel LPU donanımı sayesinde en hızlı bulut çıkarım (inference) sunan ücretsiz sağlayıcıdır. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | Groq |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı |
| **Maliyet** | Ücretsiz |
| **Kredi Kartı** | Gerekmiyor |
| **Kayıt Süresi** | ~2 dakika |
| **En İyi Yönü** | En hızlı yanıt süresi, hızlı iterasyon döngüleri |
| **Resmi Site** | [console.groq.com](https://console.groq.com) |

Groq, kendi geliştirdiği LPU (Language Processing Unit) donanımı ile GPU'lardan kat kat hızlı çıkarım sağlar. Yanıt süreleri saniyenin kesirlerinde ölçülür. Hızlı deneme-yanılma döngüleri ve küçük-orta ölçekli kodlama görevleri için idealdir.

---

## 2. Hesap Oluşturma

1. [console.groq.com](https://console.groq.com) adresine gidin
2. **"Sign Up"** butonuna tıklayın
3. Google, GitHub veya e-posta ile kayıt olun
4. E-posta doğrulamasını tamamlayın (e-posta ile kayıt olduysanız)
5. Giriş yaptıktan sonra sol menüden **"API Keys"** sekmesine gidin
6. **"Create API Key"** butonuna tıklayın
7. Anahtara bir isim verin (ör. "opencode") ve **"Submit"** tıklayın
8. API anahtarınız ekranda görünecektir — hemen kopyalayın

> **Uyarı:** Anahtar sadece bir kez gösterilir. Kopyalamadan sayfayı kapatırsanız yeni anahtar oluşturmanız gerekir.

---

## 3. Kimlik Doğrulama

Groq, OpenCode'da yerleşik sağlayıcıdır. API anahtarınızı `/connect` komutu ile kaydedin.

API anahtarı formatı:
```
gsk_...
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
│  ● Groq
│
◇  Enter your API key
│  gsk_...
└
```

### Yöntem 2 — CLI ile

```bash
opencode auth login
```

Sağlayıcı olarak **Groq** seçin ve API anahtarınızı girin.

### Yöntem 3 — opencode.json ile Manuel Yapılandırma

Proje kök dizininde `opencode.json` oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "groq": {
      "npm": "@ai-sdk/groq",
      "name": "Groq",
      "options": {
        "apiKey": "{env:GROQ_API_KEY}"
      },
      "models": {
        "llama-3.3-70b-versatile": {
          "name": "Llama 3.3 70B",
          "limit": {
            "context": 128000,
            "output": 32768
          }
        },
        "qwen-qwq-32b": {
          "name": "Qwen QwQ 32B",
          "limit": {
            "context": 131072,
            "output": 32768
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
$env:GROQ_API_KEY = "gsk_..."

# macOS/Linux
export GROQ_API_KEY="gsk_..."
```

---

## 5. Kodlama İçin Önerilen Modeller

Model seçimi TUI'de:
```
/models
```

### Llama 3.3 70B (Birincil Tercih)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `llama-3.3-70b-versatile` |
| **Bağlam Penceresi** | 128K token |
| **Çıktı Limiti** | 32K token |
| **Hız** | Çok hızlı (Groq LPU) |

**Neden:** Genel kodlama görevleri için en güçlü açık kaynak model. Meta'nın Llama 3.3 serisi, kod üretimi, hata ayıklama ve refactoring'de yüksek başarı gösterir. 70B parametre boyutu, kalite ve hız arasında mükemmel denge sunar. Groq'un LPU donanımı sayesinde saniyeler içinde yanıt alırsınız.

### Qwen QwQ 32B (Türkçe İçerik İçin)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `qwen-qwq-32b` |
| **Bağlam Penceresi** | 128K token |
| **Çıktı Limiti** | 32K token |
| **Hız** | Çok hızlı (Groq LPU) |

**Neden:** Alibaba'nın Qwen serisi, çok dilli desteği ile öne çıkar. Türkçe yorumlar, değişken isimleri veya Türkçe dokümantasyon içeren projelerde daha tutarlı sonuçlar verir. Kodlama kalitesi Llama 3.3'e yakındır.

> **Strateji:** İngilizce ağırlıklı kod projelerinde Llama 3.3 70B, Türkçe içerik gerektiren işlerde Qwen QwQ 32B kullanın.

---

## 6. Limitler ve İpuçları

### Ücretsiz Katman Limitleri

| Limit | Değer |
|-------|-------|
| **RPM (dakikadaki istek)** | 30 |
| **Günlük token** | 500.000 |
| **Bağlam penceresi** | 128K token |
| **Çıktı limiti** | 32K token/istek |

### Kotayı Verimli Kullanma İpuçları

- **Kısa mesajlar gönderin:** Groq'un gücü hızdadır — küçük, odaklı istekler gönderin ve hızlı iterasyon yapın
- **500K token/gün yeterlidir:** Normal bir kodlama gününde 500K token fazlasıyla yeterlidir
- **30 RPM oldukça cömert:** Dakikada 30 istek, neredeyse hiç bekleme olmadan çalışmanıza olanak sağlar
- **`/compact` kullanın:** Uzun sohbetlerde bağlam penceresini sıkıştırarak 128K limiti daha verimli kullanın
- **Büyük dosyalarda dikkat:** 128K bağlam penceresi Gemini'nin 1M'sine göre daha küçüktür — çok büyük dosyalar eklemeyin

### Groq'un Avantajı: Hız

Groq'un en büyük avantajı yanıt hızıdır. Bu, şu iş akışları için idealdir:

- **Hızlı prototipleme:** Fikir → kod → test döngüsünü saniyeler içinde tamamlayın
- **Deneme-yanılma:** Farklı yaklaşımları hızlıca deneyin
- **Küçük düzeltmeler:** Hata düzeltme, değişken isimlendirme, tip dönüşümleri

---

## 7. Sorun Giderme

### "Invalid API Key" hatası

- [console.groq.com/keys](https://console.groq.com/keys) adresinden anahtarın aktif olduğunu doğrulayın
- Anahtarı yeniden kopyalayıp `/connect` ile tekrar girin
- Anahtarın `gsk_` ile başladığından emin olun
- Başında/sonunda boşluk olmadığını kontrol edin

### "Rate limit exceeded" veya "429" hatası

- RPM limiti aşılmış olabilir — 1-2 dakika bekleyin
- Günlük token kotası dolmuş olabilir — ertesi gün sıfırlanır (UTC gece yarısı)
- Alternatif: Google Gemini veya Mistral'e geçiş yapın (`/models`)

### Yanıt kesildi (output truncated)

- Groq'un çıktı limiti istek başına 32K tokendir
- Çok uzun kod üretimi istiyorsanız, isteği parçalara bölün
- Örnek: "Önce fonksiyon imzalarını yaz, sonra implementasyonu ekle"

### Model listede görünmüyor

```bash
opencode models --refresh
```

Bu komut model önbelleğini yeniler.

### Sağlayıcı paketi hatası (AI_APICallError)

OpenCode önbelleğini temizleyin:

**Windows:** `WIN+R` ile `%USERPROFILE%\.cache\opencode` klasörünü silin.

**macOS/Linux:**
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın.

### Yavaş yanıt alıyorum

Bu Groq için olağandışıdır. Kontrol edin:

- İnternet bağlantı hızınız
- VPN/proxy kullanıyorsanız geçici olarak devre dışı bırakın
- [status.groq.com](https://status.groq.com) adresinden servis durumunu kontrol edin

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

Listede `groq/llama-3.3-70b-versatile` veya benzer bir model görünmelidir. Modeli seçin, ardından:

```
Merhaba! Bir Python sınıfı yaz: Stack veri yapısını implement eden, push, pop ve peek metodları olan bir sınıf.
```

Çalışan bir Python kodu üretiliyorsa kurulum başarılıdır. Yanıtın birkaç saniye içinde geldiğine dikkat edin — bu Groq'un hız avantajıdır.

**CLI ile hızlı test:**

```bash
opencode run "FizzBuzz problemini çözen bir Python fonksiyonu yaz"
```

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
