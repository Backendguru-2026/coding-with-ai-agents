# LM Studio Kurulumu

> LM Studio, yerel bilgisayarınızda AI modelleri çalıştırmanızı sağlayan ücretsiz, GUI tabanlı bir masaüstü uygulamasıdır. Grafiksel arayüzü sayesinde model indirme, yönetme ve test etme işlemleri görsel olarak yapılır. Kredi kartı gerektirmez.

---

## 1. Genel Bakış ve Fiyatlandırma

| Özellik | Detay |
|---------|-------|
| **Sağlayıcı** | LM Studio (yerel) |
| **Bağlantı Türü** | opencode.json yapılandırması (`@ai-sdk/openai-compatible`) |
| **Maliyet** | $0 — tamamen ücretsiz |
| **Kredi Kartı** | Hayır, gerekmiyor |
| **İnternet** | Yalnızca model indirme için gerekli; sonrasında çevrimdışı çalışır |
| **Gereksinim** | Minimum 8 GB RAM (model boyutuna göre daha fazla önerilir) |
| **Resmi Site** | [lmstudio.ai](https://lmstudio.ai) |

**Not:** LM Studio, Ollama'nın GUI alternatifidir. Terminal yerine grafiksel arayüz tercih edenler veya modelleri görsel olarak keşfetmek isteyenler için idealdir. Dahili model mağazası ile tek tıkla model indirme imkanı sunar.

---

## 2. Hesap Oluşturma

LM Studio için hesap oluşturmaya **gerek yoktur**. Yalnızca yazılımı indirip kurmanız yeterlidir.

### Windows

1. [lmstudio.ai](https://lmstudio.ai) adresine gidin
2. **Download for Windows** butonuna tıklayın
3. İndirilen `.exe` dosyasını çalıştırın ve kurulum sihirbazını takip edin
4. Kurulum tamamlandığında LM Studio'yu başlatın

### macOS

1. [lmstudio.ai](https://lmstudio.ai) adresine gidin
2. **Download for Mac** butonuna tıklayın (Apple Silicon veya Intel)
3. İndirilen `.dmg` dosyasını açıp LM Studio'yu Applications klasörüne sürükleyin
4. Uygulamayı başlatın

### Linux

1. [lmstudio.ai](https://lmstudio.ai) adresine gidin
2. **Download for Linux** butonuna tıklayın
3. `.AppImage` dosyasını indirin ve çalıştırılabilir yapın:
   ```bash
   chmod +x LM-Studio-*.AppImage
   ./LM-Studio-*.AppImage
   ```

### İşletim Sistemi Notları

- **Windows / macOS / Linux:** Arayüz ve işlevsellik tüm platformlarda aynıdır. Tek fark kurulum dosyası formatıdır.

---

## 3. Kimlik Doğrulama

LM Studio yerel çalıştığı için **kimlik doğrulama gerekmez**. API anahtarı, hesap veya token yoktur. LM Studio'nun yerel sunucusu çalışır durumda olduğu sürece OpenCode doğrudan bağlanabilir.

---

## 4. OpenCode CLI Bağlantısı

LM Studio, `opencode.json` dosyası ile yapılandırılır. Üç adımda tamamlanır: model indirme, sunucu başlatma ve OpenCode yapılandırması.

### Adım 1 — LM Studio'da Model İndirme

1. LM Studio'yu açın
2. Sol menüden **Discover** (keşfet) sekmesine gidin
3. Arama çubuğuna model adını yazın (örn. `qwen3 4b`)
4. İstediğiniz modelin yanındaki **Download** butonuna tıklayın
5. İndirme tamamlanana kadar bekleyin

### Adım 2 — Yerel Sunucuyu Başlatma

1. Sol menüden **Developer** sekmesine gidin
2. Üst kısımdan yüklediğiniz modeli seçin
3. **Start Server** butonuna tıklayın
4. Sunucu varsayılan olarak `http://localhost:1234` adresinde başlar
5. "Server started" mesajını gördüğünüzde sunucu hazırdır

> **Önemli:** OpenCode'u kullanmadan önce LM Studio'nun yerel sunucusunun çalışır durumda olduğundan emin olun. Sunucu kapalıysa OpenCode bağlanamaz.

### Adım 3 — opencode.json Yapılandırması

Projenizin kök dizininde `opencode.json` dosyası oluşturun:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://localhost:1234/v1"
      },
      "models": {
        "qwen3-4b": {
          "name": "Qwen3 4B (local)"
        }
      }
    }
  }
}
```

> **Not:** `models` bölümündeki model ID'si, LM Studio'da yüklediğiniz modelin adıyla eşleşmelidir. LM Studio'nun Developer sekmesinde model adını görebilirsiniz.

### RAM'e Göre Tam opencode.json Örnekleri

**8 GB RAM:**
```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://localhost:1234/v1"
      },
      "models": {
        "qwen3-4b": {
          "name": "Qwen3 4B (local)"
        },
        "gemma-4-e4b": {
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
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://localhost:1234/v1"
      },
      "models": {
        "gemma-4-e12b": {
          "name": "Gemma 4 E12B (local)"
        },
        "qwen3-coder-14b": {
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
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://localhost:1234/v1"
      },
      "models": {
        "qwen3-coder-30b-a3b": {
          "name": "Qwen3 Coder 30B-A3B (local)"
        },
        "gemma-4-e27b": {
          "name": "Gemma 4 E27B (local)"
        }
      }
    }
  }
}
```

> **Dikkat:** LM Studio'daki model ID formatı Ollama'dan farklı olabilir. LM Studio'nun Developer sekmesinde gösterilen model adını kullanın.

---

## 5. Kodlama İçin Önerilen Modeller

LM Studio'nun Discover sekmesinden aşağıdaki modelleri arayıp indirebilirsiniz.

### 8 GB RAM

| Model | Neden? |
|-------|--------|
| **Qwen3 4B** | Küçük boyutuna rağmen şaşırtıcı derecede yetenekli. 8 GB RAM'de rahatça çalışır. Basit kod üretimi, hata ayıklama ve küçük düzenlemeler için yeterli. |
| **Gemma 4 E4B** | Google'ın Gemma 4 ailesinin en küçük üyesi. Hızlı yanıt süresi ve düşük kaynak tüketimi ile hafif görevler için ideal. |

### 16 GB RAM

| Model | Neden? |
|-------|--------|
| **Gemma 4 E12B** | Gemma 4 ailesinin orta boy modeli. 4B versiyonuna göre çok daha tutarlı ve kaliteli kod üretir. Günlük kodlama görevlerinin çoğunu karşılar. |
| **Qwen3 Coder 14B** | Kodlamaya özel eğitilmiş model. Kod tamamlama, hata ayıklama ve refactoring'de boyutuna göre üstün performans gösterir. |

### 24+ GB RAM

| Model | Neden? |
|-------|--------|
| **Qwen3 Coder 30B-A3B** | MoE mimarisi sayesinde 30B parametre kapasitesinde ancak yalnızca 3B aktif parametre kullanır. Hem hızlı hem güçlü — yerel modeller arasında en iyi kodlama deneyimini sunar. |
| **Gemma 4 E27B** | Gemma 4 ailesinin büyük boy modeli. Karmaşık mimari kararlar ve çok dosyalı düzenlemeler dahil zorlu kodlama görevlerinde başarılı. |

> **LM Studio Avantajı:** Discover sekmesinde modellerin boyutu, açıklaması ve popülerlik sıralaması görsel olarak gösterilir. Hangi modelin RAM'inize uygun olduğunu indirmeden önce görebilirsiniz.

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

- **Sunucuyu başlatmayı unutmayın:** OpenCode'u kullanmadan önce LM Studio'da Developer sekmesinden sunucunun çalıştığından emin olun
- **Model test etme:** LM Studio'nun kendi sohbet arayüzünde modeli test edebilirsiniz; beğendiğinizi `opencode.json`'a ekleyin
- **GPU kullanımı:** NVIDIA veya Apple Silicon GPU'nuz varsa LM Studio otomatik olarak GPU hızlandırma kullanır
- **Birden fazla model:** Birden fazla model indirebilirsiniz ancak sunucuda aynı anda yalnızca bir model yüklü olabilir
- **Bulut yedek:** Karmaşık görevlerde yerel model yetersiz kalırsa bulut sağlayıcıya (Groq, OpenRouter) geçebilirsiniz

---

## 7. Sorun Giderme

### "Connection refused" veya "ECONNREFUSED" hatası

- LM Studio'nun açık ve sunucunun çalışır durumda olduğundan emin olun
- Developer sekmesinde "Server started" mesajını görmelisiniz
- Port'un doğru olduğundan emin olun: `http://localhost:1234/v1`
- Farklı bir port kullandıysanız `opencode.json`'daki `baseURL`'i güncelleyin

### "Model not found" veya boş yanıt

- LM Studio'da bir modelin sunucuya yüklü olduğundan emin olun
- `opencode.json`'daki model ID'sinin LM Studio'da gösterilene eşleştiğini kontrol edin
- Modeli yeniden yükleyin: Developer sekmesinde modeli seçip tekrar yükleyin

### Yanıtlar çok yavaş geliyor

- Daha küçük bir model deneyin (örn. 14B yerine 4B)
- LM Studio ayarlarından GPU kullanımını kontrol edin
- Diğer ağır uygulamaları kapatarak RAM boşaltın
- NVIDIA GPU kullanıcıları: güncel GPU sürücülerinin kurulu olduğundan emin olun

### "Out of memory" hatası

- Modelin boyutu RAM'iniz için çok büyük olabilir; daha küçük bir model deneyin
- LM Studio ayarlarından bağlam boyutunu (context length) küçültün
- Çalışan diğer uygulamaları kapatarak bellek boşaltın

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

1. LM Studio'da sunucunun çalıştığını doğrulayın:
   - Developer sekmesine gidin
   - "Server started" mesajı görünmelidir
   - Eğer görünmüyorsa bir model seçip **Start Server** butonuna tıklayın

2. OpenCode CLI'yi başlatın:
   ```bash
   opencode
   ```

3. Model seçin:
   ```
   /models
   ```
   Listede `lmstudio/qwen3-4b` gibi `opencode.json` dosyasında tanımladığınız modeller görünmelidir.

4. Test mesajı gönderin:
   ```
   Bir sayının asal olup olmadığını kontrol eden Python fonksiyonu yaz
   ```

5. Çalışan bir Python asal-sayı fonksiyonu üretilmelidir.

**Beklenen çıktı:** Doğru çalışan bir asal kontrol fonksiyonu. İlk yanıt birkaç saniye sürebilir — bu normaldir, model belleğe yükleniyor. Hata mesajı yoksa bağlantı başarılıdır.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
