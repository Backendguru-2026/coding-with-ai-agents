# xAI (Grok) Kurulumu

> xAI, Elon Musk'ın kurduğu yapay zeka şirketidir. Grok modelleri üzerinden ücretsiz $25 kredi ile AI kodlama deneyimi sunar. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | xAI (Grok) |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı |
| **Maliyet** | $0 — kayıt olunca $25 ücretsiz kredi |
| **Kredi Kartı** | Hayır, gerekmiyor |
| **Faturalandırma** | Kredi tükenince kullanıma göre ücretlendirme (isteğe bağlı) |
| **Resmi Site** | [console.x.ai](https://console.x.ai) |

**Not:** $25 ücretsiz kredi, API kullanımı için oldukça cömerttir. Deneysel kullanım ve öğrenme için fazlasıyla yeterlidir.

---

## 2. Hesap Oluşturma

### Adım 1 — xAI Hesabı

1. [console.x.ai](https://console.x.ai) adresine gidin
2. **Sign Up** butonuna tıklayın
3. E-posta adresi veya X (Twitter) hesabıyla kayıt olun
4. E-posta doğrulamasını tamamlayın

### Adım 2 — API Anahtarı Oluşturma

1. [console.x.ai/team/default/api-keys](https://console.x.ai/team/default/api-keys) adresine gidin
2. **Create API Key** butonuna tıklayın
3. Anahtar için bir isim verin (örn. `opencode`)
4. Oluşturulan anahtarı kopyalayın ve güvenli bir yerde saklayın

> **Uyarı:** API anahtarı yalnızca bir kez gösterilir. Kaybederseniz yeni bir tane oluşturmanız gerekir.

### İşletim Sistemi Notları

- **Windows / macOS / Linux:** Fark yok. Hesap oluşturma tarayıcı üzerinden yapılır, OpenCode CLI bağlantısı tüm platformlarda aynıdır.

---

## 3. Kimlik Doğrulama

xAI, **API anahtarı** yöntemiyle kimlik doğrulaması yapar.

### Akış

1. xAI konsolundan API anahtarı oluşturursunuz
2. OpenCode CLI'de `/connect` komutunu çalıştırırsınız
3. Sağlayıcı listesinden xAI'yi seçersiniz
4. API anahtarınızı yapıştırırsınız
5. Bağlantı tamamlanır

Kimlik bilgileri `~/.local/share/opencode/auth.json` dosyasında saklanır.

---

## 4. OpenCode CLI Bağlantısı

xAI, OpenCode CLI'de yerleşik sağlayıcı olarak bulunur. `opencode.json` yapılandırmasına gerek yoktur.

### Yöntem 1 — TUI ile (Önerilen)

1. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

2. `/connect` komutunu yazın:
   ```
   /connect
   ```

3. Sağlayıcı listesinden **xAI** seçeneğini seçin:
   ```
   ┌  Add credential
   │
   ◆  Select provider
   │  ...
   │  ● xAI
   │  ...
   └
   ```

4. API anahtarınızı girin:
   ```
   ┌  Add credential
   │
   ◇  Enter your API key
   │  xai-...
   └
   ```

5. Bağlantı tamamlandıktan sonra `/models` ile mevcut modelleri görüntüleyin:
   ```
   /models
   ```

### Alternatif: CLI Dışından Bağlantı

```bash
opencode auth login
```

---

## 5. Kodlama İçin Önerilen Modeller

Bağlantı tamamlandıktan sonra `/models` komutuyla model seçebilirsiniz.

### Grok 3

| Özellik | Detay |
|---------|-------|
| **Model ID** | `xai/grok-3` |
| **Neden?** | xAI'nin en güçlü modeli. Uzun bağlam desteği ve güçlü muhakeme yeteneği ile karmaşık kodlama görevlerinde başarılı sonuçlar üretir. |
| **En iyi kullanım** | Mimari tasarım, karmaşık hata ayıklama, çok dosyalı düzenlemeler |

### Grok 3 Mini

| Özellik | Detay |
|---------|-------|
| **Model ID** | `xai/grok-3-mini` |
| **Neden?** | Daha hızlı ve daha ekonomik. Günlük kodlama görevleri için ideal: hızlı yanıt süresi sayesinde ücretsiz kredinizi verimli kullanırsınız. |
| **En iyi kullanım** | Kod yazma, hata ayıklama, birim test, basit refactoring |

> **Öneri:** Kredinizi verimli kullanmak için günlük görevlerde Grok 3 Mini, karmaşık görevlerde Grok 3 tercih edin.

---

## 6. Limitler ve İpuçları

### Kullanım Limitleri

| Limit Türü | Detay |
|------------|-------|
| Ücretsiz kredi | $25 (kayıt bonusu) |
| Rate limit | API kullanımına bağlı dinamik limitler |
| Token limiti | Model başına değişken |

### İpuçları

- **Kredi takibi:** [console.x.ai](https://console.x.ai) adresinden kalan kredinizi düzenli olarak kontrol edin
- **Mini modeli tercih edin:** Basit görevler için Grok 3 Mini kullanarak kredinizi 3-5 kat daha uzun süre kullanabilirsiniz
- **Bağlamı küçük tutun:** Gereksiz dosyaları bağlama eklemeyin; bu hem hız kazandırır hem kredi tüketimini azaltır
- **Yedek sağlayıcı:** Krediniz bittiğinde ücretsiz sağlayıcılardan birine (OpenRouter, Groq, Cerebras) geçebilirsiniz

---

## 7. Sorun Giderme

### "Invalid API key" hatası

- API anahtarının doğru kopyalandığından emin olun (`xai-` ile başlamalı)
- [console.x.ai/team/default/api-keys](https://console.x.ai/team/default/api-keys) adresinden anahtarın aktif olduğunu doğrulayın
- Anahtarı sildiyseniz yeni bir tane oluşturun

### "Insufficient credits" hatası

- [console.x.ai](https://console.x.ai) adresinden kredi bakiyenizi kontrol edin
- $25 ücretsiz kredi tükendiyse ödeme yöntemi ekleyebilir veya başka bir ücretsiz sağlayıcıya geçebilirsiniz

### Model listesinde Grok modelleri görünmüyor

- `/connect` ile bağlantıyı yenileyip tekrar deneyin
- `opencode models --refresh` komutuyla model önbelleğini temizleyin
- xAI konsolundan API anahtarınızın aktif olduğunu doğrulayın

### Yanıt çok yavaş geliyor

- xAI sunucuları yoğun saatlerde yavaşlayabilir
- Daha hızlı yanıt için Grok 3 Mini modeline geçin
- Birkaç dakika bekleyip tekrar deneyin

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
   Listede `xai/grok-3` veya `xai/grok-3-mini` gibi modeller görünmelidir.

3. Test mesajı gönderin:
   ```
   İki sayıyı toplayan bir Python fonksiyonu yaz ve 3 test örneği ekle
   ```

4. Çalışan bir Python fonksiyonu ve test örnekleri üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir toplama fonksiyonu. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
