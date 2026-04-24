# Mistral (La Plateforme) Kurulumu

> Mistral, kod-özelleşmiş Codestral modeli ve 256K bağlam penceresi ile uzun kodlama oturumları için ideal ücretsiz sağlayıcıdır. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | Mistral (La Plateforme) |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı |
| **Maliyet** | Ücretsiz |
| **Kredi Kartı** | Gerekmiyor (yalnızca telefon doğrulaması) |
| **Kayıt Süresi** | ~3 dakika |
| **En İyi Yönü** | Kod-özelleşmiş model (Codestral), 256K bağlam penceresi |
| **Resmi Site** | [console.mistral.ai](https://console.mistral.ai) |

Mistral, Fransa merkezli bir AI şirketidir. Codestral modeli özellikle kod üretimi, anlama ve dönüştürme için eğitilmiştir. 256K token bağlam penceresi, büyük kod tabanlarıyla çalışırken diğer sağlayıcılara göre belirgin avantaj sağlar.

---

## 2. Hesap Oluşturma

1. [console.mistral.ai](https://console.mistral.ai) adresine gidin
2. **"Sign Up"** butonuna tıklayın
3. E-posta adresi ve şifre ile kayıt olun (veya Google/GitHub ile)
4. E-posta doğrulamasını tamamlayın
5. **Telefon doğrulaması** yapın (SMS ile gelen kodu girin)
6. Giriş yaptıktan sonra sol menüden **"API Keys"** sekmesine gidin
7. **"Create new key"** butonuna tıklayın
8. Anahtara bir isim verin (ör. "opencode") ve oluşturun
9. API anahtarınız ekranda görünecektir — hemen kopyalayın

> **Uyarı:** Anahtar sadece bir kez gösterilir. Kopyalamadan sayfayı kapatırsanız yeni anahtar oluşturmanız gerekir.

> **Not:** Telefon doğrulaması zorunludur ancak kredi kartı bilgisi istenmez.

---

## 3. Kimlik Doğrulama

Mistral, OpenCode'da yerleşik sağlayıcıdır. API anahtarınızı `/connect` komutu ile kaydedin.

API anahtarı formatı:
```
...
```

Mistral API anahtarları belirli bir önek ile başlamaz — rastgele alfanumerik bir dizedir.

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
│  ● Mistral
│
◇  Enter your API key
│  ...
└
```

### Yöntem 2 — CLI ile

```bash
opencode auth login
```

Sağlayıcı olarak **Mistral** seçin ve API anahtarınızı girin.

### Yöntem 3 — opencode.json ile Manuel Yapılandırma

Proje kök dizininde `opencode.json` oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "mistral": {
      "npm": "@ai-sdk/mistral",
      "name": "Mistral",
      "options": {
        "apiKey": "{env:MISTRAL_API_KEY}"
      },
      "models": {
        "codestral-latest": {
          "name": "Codestral",
          "limit": {
            "context": 256000,
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
$env:MISTRAL_API_KEY = "anahtarınız"

# macOS/Linux
export MISTRAL_API_KEY="anahtarınız"
```

---

## 5. Kodlama İçin Önerilen Modeller

Model seçimi TUI'de:
```
/models
```

### Codestral (Birincil ve Tek Tercih)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `codestral-latest` |
| **Bağlam Penceresi** | 256K token |
| **Çıktı Limiti** | 65K token |
| **Uzmanlık** | Kod üretimi, anlama, dönüştürme |

**Neden:** Codestral, Mistral'in özellikle kod görevleri için eğittiği bir modeldir. Genel amaçlı modellerin aksine, kod yapılarını, tasarım kalıplarını ve programlama dillerinin inceliklerini daha iyi anlar. 256K token bağlam penceresi, diğer ücretsiz sağlayıcıların sunduğunun çok üzerindedir:

| Sağlayıcı | Bağlam Penceresi |
|-----------|-----------------|
| **Mistral Codestral** | **256K token** |
| Groq (Llama 3.3) | 128K token |
| Google (Gemini Flash) | 1M token |

> **Not:** Gemini daha büyük bağlam penceresine sahip olsa da, Codestral'in 256K'sı büyük çoğunluk proje için fazlasıyla yeterlidir ve Codestral kod-özelleşmiş olması nedeniyle kodlama görevlerinde daha odaklı sonuçlar verir.

### Ne Zaman Codestral Kullanmalı?

- **Büyük kod tabanları:** Birden fazla dosyayı aynı anda analiz etmeniz gerektiğinde
- **Uzun kodlama oturumları:** 256K bağlam sayesinde sohbet geçmişi daha geç dolar
- **Karmaşık refactoring:** Kod yapılarını anlama ve dönüştürme konusunda uzman
- **Çoklu dil dönüşümleri:** Bir dilden diğerine kod çevirme

---

## 6. Limitler ve İpuçları

### Ücretsiz Katman Limitleri

| Limit | Değer |
|-------|-------|
| **RPM (dakikadaki istek)** | 2 |
| **Aylık token** | 1 milyar (1B) |
| **Bağlam penceresi** | 256K token |
| **Çıktı limiti** | 65K token/istek |

### Düşük RPM, Yüksek Token — Ne Anlama Geliyor?

Mistral'in limitleri diğer sağlayıcılardan farklı bir profil çizer:

- **2 RPM (dakikada 2 istek):** Bu oldukça düşük. Her istekten sonra ~30 saniye beklemeniz gerekebilir
- **1B token/ay:** Bu inanılmaz cömert. Normal kullanımda asla dolmaz

Bu profil, Mistral'i şu senaryolar için ideal yapar:

- **Uzun, detaylı kodlama istekleri:** Tek seferde büyük miktarda kod üretme
- **Karmaşık analiz görevleri:** Büyük dosyaları analiz edip kapsamlı rapor alma
- **Düşünme gerektiren görevler:** Acele etmeden, derinlemesine çalışma

### Kotayı Verimli Kullanma İpuçları

- **İsteklerinizi birleştirin:** 2 RPM limiti nedeniyle, bir mesajda mümkün olduğunca çok şey isteyin
- **Detaylı yazın:** "Bu dosyayı düzelt" yerine "Bu dosyadaki hataları bul, düzelt ve birim testi yaz" şeklinde kapsamlı istekler gönderin
- **Yedek sağlayıcı bulundurun:** Hızlı sorular için Groq'a geçin (`/models`), büyük işler için Mistral'e dönün
- **`/compact` kritik:** Uzun oturumlarda bağlam sıkıştırma kullanarak 256K limiti koruyun

### Önerilen İş Akışı

1. Hızlı sorular ve küçük düzeltmeler → **Groq** (30 RPM, anında yanıt)
2. Büyük kodlama görevleri ve analiz → **Mistral Codestral** (256K bağlam, detaylı yanıt)

---

## 7. Sorun Giderme

### "Unauthorized" veya "401" hatası

- [console.mistral.ai](https://console.mistral.ai) adresinden anahtarın aktif olduğunu doğrulayın
- Anahtarı yeniden kopyalayıp `/connect` ile tekrar girin
- Başında/sonunda boşluk olmadığını kontrol edin
- Telefon doğrulamasını tamamladığınızdan emin olun

### "Too Many Requests" veya "429" hatası

- Bu Mistral'de sık karşılaşılan bir durumdur (2 RPM limiti nedeniyle)
- 30-60 saniye bekleyin ve tekrar deneyin
- Bu bir hata değil, normal çalışma şeklidir — isteklerinizi birleştirerek sıklığı azaltın

### "Request Entity Too Large" hatası

- Gönderdiğiniz bağlam 256K token sınırını aşmış olabilir
- `/compact` komutu ile sohbet geçmişini sıkıştırın
- Gereksiz dosya eklemelerini kaldırın

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

### Telefon doğrulaması sırasında SMS gelmiyor

- Türkiye (+90) numaraları desteklenmektedir
- Birkaç dakika bekleyin, SMS gecikmeli gelebilir
- "Resend code" seçeneğini deneyin
- Farklı bir telefon numarası kullanmayı deneyin

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

Listede `mistral/codestral-latest` veya benzer bir model görünmelidir. Modeli seçin, ardından:

```
Merhaba! Bir TypeScript fonksiyonu yaz: verilen bir dizideki en uzun artan alt diziyi (longest increasing subsequence) bulan bir fonksiyon. Tip güvenliği sağla ve edge case'leri ele al.
```

Detaylı, tip güvenli bir TypeScript kodu üretiliyorsa kurulum başarılıdır. Yanıtın diğer sağlayıcılara göre daha detaylı olabileceğine dikkat edin — Codestral kod görevlerinde derinlemesine yanıt verme eğilimindedir.

**CLI ile hızlı test:**

```bash
opencode run "Binary search algoritmasını TypeScript ile implement et"
```

> **Not:** 2 RPM limiti nedeniyle ilk istek sonrası ikinci isteği göndermek için ~30 saniye beklemeniz gerekebilir. Bu normaldir.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
