# ChatGPT Plus/Pro Kurulumu

> ChatGPT Plus ve Pro abonelikleri, OpenCode CLI'ye yerleşik olarak entegre edilmiş OpenAI sağlayıcısıdır. Tarayıcı tabanlı OAuth ile bağlanır.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | OpenAI (ChatGPT Plus / Pro) |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı |
| **Plus Planı** | $20/ay — GPT-4o erişimi |
| **Pro Planı** | $200/ay — GPT-4o, o1, o1-pro erişimi |
| **Kredi Kartı** | Evet, zorunlu |
| **Ücretsiz Deneme** | Yok (ücretsiz ChatGPT hesabı OpenCode ile çalışmaz) |
| **Faturalandırma** | Aylık, OpenAI hesabı üzerinden |

**Not:** Kurs için **ChatGPT Plus** ($20/ay) yeterlidir. Pro planı yalnızca o1-pro modeline ihtiyaç duyan ileri düzey kullanıcılar içindir.

---

## 2. Hesap Oluşturma

### Adım 1 — OpenAI Hesabı

1. [chat.openai.com/auth/login](https://chat.openai.com/auth/login) adresine gidin
2. **Sign up** butonuna tıklayın
3. E-posta adresi veya Google / Microsoft / Apple hesabıyla kayıt olun
4. E-posta doğrulamasını tamamlayın

### Adım 2 — Plus Aboneliği

1. [chat.openai.com](https://chat.openai.com) adresinde oturum açın
2. Sol alt köşedeki profil simgesine tıklayın
3. **My Plan** veya **Upgrade to Plus** seçeneğini seçin
4. **ChatGPT Plus — $20/month** planını seçin
5. Kredi kartı bilgilerinizi girin ve ödemeyi onaylayın

### İşletim Sistemi Notları

- **Windows:** Tarayıcı tabanlı OAuth; varsayılan tarayıcınız otomatik açılır. Edge veya Chrome önerilir.
- **macOS / Linux:** Aynı akış. Tarayıcı otomatik açılmazsa terminaldeki URL'yi manuel olarak kopyalayın.
- **WSL kullanıcıları:** WSL içinden tarayıcı açılamayabilir. Terminalde görünen URL'yi Windows tarayıcısına yapıştırın.

---

## 3. Kimlik Doğrulama

ChatGPT Plus/Pro, **tarayıcı tabanlı OAuth** yöntemiyle kimlik doğrulaması yapar. İki seçenek sunulur:

### Seçenek A — ChatGPT Plus/Pro OAuth (Önerilen)

Bu yöntem mevcut ChatGPT aboneliğinizi kullanır. API anahtarı gerekmez, ek ücret yansımaz.

### Seçenek B — Manuel API Anahtarı

OpenAI Platform üzerinden bir API anahtarı oluşturup girebilirsiniz. Ancak bu yöntem **kullandıkça öde** faturalandırma kullanır ve ChatGPT Plus aboneliğinden bağımsızdır.

```
┌ Select auth method
│
│ ● ChatGPT Plus/Pro    ← Önerilen
│   Manually enter API Key
└
```

> **Önemli:** "ChatGPT Plus/Pro" seçeneği abonelik kotanızı kullanır. "Manually enter API Key" seçeneği ayrı bir API bütçesi gerektirir. Kurs için ChatGPT Plus/Pro seçeneğini kullanın.

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

3. Sağlayıcı listesinden **OpenAI** seçeneğini seçin:
   ```
   ┌  Add credential
   │
   ◆  Select provider
   │  ● OpenAI
   │  ...
   └
   ```

4. Kimlik doğrulama yöntemi olarak **ChatGPT Plus/Pro** seçin:
   ```
   ┌ Select auth method
   │
   │ ● ChatGPT Plus/Pro
   │   Manually enter API Key
   └
   ```

5. Tarayıcınız otomatik olarak açılacaktır
6. OpenAI hesabınızla oturum açın
7. **Authorize** butonuna tıklayın
8. Tarayıcı "Success" sayfası gösterecek, CLI bağlantıyı tamamlayacaktır

### Yöntem 2 — CLI ile

```bash
opencode auth login
```

### Manuel API Anahtarı Yöntemi (Opsiyonel)

Eğer manuel API anahtarı tercih ederseniz:

1. [platform.openai.com/api-keys](https://platform.openai.com/api-keys) adresine gidin
2. **Create new secret key** butonuna tıklayın
3. Anahtarı kopyalayın
4. `/connect` → OpenAI → **Manually enter API Key** → anahtarı yapıştırın

```
┌ API key
│
│ sk-...
│
└ enter
```

Kimlik bilgileri `~/.local/share/opencode/auth.json` dosyasında saklanır.

---

## 5. Kodlama İçin Önerilen Modeller

Bağlantı tamamlandıktan sonra `/models` komutuyla model seçebilirsiniz.

### GPT-4o (Plus Plan — $20/ay)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `openai/gpt-4o` |
| **Neden?** | Hız ve kalite arasında en iyi denge. Kod yazma, düzenleme ve hata ayıklama görevlerinde tutarlı. Çok modlu (metin + görsel) desteğiyle ekran görüntülerinden kod üretebilir. |
| **En iyi kullanım** | Günlük kodlama, hata ayıklama, kod açıklama, test yazma |

### o1 (Pro Plan — $200/ay)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `openai/o1` |
| **Neden?** | Karmaşık akıl yürütme gerektiren görevlerde üstün performans. Çok adımlı algoritmalar, mimari kararlar ve zor hata ayıklama senaryolarında GPT-4o'dan daha derinlemesine analiz yapar. |
| **En iyi kullanım** | Karmaşık algoritmalar, sistem tasarımı, güvenlik analizi |

> **Kurs önerisi:** ChatGPT Plus ($20/ay) ile gelen **GPT-4o** modeli tüm kurs içeriği için yeterlidir. o1 modeli isteğe bağlıdır.

---

## 6. Limitler ve İpuçları

### Kullanım Limitleri

| Limit Türü | Plus ($20/ay) | Pro ($200/ay) |
|------------|:-------------:|:-------------:|
| GPT-4o | ~80 mesaj/3 saat | Sınırsıza yakın |
| o1 | Yok | ~100 mesaj/hafta |
| o1-pro | Yok | Mevcut (sınırlı) |

> **Not:** Limitler OpenAI tarafından dinamik olarak ayarlanır ve zaman zaman değişebilir.

### İpuçları

- **Mesaj limitini verimli kullanın:** Tek bir istekte birden fazla dosya düzenlemesi yaptırarak mesaj sayısını azaltın
- **Bağlamı daraltın:** Yalnızca ilgili dosyaları bağlama ekleyin; gereksiz dosyalar hem token harcar hem yanıt kalitesini düşürür
- **Limit dolduysa bekleyin:** Plus planında GPT-4o limiti yaklaşık 3 saatte sıfırlanır
- **Aboneliği kontrol edin:** [chat.openai.com](https://chat.openai.com) → profil → **My Plan** üzerinden abonelik durumunuzu görüntüleyebilirsiniz
- **API anahtarı bütçesi:** Manuel API anahtarı kullanıyorsanız [platform.openai.com/usage](https://platform.openai.com/usage) adresinden harcamalarınızı takip edin

---

## 7. Sorun Giderme

### Tarayıcı açılmıyor (OAuth akışı başlamıyor)

- Varsayılan tarayıcınızın düzgün yapılandırıldığını kontrol edin
- **Windows:** `Ayarlar → Varsayılan uygulamalar → Web tarayıcısı` ayarını kontrol edin
- **WSL:** Tarayıcı WSL içinden açılamaz. Terminaldeki URL'yi Windows tarayıcısına yapıştırın
- **SSH oturumu:** Uzak sunucuda çalışıyorsanız URL'yi yerel tarayıcınıza kopyalayın

### "ChatGPT Plus subscription required" hatası

- [chat.openai.com](https://chat.openai.com) adresinde oturum açıp aboneliğinizin aktif olduğunu doğrulayın
- Ödeme yönteminizin geçerli olduğundan emin olun
- Abonelik yeni aldıysanız birkaç dakika bekleyip tekrar deneyin

### "Rate limit exceeded" hatası

- Plus planının mesaj limiti dolmuş olabilir (~80 mesaj/3 saat)
- 3 saat bekleyin veya limitlerin sıfırlanmasını beklerken farklı bir sağlayıcıya geçin
- `/connect` ile alternatif bir sağlayıcı ekleyerek yedek olarak kullanın

### OAuth token süresi dolmuş

```bash
opencode auth login
```

Komutu çalıştırarak yeniden kimlik doğrulaması yapın.

### "Manually enter API Key" seçtim ama ChatGPT Plus kullanmak istiyorum

1. `/connect` komutunu tekrar çalıştırın
2. OpenAI sağlayıcısını seçin
3. Bu sefer **ChatGPT Plus/Pro** seçeneğini seçin

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
   Listede `openai/gpt-4o` gibi modeller görünmelidir.

3. Test mesajı gönderin:
   ```
   Basit bir REST API endpoint'i yaz: GET /hello → {"message": "Merhaba Dünya"}
   ```

4. Çalışan bir API kodu üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir HTTP endpoint kodu. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
