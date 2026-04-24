# AI Kodlama Ajanları ile Yazılım Geliştirme

**Eğitmen:** Emre Akış — [BackendGuru](https://backendguru.io)
**Format:** 8 hafta, haftada 2 oturum (Pzt & Çar), 20:00-22:00 (toplam 32 saat)
**Araç:** [OpenCode CLI](https://opencode.ai)
**Dil:** Python
**Yaklaşım:** 40/20/40 kuralı (%40 planlama, %20 kodlama, %40 test)

---

## Bu Repo Nedir?

Bu repo kursun **kod referans kaynağıdır**. Her oturumdaki canlı kodlama demolarının kodları ders sonrası buraya push edilir.

- **Slaytlar ve oturum notları** ayrı platformda paylaşılır
- **Ödevler** [GitHub Classroom](https://classroom.github.com/) üzerinden verilir ve teslim edilir

## Oturumlar

| # | Oturum | Hafta | Konu |
|---|--------|-------|------|
| 01 | [İlk Agent Görevin](sessions/01-first-agent-task/) | 1 | Agent loop, OpenCode TUI, FastAPI demo |
| 02 | [Plan vs Build & AGENTS.md](sessions/02-plan-vs-build/) | 1 | Plan/Build modları, AGENTS.md |
| 03 | [Prompt Teknikleri](sessions/03-prompt-techniques/) | 2 | CoT, few-shot, RAG, tool calling |
| 04 | [Model Seçimi & Karşılaştırma](sessions/04-model-comparison/) | 2 | Yerel vs bulut, compaction, @ targeting |
| 05 | [Repo Keşfi & Navigasyon](sessions/05-repo-exploration/) | 3 | @ targeting, custom commands, SKILL.md |
| 06 | [Debug & Güvenlik](sessions/06-debug-workflow/) | 3 | ! injection, LSP, permissions |
| 07 | [Gereksinim Toplama](sessions/07-requirements/) | 4 | User stories, Generator-Evaluator |
| 08 | [API Tasarımı & Tehdit Modeli](sessions/08-api-design/) | 4 | OpenAPI, STRIDE, çapraz kontrol |
| 09 | [Red/Green TDD](sessions/09-tdd-red-green/) | 5 | TDD döngüsü, küçük commitler, PR |
| 10 | [Paralel Ajanlar & Kod İncelemesi](sessions/10-parallel-agents/) | 5 | git worktree, AI vs human review |
| 11 | [MCP Temelleri](sessions/11-mcp-basics/) | 6 | MCP protokolü, tools, resources |
| 12 | [Sıfırdan MCP Sunucu](sessions/12-mcp-server/) | 6 | Python MCP SDK, sunucu geliştirme |
| 13 | [AI-on-AI Test & Semgrep](sessions/13-ai-testing/) | 7 | LLM-as-Judge, SAST |
| 14 | [Kalite Metrikleri & Otomasyon](sessions/14-quality-metrics/) | 7 | Coverage, GitHub Actions, opencode run |
| 15 | [Capstone Geliştirme](sessions/15-capstone-dev/) | 8 | Uçtan uca SDLC |
| 16 | [Demo & Kapanış](sessions/16-capstone-demo/) | 8 | Öğrenci demoları, retrospektif |

## Ön Koşullar

- Python 3.12+
- [OpenCode CLI](https://opencode.ai) kurulu
- En az 1 LLM provider bağlı (Ollama, Groq, veya OpenRouter)
- Git temel bilgisi

### LLM Sağlayıcı Seçenekleri

| Seviye | Sağlayıcılar | Maliyet |
|--------|-------------|---------|
| Minimum (zorunlu) | Ollama (qwen3:4b) + Groq | Ücretsiz |
| Optimal | Ollama + Groq + OpenRouter | Ücretsiz |
| Premium | Ollama + OpenCode Zen | ~$20/ay |

## Kaynaklar

- [Okuma Listesi](reading-list.md) — her oturum için küratörlü kaynaklar
- [OpenCode Docs](https://opencode.ai/docs/) — resmi dokümantasyon
- [Provider Kurulum Rehberi](setup/) — OpenCode CLI kurulumu ve tüm sağlayıcılar için adım adım kurulum
