# OpenCode Sağlayıcı Kurulum Rehberi

> Bu rehber, OpenCode CLI ile kullanabileceğiniz tüm AI sağlayıcılarının kurulum ve yapılandırma dokümanlarını içerir.
> Önce [OpenCode CLI Kurulumu](opencode-cli.md) dokümanını takip edin.

---

## Sağlayıcı Karşılaştırma Tablosu

### Tier 1 — Frontier Modeller (Abonelik)

| Sağlayıcı | Maliyet | Kredi Kartı | En İyi Kodlama Modeli | Bağlantı Yöntemi | Detay |
|-----------|---------|-------------|----------------------|------------------|-------|
| **GitHub Copilot** | $10-39/ay | Evet | GPT-4o, Claude Sonnet, Gemini | OAuth (cihaz kodu) | [Kurulum](github-copilot.md) |
| **ChatGPT Plus/Pro** | $20/ay | Evet | GPT-4o, o1 | OAuth (tarayıcı) | [Kurulum](chatgpt-plus.md) |
| **OpenCode Go** | $5 ilk ay, $10/ay | Evet | Qwen3.5+, Kimi K2.5+, GLM-5+ | Yerleşik | [Kurulum](opencode-go.md) |

### Tier 2 — Güçlü Ücretsiz Sağlayıcılar

| Sağlayıcı | Maliyet | Kredi Kartı | En İyi Kodlama Modeli | Ücretsiz Limit | Detay |
|-----------|---------|-------------|----------------------|----------------|-------|
| **OpenRouter** | $0 | Hayır | qwen3-coder, DeepSeek R1 | ~200 istek/gün, 20 RPM | [Kurulum](openrouter.md) |
| **DeepSeek** | $0 | Hayır | DeepSeek V3, R1 | 5M token (30 gün) | [Kurulum](deepseek.md) |
| **Cerebras** | $0 | Hayır | Qwen3 235B | 1M token/gün, 30 RPM | [Kurulum](cerebras.md) |

### Tier 3 — Stabil Ücretsiz Sağlayıcılar

| Sağlayıcı | Maliyet | Kredi Kartı | En İyi Kodlama Modeli | Ücretsiz Limit | Detay |
|-----------|---------|-------------|----------------------|----------------|-------|
| **Google Gemini** | $0 | Hayır | Gemini 2.5 Flash, 2.5 Pro | 1K istek/gün, 15 RPM | [Kurulum](google-gemini.md) |
| **Groq** | $0 | Hayır | Llama 3.3 70B, Qwen QwQ 32B | 500K token/gün, 30 RPM | [Kurulum](groq.md) |
| **Mistral** | $0 | Hayır | Codestral | 1B token/ay, 2 RPM | [Kurulum](mistral.md) |

### Tier 4 — Keşfedilecek Sağlayıcılar

| Sağlayıcı | Maliyet | Kredi Kartı | En İyi Kodlama Modeli | Ücretsiz Limit | Detay |
|-----------|---------|-------------|----------------------|----------------|-------|
| **xAI (Grok)** | $0 | Hayır | Grok modelleri | $25 kredi | [Kurulum](xai.md) |
| **NVIDIA NIM** | $0 | Hayır | NVIDIA barındırmalı modeller | ~40 RPM | [Kurulum](nvidia-nim.md) |
| **Hugging Face** | $0 | Hayır | Açık kaynak modeller | Ücretsiz CPU tier | [Kurulum](hugging-face.md) |

### Yerel Modeller

| Sağlayıcı | Maliyet | Gereksinim | Açıklama | Detay |
|-----------|---------|-----------|----------|-------|
| **Ollama** | $0 | 8+ GB RAM | CLI tabanlı yerel model çalıştırıcı | [Kurulum](ollama.md) |
| **LM Studio** | $0 | 8+ GB RAM | GUI tabanlı yerel model çalıştırıcı | [Kurulum](lm-studio.md) |

### İleri Seviye

| Konu | Açıklama | Detay |
|------|----------|-------|
| **Dinamik Yönlendirme** | OpenRouter fallback ve çoklu sağlayıcı yönlendirme | [Rehber](dynamic-routing.md) |

---

## Hızlı Başlangıç Önerileri

### Sıfır Bütçe — En Hızlı Başlangıç
1. [OpenCode CLI Kurulumu](opencode-cli.md)
2. [Groq Kurulumu](groq.md) — hızlı, yüksek limit, kayıt 2 dakika
3. Hazırsınız!

### Sıfır Bütçe — En İyi Kodlama Kalitesi
1. [OpenCode CLI Kurulumu](opencode-cli.md)
2. [OpenRouter Kurulumu](openrouter.md) — qwen3-coder veya DeepSeek R1
3. [Groq Kurulumu](groq.md) — hız gerektiğinde yedek olarak
4. İsteğe bağlı: [Cerebras Kurulumu](cerebras.md) — 1M token/gün

### Yerel + Bulut Hibrit
1. [OpenCode CLI Kurulumu](opencode-cli.md)
2. [Ollama Kurulumu](ollama.md) — gizlilik ve çevrimdışı çalışma
3. [Groq Kurulumu](groq.md) — büyük modeller için bulut yedek

### Abonelik — Frontier Modeller
1. [OpenCode CLI Kurulumu](opencode-cli.md)
2. [GitHub Copilot Kurulumu](github-copilot.md) — $10/ay'a GPT-4o, Claude, Gemini
3. İsteğe bağlı: [OpenCode Go Kurulumu](opencode-go.md) — $10/ay'a açık kaynak modeller

---

## Önemli Notlar

- **Anthropic Claude Pro/Max:** OpenCode v1.3.0 itibarıyla Anthropic, Claude Pro/Max aboneliklerinin üçüncü parti araçlarda kullanımını yasaklamıştır. Claude modelleri yalnızca doğrudan API anahtarı (ücretli, token başına) ile kullanılabilir.
- **Birden fazla sağlayıcı:** Aynı anda birden fazla sağlayıcı bağlayabilirsiniz. `/models` komutu ile aktif sağlayıcı ve model seçimi yapabilirsiniz.
- **Tüm sağlayıcılar:** OpenCode 75+ sağlayıcıyı destekler. Bu rehber kurs için önerilen sağlayıcıları kapsar. Tam liste: [opencode.ai/docs/providers](https://opencode.ai/docs/providers)

---

*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
