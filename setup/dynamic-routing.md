# Dinamik Yönlendirme — OpenCode CLI İleri Seviye Rehber

> Bu rehber, OpenRouter üzerinden çoklu sağlayıcı yönlendirmesi (provider routing) yapılandırmasını kapsar. Birden fazla sağlayıcıyı akıllıca yönlendirerek rate limit sorunlarını aşmak, maliyeti optimize etmek ve kesintisiz bir kodlama deneyimi sağlamak için kullanılır.

---

## 1. Dinamik Yönlendirme Nedir?

Dinamik yönlendirme, bir AI modeline yapılan isteğin **birden fazla sağlayıcı arasında otomatik olarak yönlendirilmesi** mekanizmasıdır.

### Neden Gerekli?

- **Rate limit aşımı:** Bir sağlayıcının limitine ulaştığınızda otomatik olarak başka bir sağlayıcıya geçiş
- **Maliyet optimizasyonu:** Aynı modeli sunan sağlayıcılar arasında en uygun fiyatlı olanı tercih etme
- **Kesintisiz çalışma:** Bir sağlayıcı çöktüğünde veya bakıma girdiğinde otomatik fallback
- **Gecikme optimizasyonu:** En hızlı yanıt veren sağlayıcıyı tercih etme

### Nasıl Çalışır?

OpenRouter, aynı modeli birden fazla altyapı sağlayıcısı (inference provider) üzerinden sunar. Örneğin `moonshotai/kimi-k2` modeli Baseten, Together AI veya Fireworks gibi farklı sağlayıcılarda barındırılabilir. Yönlendirme kuralları ile bu sağlayıcılar arasındaki öncelik sırasını ve fallback davranışını kontrol edebilirsiniz.

---

## 2. OpenRouter ile Yönlendirme

OpenRouter, yönlendirme için iki temel parametre sunar:

### `order` — Sağlayıcı Öncelik Sırası

Hangi altyapı sağlayıcısının öncelikli kullanılacağını belirler. İlk sağlayıcı başarısız olursa sıradakine geçilir.

```json
"order": ["baseten", "together-ai", "fireworks"]
```

Bu örnekte:
1. Önce **Baseten** denenir
2. Baseten başarısız olursa **Together AI** denenir
3. O da başarısız olursa **Fireworks** denenir

### `allow_fallbacks` — Otomatik Yedekleme

`order` listesindeki tüm sağlayıcılar başarısız olduğunda, OpenRouter'ın kendi seçimiyle başka bir sağlayıcıya yönlendirme yapıp yapamayacağını kontrol eder.

| Değer | Davranış |
|-------|----------|
| `true` (varsayılan) | Liste dışı sağlayıcılara da otomatik yönlendirme yapılır |
| `false` | Yalnızca `order` listesindeki sağlayıcılar kullanılır; hepsi başarısız olursa hata döner |

---

## 3. opencode.json Yapılandırma Örnekleri

### Örnek 1 — Belirli Sağlayıcıya Sabitleme

Bir modeli yalnızca belirli bir altyapı sağlayıcısı üzerinden kullanmak istiyorsanız:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openrouter": {
      "models": {
        "moonshotai/kimi-k2": {
          "options": {
            "provider": {
              "order": ["baseten"],
              "allow_fallbacks": false
            }
          }
        }
      }
    }
  }
}
```

Bu yapılandırma `kimi-k2` modelini **yalnızca Baseten** üzerinden kullanır. Baseten çalışmıyorsa istek başarısız olur (başka sağlayıcıya yönlendirme yapılmaz).

### Örnek 2 — Öncelikli Sağlayıcı + Fallback

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openrouter": {
      "models": {
        "moonshotai/kimi-k2": {
          "options": {
            "provider": {
              "order": ["baseten", "together-ai"],
              "allow_fallbacks": true
            }
          }
        }
      }
    }
  }
}
```

Bu yapılandırma:
1. Önce **Baseten** dener
2. Baseten başarısız olursa **Together AI** dener
3. İkisi de başarısız olursa OpenRouter **otomatik olarak başka bir sağlayıcı** seçer

### Örnek 3 — Birden Fazla Model İçin Yönlendirme

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openrouter": {
      "models": {
        "moonshotai/kimi-k2": {
          "options": {
            "provider": {
              "order": ["baseten"],
              "allow_fallbacks": false
            }
          }
        },
        "qwen/qwen3-coder": {
          "options": {
            "provider": {
              "order": ["together-ai", "fireworks"],
              "allow_fallbacks": true
            }
          }
        }
      }
    }
  }
}
```

Her model için farklı yönlendirme kuralları tanımlayabilirsiniz. Bu örnekte:
- `kimi-k2` yalnızca Baseten üzerinden çalışır
- `qwen3-coder` önce Together AI, sonra Fireworks dener ve fallback açıktır

---

## 4. Kullanım Senaryoları

### Senaryo 1 — Rate Limit Aşımında Otomatik Geçiş

Yoğun bir kodlama seansında tek bir sağlayıcının rate limit'ine takılmak iş akışınızı bozar. Birden fazla sağlayıcı tanımlayarak kesintisiz çalışabilirsiniz.

**Çözüm:** `order` listesine birden fazla sağlayıcı ekleyin ve `allow_fallbacks: true` yapın.

### Senaryo 2 — Tutarlı Performans İçin Sabitleme

Bazı altyapı sağlayıcıları aynı model için farklı performans profilleri sunabilir. Belirli bir sağlayıcının yanıt kalitesinden memnunsanız o sağlayıcıya sabitleyebilirsiniz.

**Çözüm:** `order` listesine tek sağlayıcı yazın ve `allow_fallbacks: false` yapın.

### Senaryo 3 — Maliyet Optimizasyonu

Farklı sağlayıcılar aynı model için farklı fiyatlandırma uygulayabilir. En uygun fiyatlı sağlayıcıyı öncelikli yaparak maliyeti düşürebilirsiniz.

**Çözüm:** `order` listesini fiyat sırasına göre düzenleyin (en ucuz ilk sırada).

### Senaryo 4 — Bölgesel Gecikme Optimizasyonu

Coğrafi konumunuza en yakın veri merkezindeki sağlayıcıyı öncelikli yaparak gecikmeyi azaltabilirsiniz.

**Çözüm:** Gecikme testleri yaparak en hızlı sağlayıcıyı `order` listesinin başına koyun.

---

## 5. İpuçları

### Genel Öneriler

- **Önce OpenRouter'ı bağlayın:** Yönlendirme özelliğini kullanmak için önce `/connect` ile OpenRouter sağlayıcısını bağlamanız gerekir
- **Sağlayıcı isimlerini doğrulayın:** `order` listesindeki sağlayıcı isimleri OpenRouter'ın tanımladığı isimlerle eşleşmelidir; yanlış isim sessizce atlanır
- **Test edin:** Yapılandırmayı değiştirdikten sonra basit bir test mesajı göndererek çalıştığını doğrulayın
- **Model uyumluluğu:** Her model her sağlayıcıda mevcut olmayabilir; [openrouter.ai/models](https://openrouter.ai/models) üzerinden mevcut sağlayıcıları kontrol edin

### Ne Zaman Kullanmalı?

| Durum | Yönlendirme Gerekli mi? |
|-------|:----------------------:|
| Tek sağlayıcı, düşük kullanım | Hayır |
| Yoğun kodlama seansları | Evet — rate limit fallback |
| Üretim ortamı güvenilirliği | Evet — kesintisiz çalışma |
| Maliyet hassasiyeti | Evet — fiyat optimizasyonu |
| Tek sağlayıcı yeterli | Hayır — gereksiz karmaşıklık |

### Ne Zaman Kullanmamalı?

- **Başlangıç seviyesinde:** Önce temel sağlayıcı bağlantısını öğrenin, yönlendirmeyi sonra ekleyin
- **Tek model yeterliyse:** Bir sağlayıcı sorunsuz çalışıyorsa yönlendirme eklemeye gerek yoktur
- **Hata ayıklama sırasında:** Karmaşık yönlendirme yapılandırması hata ayıklamayı zorlaştırabilir; önce basit yapılandırmayla test edin

---

> **Not:** Bu rehber OpenRouter'ın sağlayıcı yönlendirmesini kapsar. OpenCode CLI'nin temel kullanımı için [OpenCode CLI Kurulumu](opencode-cli.md), OpenRouter bağlantısı için [OpenRouter Kurulumu](openrouter.md) rehberlerine bakın.

---
*Son güncelleme: Nisan 2026*
*Kurs: AI Kodlama Ajanları ile Yazılım Geliştirme — BackendGuru*
