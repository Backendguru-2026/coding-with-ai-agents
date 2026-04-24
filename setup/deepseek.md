# DeepSeek Kurulumu

> DeepSeek, kodlama ve mantık yürütme konusunda öne çıkan Çin merkezli bir AI laboratuvarıdır. Ücretsiz API erişimi sunar ve kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Maliyet** | Ücretsiz (5M token hediye) |
| **Kredi Kartı** | Gerekmiyor |
| **Resmi Site** | [deepseek.com](https://deepseek.com) |
| **API Platformu** | [platform.deepseek.com](https://platform.deepseek.com) |
| **Avantaj** | Kodlama benchmark'larında frontier modellere yakın performans, ücretsiz |

DeepSeek, özellikle yazılım geliştirme ve mantık yürütme görevlerinde son derece güçlü modellerle tanınır. Kayıt olduğunuzda hesabınıza 5 milyon token ücretsiz yüklenir (30 gün geçerli). Kodlama benchmark'larında (SWE-bench, HumanEval) tutarlı biçimde üst sıralarda yer alır.

---

## 2. Hesap Oluşturma

1. [platform.deepseek.com](https://platform.deepseek.com) adresine gidin
2. **Sign Up** butonuna tıklayın
3. E-posta adresinizi girin ve doğrulama kodunu alın
4. Şifrenizi belirleyin
5. Hesabınız oluşturulur ve otomatik olarak 5M ücretsiz token yüklenir

> **Not:** Kayıt sırasında telefon numarası veya kredi kartı istenmez.

---

## 3. Kimlik Doğrulama

### API Anahtarı Oluşturma

1. [platform.deepseek.com/api_keys](https://platform.deepseek.com/api_keys) adresine gidin
2. **Create new API key** butonuna tıklayın
3. Anahtarınıza bir isim verin (ör. `opencode-kurs`)
4. Oluşturulan `sk-...` ile başlayan anahtarı kopyalayın

> **Uyarı:** API anahtarı yalnızca oluşturulma anında tam olarak gösterilir. Hemen kopyalayıp güvenli bir yere kaydedin.

---

## 4. OpenCode CLI Bağlantısı

DeepSeek, OpenCode'un yerleşik sağlayıcılarından biridir. Yapılandırma dosyası gerekmez.

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
│  ● DeepSeek
└
```

DeepSeek'i seçin ve API anahtarınızı yapıştırın:

```
┌  Add credential
│
◇  Enter your API key
│  sk-...
└
```

### Yöntem 2 — CLI ile

```bash
opencode auth login
# DeepSeek'i seçin → API anahtarınızı girin
```

### Bağlantıyı Doğrulama

```bash
opencode auth list
```

Çıktıda `deepseek` görünmelidir.

---

## 5. Kodlama İçin Önerilen Modeller

Model seçimi için TUI içinde `/models` komutunu kullanın.

### DeepSeek V3 (Genel Kodlama)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `deepseek-chat` |
| **Bağlam Penceresi** | 64K token |
| **Neden** | Hızlı ve verimli genel amaçlı kodlama modeli. Günlük görevler (kod yazma, düzenleme, hata düzeltme, testler) için en iyi seçimdir. Token tüketimi R1'e göre düşüktür. |

### DeepSeek R1 (Mantık Yürütme)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `deepseek-reasoner` |
| **Bağlam Penceresi** | 64K token |
| **Neden** | "Düşünce zinciri" (chain-of-thought) ile çalışır. Karmaşık algoritmalar, mimari kararlar, çok adımlı hata ayıklama ve zor matematik/mantık problemleri için idealdir. |

> **Tavsiye:** Günlük kodlama görevleri için **DeepSeek V3** kullanın — daha hızlı yanıt verir ve daha az token harcar. Takıldığınız karmaşık problemler için **R1**'e geçin; R1 düşünce sürecini görünür şekilde gösterir, bu da hatayı anlamanıza yardımcı olur.

---

## 6. Limitler ve İpuçları

### Ücretsiz Katman Limitleri

| Limit | Değer |
|-------|-------|
| **Ücretsiz token** | 5.000.000 token |
| **Token geçerlilik süresi** | Kayıttan itibaren 30 gün |
| **Eşzamanlı istek** | Sınırlı (yoğun saatlerde kuyruk oluşabilir) |

### Kullanımı Optimize Etme İpuçları

- **Token bakiyenizi takip edin:** [platform.deepseek.com/usage](https://platform.deepseek.com/usage) adresinden kalan token miktarınızı kontrol edin
- **V3'ü varsayılan yapın:** R1 düşünce zincirleri ekstra token harcar; günlük işler için V3 yeterlidir
- **`/compact` komutunu kullanın:** Uzun sohbetlerde bağlam penceresini sıkıştırarak token tasarrufu yapın
- **Kısa talimatlar verin:** Gereksiz bağlam göndermekten kaçının
- **Yoğun saatlere dikkat:** Asya çalışma saatlerinde (UTC+8 sabah-akşam) sunucular yoğun olabilir; yanıt süreleri uzayabilir

### Token Tükendiğinde

5M token bittikten veya 30 gün dolduktan sonra:
- Hesabınıza bakiye yükleyebilirsiniz (çok düşük fiyatlar: V3 $0.27/M input, R1 $0.55/M input)
- Alternatif olarak OpenRouter veya Cerebras gibi diğer ücretsiz sağlayıcılara geçebilirsiniz

---

## 7. Sorun Giderme

### "Insufficient balance" hatası

Ücretsiz token bakiyeniz tükenmiş veya 30 günlük süre dolmuş.

**Çözüm:**
- [platform.deepseek.com/usage](https://platform.deepseek.com/usage) adresinden bakiyenizi kontrol edin
- Bakiye yükleyin veya alternatif sağlayıcıya geçin:
  ```
  /connect → OpenRouter veya Cerebras
  ```

### "Rate limit" veya yavaş yanıtlar

DeepSeek sunucuları yoğun saatlerde kapasiteye ulaşabilir.

**Çözüm:**
- Birkaç dakika bekleyip tekrar deneyin
- Alternatif olarak aynı modeli OpenRouter üzerinden kullanın (OpenRouter, DeepSeek modellerini birden fazla altyapıdan sunar)

### "Invalid API key" hatası

**Çözüm:**
- [platform.deepseek.com/api_keys](https://platform.deepseek.com/api_keys) adresinden anahtarınızın aktif olduğunu doğrulayın
- Anahtarın `sk-` ile başladığından emin olun
- Yeniden bağlanın:
  ```
  /connect → DeepSeek → yeni API anahtarı yapıştırın
  ```

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
# Çıktıda "deepseek" görünmeli
```

TUI içinde test edin:

```
opencode
# /models → deepseek-chat seçin
# Test mesajı: "Verilen bir listenin tüm permütasyonlarını üreten bir Python fonksiyonu yaz"
```

Çalışan bir Python kodu görüyorsanız DeepSeek kurulumu tamamdır.

---

*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
