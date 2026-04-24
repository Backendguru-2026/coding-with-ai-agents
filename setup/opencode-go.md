# OpenCode Go Kurulumu

> OpenCode Go, OpenCode ekibinin sunduğu düşük maliyetli yerleşik abonelik hizmetidir. Popüler açık kaynak kodlama modellerine güvenilir erişim sağlar.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | OpenCode Go (OpenCode ekibi) |
| **Bağlantı Türü** | OpenCode yerleşik abonelik |
| **Fiyat** | $10/ay (ilk ay $5 — beta indirimi) |
| **Durum** | Beta |
| **Kredi Kartı** | Evet, zorunlu |
| **Ücretsiz Deneme** | Yok (ancak ilk ay %50 indirimli) |
| **Faturalandırma** | Aylık, OpenCode hesabı üzerinden |

### Kayan Kullanım Limitleri

| Limit Türü | Miktar |
|------------|--------|
| **5 saatlik limit** | $12 değerinde kullanım |
| **Haftalık limit** | $30 değerinde kullanım |
| **Aylık limit** | $60 değerinde kullanım |

> **Not:** Limit miktarları dolar bazında tanımlanmıştır. Ucuz modeller daha fazla istek yapmanıza olanak tanırken, pahalı modeller limiti daha hızlı tüketir.

---

## 2. Hesap Oluşturma

### Adım 1 — OpenCode Hesabı

1. [opencode.ai](https://opencode.ai) adresine gidin
2. **Sign Up** veya **Get Started** butonuna tıklayın
3. E-posta adresi veya GitHub hesabıyla kayıt olun
4. E-posta doğrulamasını tamamlayın

### Adım 2 — Go Aboneliği

1. [opencode.ai](https://opencode.ai) adresinde oturum açın
2. Hesap ayarlarından **OpenCode Go** aboneliğine gidin
3. **Subscribe — $5/first month** butonuna tıklayın
4. Kredi kartı bilgilerinizi girin
5. Aboneliği onaylayın

### İşletim Sistemi Notları

- **Windows / macOS / Linux:** Fark yok. Hesap tarayıcı üzerinden oluşturulur, OpenCode CLI üzerinden bağlantı tüm platformlarda aynıdır.

---

## 3. Kimlik Doğrulama

OpenCode Go, OpenCode hesabınız üzerinden kimlik doğrulaması yapar. Ayrı bir API anahtarı oluşturmanız gerekmez.

### Akış

1. OpenCode CLI'de `/connect` komutunu çalıştırırsınız
2. Sağlayıcı olarak OpenCode Go'yu seçersiniz
3. Tarayıcınız açılarak OpenCode hesabınızla giriş yapmanızı ister
4. Yetkilendirme tamamlandığında CLI bağlantıyı otomatik olarak kurar

Kimlik bilgileri `~/.local/share/opencode/auth.json` dosyasında saklanır.

---

## 4. OpenCode CLI Bağlantısı

### Yöntem 1 — TUI ile (Önerilen)

1. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

2. `/connect` komutunu yazın:
   ```
   /connect
   ```

3. Sağlayıcı listesinden **OpenCode Go** seçeneğini seçin:
   ```
   ┌  Add credential
   │
   ◆  Select provider
   │  ● OpenCode Go
   │  ...
   └
   ```

4. Tarayıcınız açılacak, OpenCode hesabınızla oturum açın
5. **Authorize** butonuna tıklayın
6. CLI bağlantıyı otomatik olarak tamamlayacaktır

### Yöntem 2 — CLI ile

```bash
opencode auth login
```

### Zen API Yapılandırması (İleri Düzey)

OpenCode Go modelleri, OpenCode'un Zen API altyapısı üzerinden sunulur. Özel yapılandırma yapmak isterseniz `opencode.json` dosyasını kullanabilirsiniz:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "opencode": {
      "models": {
        "qwen3.5-plus": {
          "options": {}
        }
      }
    }
  }
}
```

> **Not:** Çoğu kullanıcı için `opencode.json` yapılandırması gerekmez. `/connect` ile bağlanmak yeterlidir.

---

## 5. Kodlama İçin Önerilen Modeller

OpenCode Go, test edilmiş ve doğrulanmış açık kaynak kodlama modellerine erişim sağlar. Bağlantı tamamlandıktan sonra `/models` komutuyla model seçebilirsiniz.

### Qwen 3.5 Plus

| Özellik | Detay |
|---------|-------|
| **Model ID** | `qwen3.5-plus` |
| **Sağlayıcı** | Alibaba (AI SDK: `@ai-sdk/alibaba`) |
| **Neden?** | Maliyet-performans oranı en yüksek model. Hızlı yanıt verir, Türkçe dahil çok dilli desteği güçlüdür. Günlük kodlama görevlerinde frontier modellerle rekabet eder. |
| **En iyi kullanım** | Günlük kodlama, hata ayıklama, test yazma, kod açıklama |

### Kimi K2.5+

| Özellik | Detay |
|---------|-------|
| **Model ID** | `kimi-k2.5` / `kimi-k2.6` |
| **Sağlayıcı** | Moonshot AI |
| **Neden?** | Uzun bağlam desteği ve karmaşık akıl yürütme yeteneği güçlüdür. Çok dosyalı analiz ve mimari düzenleme görevlerinde tutarlı sonuçlar üretir. |
| **En iyi kullanım** | Büyük kod tabanı analizi, mimari değişiklikler, karmaşık hata ayıklama |

### Diğer Mevcut Modeller

| Model | Açıklama |
|-------|----------|
| **GLM-5** | Zhipu'nun genel amaçlı kodlama modeli |
| **GLM-5.1** | GLM-5'in geliştirilmiş sürümü, daha iyi kod anlama |
| **MiMo-V2-Pro** | Kod üretme ve düzenleme odaklı, kompakt model |
| **MiniMax M2.5** | Hızlı yanıt süresi, basit görevler için ideal |
| **Qwen 3.6 Plus** | Qwen 3.5 Plus'ın güncel sürümü |

> **Kurs önerisi:** Başlangıç için **Qwen 3.5 Plus** kullanın. İhtiyaca göre Kimi K2.5+ veya GLM-5.1'e geçiş yapabilirsiniz.

---

## 6. Limitler ve İpuçları

### Kayan Kullanım Limitleri Detayı

OpenCode Go'nun limitleri **dolar değeri** üzerinden hesaplanır ve birbirinden bağımsız olarak kayan pencerelerle çalışır:

| Pencere | Limit | Açıklama |
|---------|-------|----------|
| **5 saat** | $12 | Son 5 saat içindeki toplam kullanım |
| **1 hafta** | $30 | Son 7 gün içindeki toplam kullanım |
| **1 ay** | $60 | Son 30 gün içindeki toplam kullanım |

**Kayan pencere şu anlama gelir:** Limitler sabit tarihte sıfırlanmaz. Sürekli olarak geriye doğru hesaplanır. Örneğin 5 saatlik limit, şu anki zamandan 5 saat geriye bakarak hesaplanır.

### Model Bazında İstek Kapasitesi

Ucuz modeller daha fazla istek yapmanıza olanak tanır:

| Senaryo | Tahmini Kapasite |
|---------|-----------------|
| Qwen 3.5 Plus ile hafif kullanım | Günlük yüzlerce istek |
| Kimi K2.5+ ile yoğun kullanım | Günlük onlarca istek |
| Karışık model kullanımı | Çoğu geliştirici için yeterli |

### İpuçları

- **Ucuz modeli varsayılan yapın:** Günlük görevler için Qwen 3.5 Plus veya MiniMax M2.5 kullanarak limiti yavaş tüketin
- **Pahalı modelleri stratejik kullanın:** Kimi K2.5+ ve GLM-5.1'i yalnızca karmaşık görevlerde tercih edin
- **Limitleri izleyin:** Limit dolduğunda OpenCode CLI sizi bilgilendirecektir
- **Yedek sağlayıcı ekleyin:** Go limiti dolarsa alternatif bir ücretsiz sağlayıcı (OpenRouter, Cerebras) ekleyerek kesintisiz çalışmaya devam edin
- **Beta avantajı:** İlk ay $5 ile tam erişim; limitlerin sizin kullanım alışkanlıklarınıza yetip yetmediğini test edin

---

## 7. Sorun Giderme

### "Subscription required" hatası

- [opencode.ai](https://opencode.ai) adresinde oturum açıp Go aboneliğinizin aktif olduğunu doğrulayın
- Ödeme yönteminizin geçerli olduğundan ve kartın süresinin dolmadığından emin olun
- Yeni abone olduysanız birkaç dakika bekleyip CLI'yi yeniden başlatın

### "Rate limit exceeded" veya "Usage limit reached" hatası

- Kayan limitlerden biri dolmuş olabilir
- Hangi limitin dolduğunu kontrol edin:
  - **5 saatlik limit:** Birkaç saat bekleyin, en eski kullanımlar pencereden çıkınca limit açılır
  - **Haftalık limit:** Daha eski kullanımlar haftalık pencereden çıkana kadar bekleyin
  - **Aylık limit:** Aylık pencere dolmuşsa alternatif sağlayıcı kullanın
- Beklerken ücretsiz bir sağlayıcıya (OpenRouter, Cerebras) geçiş yapın

### Model yanıt vermiyor veya zaman aşımı

- İnternet bağlantınızı kontrol edin
- Farklı bir model deneyin (`/models` ile)
- OpenCode Go beta aşamasındadır; geçici kesintiler yaşanabilir
- [opencode.ai](https://opencode.ai) adresinden servis durumunu kontrol edin

### Bağlantı/token süresi dolmuş

```bash
opencode auth login
```

Komutu çalıştırarak yeniden kimlik doğrulaması yapın.

### Model listesi boş görünüyor

- `/connect` ile bağlantıyı yenileyip tekrar deneyin
- OpenCode CLI'nin güncel sürümde olduğundan emin olun:
  ```bash
  npm update -g opencode-ai@latest
  ```

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
   Listede `qwen3.5-plus`, `kimi-k2.5`, `glm-5` gibi OpenCode Go modelleri görünmelidir.

3. Test mesajı gönderin:
   ```
   Bir Python sınıfı yaz: Stack veri yapısını implement eden, push, pop ve peek metodları olan
   ```

4. Çalışan bir Python Stack sınıfı üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir Stack implementasyonu. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
