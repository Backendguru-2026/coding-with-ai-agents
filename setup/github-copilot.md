# GitHub Copilot Kurulumu

> GitHub Copilot, OpenCode CLI'ye yerleşik olarak entegre edilmiş bir sağlayıcıdır. OAuth cihaz kodu ile bağlanır ve frontier modellere erişim sağlar.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | GitHub Copilot (Microsoft / GitHub) |
| **Bağlantı Türü** | OpenCode yerleşik sağlayıcı |
| **Pro Planı** | $10/ay — GPT-4o, Claude Sonnet erişimi |
| **Pro+ Planı** | $39/ay — tüm modeller (Gemini, o1 dahil) |
| **Kredi Kartı** | Evet, zorunlu |
| **Ücretsiz Deneme** | GitHub Copilot Free ile sınırlı erişim mümkün |
| **Faturalandırma** | Aylık, GitHub hesabı üzerinden |

**Not:** Pro planı çoğu kullanım senaryosu için yeterlidir. Pro+ yalnızca o1 ve bazı Gemini modellerine ihtiyaç duyuyorsanız gereklidir.

---

## 2. Hesap Oluşturma

### Adım 1 — GitHub Hesabı

Zaten bir GitHub hesabınız yoksa:

1. [github.com/signup](https://github.com/signup) adresine gidin
2. E-posta, şifre ve kullanıcı adı bilgilerini girin
3. E-posta doğrulamasını tamamlayın

### Adım 2 — Copilot Aboneliği

1. [github.com/github-copilot/signup](https://github.com/github-copilot/signup) adresine gidin
2. **Pro** ($10/ay) veya **Pro+** ($39/ay) planını seçin
3. Kredi kartı bilgilerinizi girin
4. Aboneliği onaylayın

### İşletim Sistemi Notları

- **Windows / macOS / Linux:** Fark yok. GitHub hesabı tarayıcı üzerinden oluşturulur, OpenCode CLI üzerinden bağlantı tüm platformlarda aynıdır.

---

## 3. Kimlik Doğrulama

GitHub Copilot, **OAuth cihaz kodu** (device code flow) yöntemiyle kimlik doğrulaması yapar. API anahtarı gerekmez.

### Akış

1. OpenCode CLI'de `/connect` komutunu çalıştırırsınız
2. CLI size bir cihaz kodu ve URL verir
3. Tarayıcıda URL'ye gidip kodu girersiniz
4. GitHub hesabınızla yetkilendirirsiniz
5. CLI otomatik olarak bağlantıyı tamamlar

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

3. Sağlayıcı listesinden **GitHub Copilot** seçeneğini seçin:
   ```
   ┌  Add credential
   │
   ◆  Select provider
   │  ● GitHub Copilot
   │  ...
   └
   ```

4. Ekranda bir cihaz kodu görünecektir:
   ```
   ┌ Login with GitHub Copilot
   │
   │ https://github.com/login/device
   │
   │ Enter code: 8F43-6FCF
   │
   └ Waiting for authorization...
   ```

5. Tarayıcınızda [github.com/login/device](https://github.com/login/device) adresine gidin
6. Ekrandaki kodu (örn. `8F43-6FCF`) girin
7. **Authorize** butonuna tıklayın
8. CLI otomatik olarak bağlantıyı algılayacaktır

### Yöntem 2 — CLI ile

```bash
opencode auth login
```

Bu komut aynı cihaz kodu akışını terminal üzerinden başlatır.

---

## 5. Kodlama İçin Önerilen Modeller

Bağlantı tamamlandıktan sonra `/models` komutuyla model seçebilirsiniz.

### GPT-4o (Pro Plan — $10/ay)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `copilot/gpt-4o` |
| **Neden?** | Hızlı yanıt süresi, güçlü kod tamamlama ve düzenleme. Günlük kodlama görevleri için ideal denge: hız + kalite. |
| **En iyi kullanım** | Kod yazma, hata ayıklama, refactoring, birim test yazma |

### Claude Sonnet (Pro Plan — $10/ay)

| Özellik | Detay |
|---------|-------|
| **Model ID** | `copilot/claude-sonnet` |
| **Neden?** | Uzun bağlam desteği ve talimat takibinde üstün. Karmaşık, çok dosyalı düzenleme görevlerinde GPT-4o'dan daha tutarlı sonuçlar üretir. |
| **En iyi kullanım** | Mimari değişiklikler, büyük refactoring, kod inceleme |

> **Not:** Gemini ve o1 gibi bazı modeller yalnızca **Pro+** ($39/ay) aboneliğiyle kullanılabilir. `/models` komutuyla mevcut modellerinizi görebilirsiniz.

---

## 6. Limitler ve İpuçları

### Kullanım Limitleri

| Limit Türü | Pro ($10/ay) | Pro+ ($39/ay) |
|------------|:------------:|:-------------:|
| GPT-4o istekleri | Yüksek | Sınırsıza yakın |
| Claude Sonnet | Mevcut | Mevcut |
| o1, Gemini | Yok | Mevcut |
| Aylık istek tavanı | Var (dinamik) | Var (daha yüksek) |

### İpuçları

- **Model değiştirin:** Basit görevler için daha hafif modelleri, karmaşık görevler için Claude Sonnet veya GPT-4o kullanın
- **Bağlamı küçük tutun:** Gereksiz dosyaları bağlama eklemeyin; bu hem hız kazandırır hem token tüketimini azaltır
- **Limit dolduğunda:** Rate limit hatasıyla karşılaşırsanız birkaç dakika bekleyin, Copilot limitleri kısa aralıklarla sıfırlanır
- **Aboneliği kontrol edin:** [github.com/settings/copilot](https://github.com/settings/copilot) adresinden plan durumunuzu ve kullanım istatistiklerinizi görüntüleyebilirsiniz

---

## 7. Sorun Giderme

### "Waiting for authorization..." mesajı takılı kalıyor

- Tarayıcınızda [github.com/login/device](https://github.com/login/device) adresine gittiğinizden emin olun
- Kodu doğru girdiğinizi kontrol edin (büyük harf ve tire dahil)
- GitHub hesabınızla oturum açtığınızdan emin olun
- Pop-up engelleyici tarayıcı uzantılarını geçici olarak devre dışı bırakın

### "Copilot subscription required" hatası

- [github.com/settings/copilot](https://github.com/settings/copilot) adresinden aboneliğinizin aktif olduğunu doğrulayın
- Ücretsiz deneme süresi dolduysa ödeme yöntemini ekleyin
- Kuruluş (organization) hesabıyla giriş yapıyorsanız, yöneticinizin Copilot erişimi verdiğinden emin olun

### Model listesinde beklenen model görünmüyor

- `/models` komutunu çalıştırarak mevcut modelleri kontrol edin
- Bazı modeller yalnızca Pro+ planında mevcuttur
- `/connect` ile bağlantıyı yenileyip tekrar deneyin

### Bağlantı token süresi dolmuş

```bash
opencode auth login
```

Komutu çalıştırarak yeniden kimlik doğrulaması yapın. Eski token otomatik olarak yenilenecektir.

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
   Listede `copilot/gpt-4o` veya `copilot/claude-sonnet` gibi modeller görünmelidir.

3. Test mesajı gönderin:
   ```
   Fibonacci sayılarını hesaplayan recursive bir Python fonksiyonu yaz
   ```

4. Çalışan bir Python kodu üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir Fibonacci fonksiyonu. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
