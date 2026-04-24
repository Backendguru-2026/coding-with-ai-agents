# OpenCode CLI Kurulumu

> OpenCode, terminal tabanlı açık kaynak bir AI kodlama ajanıdır. 75+ LLM sağlayıcısını destekler.

---

## 1. Genel Bakış

| Özellik | Detay |
|---------|-------|
| **Tür** | Terminal (TUI) tabanlı AI kodlama ajanı |
| **Lisans** | Açık kaynak |
| **Gereksinimler** | Node.js 18+ veya standalone kurulum |
| **Resmi Site** | [opencode.ai](https://opencode.ai) |
| **Dokümantasyon** | [opencode.ai/docs](https://opencode.ai/docs) |

---

## 2. Kurulum

### Windows

**Scoop (Önerilen):**
```bash
scoop install opencode
```

**Chocolatey:**
```bash
choco install opencode
```

**npm:**
```bash
npm install -g opencode-ai@latest
```

**Standalone (PowerShell):**
```powershell
irm https://opencode.ai/install.ps1 | iex
```

### macOS

**Homebrew (Önerilen):**
```bash
brew install anomalyco/tap/opencode
```

**npm:**
```bash
npm install -g opencode-ai@latest
```

**Standalone:**
```bash
curl -fsSL https://opencode.ai/install | bash
```

### Linux

**Homebrew:**
```bash
brew install anomalyco/tap/opencode
```

**npm:**
```bash
npm install -g opencode-ai@latest
```

**Arch Linux:**
```bash
sudo pacman -S opencode        # Stable
paru -S opencode-bin           # AUR (en güncel)
```

**Nix:**
```bash
nix run nixpkgs#opencode
```

**Standalone:**
```bash
curl -fsSL https://opencode.ai/install | bash
```

**Özel dizine kurulum:**
```bash
OPENCODE_INSTALL_DIR=/usr/local/bin curl -fsSL https://opencode.ai/install | bash
```

### Evrensel Paket Yöneticileri

```bash
bun install -g opencode-ai     # Bun
pnpm install -g opencode-ai    # pnpm
yarn global add opencode-ai    # Yarn
mise use -g opencode            # mise
```

---

## 3. İlk Çalıştırma

Kurulum tamamlandıktan sonra terminalde:

```bash
opencode
```

OpenCode TUI (Terminal User Interface) açılacaktır. İlk çalıştırmada henüz bir sağlayıcı bağlı değildir.

---

## 4. Sağlayıcı Bağlama

Bir AI sağlayıcısını bağlamak için TUI içinde:

```
/connect
```

Bu komut mevcut sağlayıcıları listeler. Birini seçin ve kimlik doğrulama adımlarını takip edin.

**Alternatif — CLI üzerinden:**
```bash
opencode auth login
```

Kimlik bilgileri `~/.local/share/opencode/auth.json` dosyasında saklanır.

**Bağlı sağlayıcıları görmek için:**
```bash
opencode auth list
```

---

## 5. Temel Komutlar

| Komut | Açıklama |
|-------|----------|
| `opencode` | TUI modunda aç |
| `opencode run "mesaj"` | Tek seferlik mesaj gönder |
| `opencode run --continue "mesaj"` | Son oturuma devam et |
| `opencode run --agent plan "mesaj"` | Plan modu ile çalıştır |
| `opencode models` | Mevcut modelleri listele |
| `opencode models <sağlayıcı>` | Belirli sağlayıcının modellerini listele |
| `opencode models --refresh` | Model önbelleğini yenile |
| `opencode auth login` | Sağlayıcı kimlik doğrulaması |
| `opencode auth list` | Bağlı sağlayıcıları listele |
| `opencode auth logout` | Oturumu kapat |

---

## 6. TUI İçi Komutlar

TUI açıkken kullanılabilecek komutlar:

| Komut | Açıklama |
|-------|----------|
| `/connect` | Yeni sağlayıcı bağla |
| `/models` | Model seçimi yap |
| `/init` | Proje için `AGENTS.md` oluştur |
| `/compact` | Bağlam penceresini sıkıştır |
| `/clear` | Sohbet geçmişini temizle |

---

## 7. Özel Sağlayıcı Yapılandırma (opencode.json)

Bazı sağlayıcılar (Ollama, LM Studio gibi) `opencode.json` dosyası ile yapılandırılır. Projenizin kök dizininde oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "sağlayıcı-id": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Sağlayıcı Adı",
      "options": {
        "baseURL": "https://api.example.com/v1"
      },
      "models": {
        "model-adı": {
          "name": "Model Görünen Adı"
        }
      }
    }
  }
}
```

---

## 8. Sorun Giderme

### Sağlayıcı paketi hatası (AI_APICallError)

OpenCode, sağlayıcı paketlerini dinamik olarak indirir ve önbelleğe alır. Hata alırsanız önbelleği temizleyin:

**Windows:**
`WIN+R` → `%USERPROFILE%\.cache\opencode` klasörünü silin.

**macOS/Linux:**
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın — paketler otomatik indirilecektir.

### OpenCode güncellenemiyorsa

```bash
npm update -g opencode-ai@latest
```

veya Homebrew ile:

```bash
brew upgrade opencode
```

---

## 9. Doğrulama

Kurulumun başarılı olduğunu doğrulamak için:

```bash
opencode --version
```

Ardından TUI'yi açıp bir sağlayıcı bağlayın:

```bash
opencode
# TUI içinde: /connect → Sağlayıcı seçin → Kimlik doğrulayın
# TUI içinde: /models → Model seçin
# Test: "Merhaba, 1'den 10'a kadar sayıları yazdıran bir Python scripti yaz"
```

Çıktı olarak çalışan bir Python kodu görüyorsanız kurulum tamamdır.

---

*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
