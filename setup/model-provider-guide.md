# AI Kodlama Ajanları ile Yazılım Geliştirme — Model & Provider Rehberi

> Bu rehber, kurs boyunca kullanabileceğiniz LLM model ve sağlayıcı seçeneklerini içerir.
> Kurs politikası: Tüm katılımcılar ücretsiz seçeneklerle kursu tamamlayabilir.

---

## 1. Uzak (Remote) Sağlayıcılar

### Ücretsiz Seçenekler

| Sağlayıcı | Tür | Modeller | Günlük Limit | Aylık Maliyet | En İyi Kullanım |
|-----------|-----|---------|-------------|---------------|----------------|
| **Groq** | Remote | Llama 3.3 70B, Qwen3 32B, Llama 4 Scout | 14.400 istek/gün, 30 RPM | $0 | Hızlı iterasyon, yüksek hacim |
| **OpenRouter** | Remote | qwen3-coder:480b, deepseek-r1 | ~200 istek/gün | $0 | En güçlü ücretsiz kodlama modeli |
| **Google Gemini** | Remote | Gemini 3 Flash, 2.5 Flash | ~250 istek/gün | $0 | Google ekosistemi |

### Ücretli Seçenekler (Aylık $20'a kadar)

| Sağlayıcı | Tür | Modeller | Fiyatlandırma | Aylık Tahmini Maliyet | En İyi Kullanım |
|-----------|-----|---------|--------------|----------------------|----------------|
| **OpenCode Go** | Remote | Seçilmiş kodlama modelleri | Sabit ücret | $5-10/ay | En kolay kurulum |
| **OpenCode Zen** | Remote | Claude, GPT-5, Gemini Pro | Kullandıkça öde (limit ayarlanabilir) | $20/ay'a kadar | En iyi modeller |
| **Claude API** | Remote | Haiku 4.5 ($1/$5 MTok) | Token başına | ~$5-20/ay | Güçlü kodlama, prompt caching |
| **OpenRouter** | Remote | DeepSeek Chat ($0.32/$0.89 MTok) | Token başına | ~$2-5/ay | En ucuz frontier model |

---

## 2. Yerel (Local) Modeller — Ollama ile

### Donanım Gereksinimlerine Göre Model Seçimi

#### 8 GB RAM (Minimum)

| Model | Parametre (Aktif) | RAM/VRAM | Kodlama Gücü | Notlar |
|-------|-------------------|----------|-------------|--------|
| **qwen3:4b** | 4B | ~4 GB | İyi | Kanıtlanmış, küçük, stabil |
| **nemotron-3-nano-4b** | 4B | ~5 GB | İyi | NVIDIA, 1M bağlam, matematik/kod odaklı |
| **gemma4:e4b** | 4B | 6-8 GB | İyi | Google, Nisan 2026, çoklu ortam (multimodal) |

#### 16 GB RAM

| Model | Parametre (Aktif) | RAM/VRAM | Kodlama Gücü | Notlar |
|-------|-------------------|----------|-------------|--------|
| **gemma4:e12b** | 12B | 12-16 GB | Güçlü | En iyi fiyat/performans |
| **nemotron-3-super** | 12B aktif / 120B toplam | 12-16 GB | Güçlü | MoE, NVIDIA |

#### 24+ GB VRAM (GPU)

| Model | Parametre (Aktif) | RAM/VRAM | Kodlama Gücü | Notlar |
|-------|-------------------|----------|-------------|--------|
| **qwen3-coder:30b-a3b** | 30B (3.3B aktif) | ~17 GB | Çok Güçlü | MoE, kodlama optimizeli |
| **gemma4:e27b** | 27B | 24+ GB | Çok Güçlü | %80 LiveCodeBench v6 |
| **nemotron-3-nano:30b** | 30B (6B aktif) | 24 GB | Çok Güçlü | MoE, 1M bağlam |

---

## 3. Kodlama Benchmark Karşılaştırması (Nisan 2026)

### Frontier Modeller (Ücretli)

| Model | SWE-bench | LiveCodeBench v6 | Lisans |
|-------|-----------|-------------------|--------|
| Claude Mythos Preview | — | — | Proprietary (Ağırlıklı: %100) |
| Gemini 3.1 Pro | %80.6 | — | Proprietary |
| Claude Opus 4.6 | %80.8 | — | Proprietary |

### Açık Kaynak / Ücretsiz Modeller

| Model | SWE-bench | LiveCodeBench v6 | HumanEval | Yerel Çalışır mı? |
|-------|-----------|-------------------|-----------|-------------------|
| Kimi K2.5 | — | %85 | %99 | Hayır (MIT, çok büyük) |
| Qwen3.5-plus | %76-78 | %83.6 | — | Hayır (bulut) |
| DeepSeek V3.2 | Güçlü | Güçlü | — | Hayır (671B) |
| GLM-5 | %77.8 | — | — | Hayır (büyük) |
| Gemma4 E27B | — | %80 | — | Evet (24GB GPU) |
| qwen3-coder:480b | Güçlü | — | — | OpenRouter ücretsiz |
| qwen3-coder:30b-a3b | İyi | — | — | Evet (17GB) |
| Gemma4 E12B | — | ~%65 | — | Evet (12-16GB) |

---

## 4. OpenCode CLI Sağlayıcı Kurulumu

### Groq (Önerilen Ücretsiz Bulut)
1. [console.groq.com](https://console.groq.com) adresinden hesap oluşturun (kredi kartı gerekmez)
2. API anahtarı oluşturun
3. OpenCode'da: `/connect` → Groq → API anahtarını yapıştırın
4. `/models groq` ile mevcut modelleri görün

### OpenRouter (Ücretsiz Kodlama Modelleri)
1. [openrouter.ai](https://openrouter.ai) adresinden hesap oluşturun
2. API anahtarı oluşturun
3. OpenCode'da: `/connect` → OpenRouter → API anahtarını yapıştırın
4. `/models openrouter` ile modelleri listeleyin

### Google Gemini (AI Studio Ücretsiz)
1. [aistudio.google.com](https://aistudio.google.com) adresinden API anahtarı alın
2. OpenCode'da: `/connect` → Google → API anahtarını yapıştırın

### Ollama (Yerel)
1. [ollama.com](https://ollama.com) adresinden Ollama'yı indirip kurun
2. Model çekin: `ollama pull qwen3:4b`
3. `opencode.json` dosyasına ekleyin:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen3:4b": {
          "name": "Qwen 3 4B (Local)"
        }
      }
    }
  }
}
```

---

## 5. Önerilen Minimum Kurulum

| Senaryo | Yerel | Bulut | Toplam Maliyet |
|---------|-------|-------|---------------|
| **Ücretsiz Minimum** | Ollama + qwen3:4b | Groq (ücretsiz) | $0 |
| **Ücretsiz Optimal** | Ollama + qwen3:4b | Groq + OpenRouter | $0 |
| **Ücretli Optimal** | Ollama + gemma4:e12b | OpenCode Zen ($20 limit) | ~$20/ay |

> **Not:** Tüm lab ve ödevler ücretsiz seçeneklerle tamamlanabilir.
> Ücretli seçenekler daha iyi kodlama deneyimi sunar ancak zorunlu değildir.

---

*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
