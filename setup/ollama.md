# Ollama Kurulumu

> Ollama, yerel bilgisayarınızda AI modelleri çalıştırmanızı sağlayan ücretsiz ve açık kaynak bir araçtır. İnternet bağlantısı gerektirmez (model indirildikten sonra), verileriniz bilgisayarınızdan çıkmaz. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | Ollama (yerel) |
| **Bağlantı Türü** | opencode.json yapılandırması (`@ai-sdk/openai-compatible`) |
| **Maliyet** | $0 — tamamen ücretsiz, açık kaynak |
| **Kredi Kartı** | Hayır, gerekmiyor |
| **İnternet** | Yalnızca model indirme için gerekli; sonrasında çevrimdışı çalışır |
| **Gereksinim** | Minimum 8 GB RAM (model boyutuna göre daha fazla önerilir) |
| **Resmi Site** | [ollama.com](https://ollama.com) |

**Not:** Ollama, gizlilik odaklı çalışmak isteyen veya internet erişimi kısıtlı ortamlarda bulunan geliştiriciler için idealdir. Tüm işlemler yerel donanımınızda gerçekleşir.

---

## 2. Hesap Oluşturma

Ollama için hesap oluşturmaya **gerek yoktur**. Yalnızca yazılımı indirip kurmanız yeterlidir.

### Windows

1. [ollama.com/download](https://ollama.com/download) adresine gidin
2. **Windows** seçeneğini tıklayın ve kurulum dosyasını indirin
3. İndirilen `.exe` dosyasını çalıştırın ve kurulum sihirbazını takip edin
4. Kurulum tamamlandığında Ollama arka planda çalışmaya başlar

### macOS

1. [ollama.com/download](https://ollama.com/download) adresine gidin
2. **macOS** seçeneğini tıklayın ve `.dmg` dosyasını indirin
3. `.dmg` dosyasını açıp Ollama'yı Applications klasörüne sürükleyin
4. Uygulamayı başlatın

**Alternatif (Homebrew):**
```bash
brew install ollama
```

### Linux

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

### Kurulumu Doğrulama

Terminalde:
```bash
ollama --version
```

Sürüm numarası görünüyorsa kurulum başarılıdır.

---

## 3. Kimlik Doğrulama

Ollama yerel çalıştığı için **kimlik doğrulama gerekmez**. API anahtarı, hesap veya token yoktur. Ollama çalışır durumda olduğu sürece OpenCode doğrudan bağlanabilir.

---

## 4. OpenCode CLI Bağlantısı

Ollama, `opencode.json` dosyası ile yapılandırılır. İki adımda tamamlanır: model indirme ve OpenCode yapılandırması.

### Adım 1 — Model İndirme

Kullanmak istediğiniz modeli indirin. İndirme sırasında internet bağlantısı gereklidir, sonrasında çevrimdışı kullanılabilir.

```bash
ollama pull qwen3:4b
```

İndirme tamamlandığında modeli doğrulayın:

```bash
ollama list
```

Çıktıda indirdiğiniz model görünmelidir:
```
NAME            ID              SIZE      MODIFIED
qwen3:4b        ...             2.6 GB    Just now
```

### Adım 2 — opencode.json Yapılandırması

Projenizin kök dizininde `opencode.json` dosyası oluşturun:

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
          "name": "Qwen3 4B (local)"
        }
      }
    }
  }
}
```

### RAM'e Göre Tam opencode.json Örnekleri

**8 GB RAM:**
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
          "name": "Qwen3 4B (local)"
        },
        "gemma4:e4b": {
          "name": "Gemma 4 E4B (local)"
        }
      }
    }
  }
}
```

**16 GB RAM:**
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
        "gemma4:e12b": {
          "name": "Gemma 4 E12B (local)"
        },
        "qwen3-coder:14b": {
          "name": "Qwen3 Coder 14B (local)"
        }
      }
    }
  }
}
```

**24+ GB RAM:**
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
        "qwen3-coder:30b-a3b": {
          "name": "Qwen3 Coder 30B-A3B (local)"
        },
        "gemma4:e27b": {
          "name": "Gemma 4 E27B (local)"
        }
      }
    }
  }
}
```

### Model İndirme Komutları (RAM'e göre)

**8 GB RAM:**
```bash
ollama pull qwen3:4b
ollama pull gemma4:e4b
```

**16 GB RAM:**
```bash
ollama pull gemma4:e12b
ollama pull qwen3-coder:14b
```

**24+ GB RAM:**
```bash
ollama pull qwen3-coder:30b-a3b
ollama pull gemma4:e27b
```

İndirilen tüm modelleri görmek için:
```bash
ollama list
```

---

## 5. Kodlama İçin Önerilen Modeller

Aşağıdaki tablo, RAM miktarınıza göre en uygun modelleri gösterir.

### 8 GB RAM

| Model | Neden? |
|-------|--------|
| **qwen3:4b** | Küçük boyutuna rağmen şaşırtıcı derecede yetenekli. 8 GB RAM'de rahatça çalışır. Basit kod üretimi, hata ayıklama ve küçük düzenlemeler için yeterli. |
| **gemma4:e4b** | Google'ın Gemma 4 ailesinin en küçük üyesi. Hızlı yanıt süresi ve düşük kaynak tüketimi ile hafif görevler için ideal. |

### 16 GB RAM

| Model | Neden? |
|-------|--------|
| **gemma4:e12b** | Gemma 4 ailesinin orta boy modeli. 4B versiyonuna göre çok daha tutarlı ve kaliteli kod üretir. Günlük kodlama görevlerinin çoğunu karşılar. |
| **qwen3-coder:14b** | Kodlamaya özel eğitilmiş model. Kod tamamlama, hata ayıklama ve refactoring'de boyutuna göre üstün performans gösterir. |

### 24+ GB RAM

| Model | Neden? |
|-------|--------|
| **qwen3-coder:30b-a3b** | MoE (Mixture of Experts) mimarisi sayesinde 30B parametre kapasitesinde ancak yalnızca 3B aktif parametre kullanır. Hem hızlı hem güçlü — yerel modeller arasında en iyi kodlama deneyimini sunar. |
| **gemma4:e27b** | Gemma 4 ailesinin büyük boy modeli. Karmaşık mimari kararlar ve çok dosyalı düzenlemeler dahil zorlu kodlama görevlerinde başarılı. |

> **Genel Kural:** RAM'inizin yaklaşık yarısını model için ayırın, kalanını işletim sistemi ve diğer uygulamalar kullansın.

---

## 6. Limitler ve İpuçları

### Kullanım Limitleri

| Limit Türü | Detay |
|------------|-------|
| Rate limit | Yok — yerel donanımınız kadar hızlı |
| Maliyet | $0 — tamamen ücretsiz |
| İnternet | Model indirdikten sonra gerekmiyor |
| Performans | GPU varsa çok daha hızlı; yalnızca CPU ile de çalışır |

### İpuçları

- **GPU kullanımı:** NVIDIA GPU'nuz varsa Ollama otomatik olarak GPU hızlandırma kullanır. Bu, yanıt süresini 5-10 kat hızlandırabilir
- **Birden fazla model:** Birden fazla model indirebilir ve `opencode.json`'da tanımlayabilirsiniz; `/models` ile aralarında geçiş yapabilirsiniz
- **Ollama'nın çalıştığından emin olun:** OpenCode'u başlatmadan önce Ollama servisinin çalışır durumda olduğunu doğrulayın
- **Model silme:** Artık kullanmadığınız modelleri `ollama rm model-adi` ile silerek disk alanı kazanabilirsiniz
- **Bulut yedek:** Karmaşık görevlerde yerel model yetersiz kalırsa bulut sağlayıcıya (Groq, OpenRouter) geçebilirsiniz

### Ollama Servisini Başlatma

Ollama normalde kurulumla birlikte arka planda çalışır. Çalışmıyorsa:

**Windows:** Görev çubuğunda Ollama simgesine tıklayın veya başlat menüsünden Ollama'yı açın.

**macOS:** Applications'dan Ollama'yı başlatın.

**Linux:**
```bash
ollama serve
```

---

## 7. Sorun Giderme

### "Connection refused" veya "ECONNREFUSED" hatası

- Ollama servisinin çalıştığından emin olun
- Terminalde `ollama list` komutunu çalıştırarak bağlantıyı test edin
- Windows'ta: Görev Yöneticisi'nde `ollama` sürecinin çalıştığını doğrulayın
- Port'un doğru olduğundan emin olun: `http://localhost:11434/v1`

### "Model not found" hatası

- Modelin indirildiğinden emin olun: `ollama list`
- Model adının `opencode.json` ile `ollama list` çıktısında birebir eşleştiğini kontrol edin
- Modeli henüz indirmediyseniz: `ollama pull model-adi`

### Yanıtlar çok yavaş geliyor

- Daha küçük bir model deneyin (örn. 14B yerine 4B)
- GPU varsa düzgün çalıştığını kontrol edin: `ollama ps`
- Diğer ağır uygulamaları kapatarak RAM boşaltın
- NVIDIA GPU kullanıcıları: güncel GPU sürücülerinin kurulu olduğundan emin olun

### "Out of memory" hatası

- Modelin boyutu RAM'iniz için çok büyük olabilir; daha küçük bir model deneyin
- Çalışan diğer uygulamaları kapatarak bellek boşaltın
- 8 GB RAM için 4B modeller, 16 GB için 12-14B modeller önerilir

### Sağlayıcı paketi hatası (AI_APICallError)

OpenCode sağlayıcı paketlerini dinamik olarak indirir. Hata alırsanız önbelleği temizleyin:

**Windows:**
`WIN+R` → `%USERPROFILE%\.cache\opencode` klasörünü silin.

**macOS/Linux:**
```bash
rm -rf ~/.cache/opencode
```

OpenCode'u yeniden başlatın — paketler otomatik indirilecektir.

---

## 8. Doğrulama

Bağlantının başarılı olduğunu doğrulamak için aşağıdaki adımları izleyin:

1. Ollama'nın çalıştığını doğrulayın:
   ```bash
   ollama list
   ```
   İndirdiğiniz modeller listelenmelidir.

2. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

3. Model seçin:
   ```
   /models
   ```
   Listede `ollama/qwen3:4b` gibi `opencode.json` dosyasında tanımladığınız modeller görünmelidir.

4. Test mesajı gönderin:
   ```
   FizzBuzz problemini çözen bir Python fonksiyonu yaz
   ```

5. Çalışan bir Python FizzBuzz fonksiyonu üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir FizzBuzz kodu. İlk yanıt birkaç saniye sürebilir — bu normaldir, model belleğe yükleniyor. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
