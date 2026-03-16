# SEO Monthly Report — Claude Skill

Marka adı ve ay verin, gerisini bu skill halleder: API'lerden veri çekme, hesaplama, chart üretimi, insight yazımı ve PPTX sunum oluşturma.

Inbound dijital pazarlama ajansının tüm markaları için çalışır. Google APIs (GA4, GSC, Ads) ve SEOmonitor API entegrasyonuyla veri çeker.

---

## Ne İşe Yarar?

Her ay müşteri markalar için hazırlanan **Aylık SEO Değerlendirme Sunumu**nu otomatik oluşturur.

23 slide'lık bir sunum üretir — arkada şunları yapar:

- **Google Ads API** ile brand, brand+category, non-brand ve rakip keyword'lerin aylık arama hacimlerini çeker
- **GA4 Data API** ile organik trafik, kanal performansı, AI referral trafik verilerini çeker
- **Search Console API** ile impression, click, CTR, position verilerini brand/non-brand kırılımında çeker
- **SEOmonitor API** ile visibility, share of clicks, AI search SoV, kategori visibility, keyword ranking verilerini çeker
- YoY ve MoM değişimleri hesaplar, conditional formatting uygular
- Grouped bar, combo (stacked bar + line), pie chart gibi görseller üretir
- Her slide için Türkçe insight cümleleri oluşturur (dinamik, veriye göre değişen)
- Tüm bunları Inbound sunum tasarımına uygun bir PPTX dosyasına dönüştürür

---

## Sunum İçeriği (23 Slide)

| # | Slide | Veri Kaynağı |
|---|-------|-------------|
| 1 | Kapak | — |
| 2 | Sunum Akışı | — |
| 3 | 🔷 Bölüm: Arama Hacmi & Rekabet | — |
| 4 | Brand Arama Hacmi (chart + tablo) | Google Ads API |
| 5 | Brand + Category Hacmi (chart + tablo) | Google Ads API |
| 6 | Non-Branded Hacim (chart + tablo) | Google Ads API |
| 7 | Segment Kırılım (4 tablo + insight) | Google Ads API |
| 8 | Rekabet Tablosu (büyük tablo + insight) | Google Ads API |
| 9 | 🔷 Bölüm: GA4 Trafik | — |
| 10 | GA4 Kanal Performansı (tablo + mini tablo) | GA4 Data API |
| 11 | GA4 Organik Detay (combo chart + tablo) | GA4 Data API |
| 12 | 🔷 Bölüm: GSC & Visibility | — |
| 13 | GSC Brand & Non-Brand (KPI kartları + tablo) | Search Console API |
| 14 | GSC Impression/Click Trend (2 bar chart) | Search Console API |
| 15 | Competitor Visibility (tablo + insight) | SEOmonitor API |
| 16 | Share of Clicks & AI SoV (2 pie chart) | SEOmonitor API |
| 17 | Kategori Bazında Visibility (liste + insight) | SEOmonitor API |
| 18 | Top Impact Keywords (tablo) | SEOmonitor API |
| 19 | Top & Rising Keywords (2 panel tablo) | SEOmonitor API |
| 20 | AI Referral Trafik (tablo + insight) | GA4 Data API |
| 21 | GSC Keyword Performansı — İyileşen/Kötüleşen | Search Console API |
| 22 | GSC Sayfa Performansı — İyileşen/Kötüleşen | Search Console API |
| 23 | Teşekkürler | — |

---

## Kurulum

### Adım 1: Repo'yu Klonla

```bash
git clone https://github.com/erdogan1ozdemir/seo-monthly-report.git
```

### Adım 2: Skill'i Claude Code'a Kur

```bash
# Global olarak kur (tüm projelerde kullanılabilir)
mkdir -p ~/.claude/skills
cp -r seo-monthly-report/ ~/.claude/skills/seo-monthly-report/
```

### Adım 3: API Bağlantıları

Skill, WHC platformu üzerinden bağlı olan API'leri kullanır. Alternatif olarak Google API'lerine doğrudan tool call da yapılabilir.

**Gerekli API'ler:**

| API | Kullanım | Slides |
|-----|----------|--------|
| Google Ads API (Keyword Planner) | Keyword arama hacimleri | 4–8 |
| GA4 Data API | Trafik, organik performans, AI referral | 10, 11, 20 |
| Search Console API | Impression, click, keyword/sayfa performansı | 13, 14, 21, 22 |
| SEOmonitor API | Visibility, SoC, AI SoV, keyword sıralamaları | 15–19 |

### Adım 4: Marka Profili Oluştur

Her marka için `references/brand-profiles/` altında bir JSON dosyası oluştur:

```bash
cp references/brand-profiles/vitra.json references/brand-profiles/yeni-marka.json
# Düzenle: domain, API ID'ler, rakipler, segmentler
```

Profil yapısı için [Marka Profili Şablonu](#marka-profili-şablonu) bölümüne bak.

---

## Nasıl Kullanılır?

### Temel Kullanım

```
VitrA için Şubat 2026 sunumu hazırla
```

Claude şunları sorar:
1. **Keyword veri kaynağı:** SEOmonitor gruplarından mı çekeyim, CSV mi yükleyeceksin, WHC'den mi alayım?

Cevapladıktan sonra tüm API çağrılarını yapar ve PPTX dosyasını oluşturur.

### CSV ile Kullanım

```
Flormar için Mart 2026 SEO sunumu hazırla. Keyword verilerini ekteki CSV'den al.
[CSV dosyası yükle]
```

CSV formatı:
```
keyword,segment,type,jan_2024,feb_2024,...,mar_2026
klozet,Vitrifiyeler,nonbrand,74000,60500,...,65000
vitra klozet,Vitrifiyeler,brand_cat,6600,5400,...,5800
```

### Farklı Markalar

```
Turkcell Pasaj için Ocak 2026 aylık SEO değerlendirme sunumunu oluştur
```

```
Özdilekteyim için Nisan 2026 SEO raporu hazırla
```

---

## Dosya Yapısı

```
seo-monthly-report/
├── SKILL.md                                    # Ana workflow: config → veri → chart → insight → PPTX
├── README.md                                   # Bu dosya
├── LICENSE                                     # MIT License
├── .gitignore
├── references/
│   ├── slide-mapping.md                        # Her slide için: API, tablo yapısı, chart spec, insight template
│   ├── design-system.md                        # Renkler, fontlar, layout kuralları, slide tipleri
│   ├── chart-specs.md                          # matplotlib chart rendering kodu (4 chart tipi)
│   └── brand-profiles/                         # Marka bazlı config dosyaları
│       ├── vitra.json                          # VitrA örnek profili
│       └── _template.json                      # Boş şablon — yeni marka eklerken kopyala
└── assets/
    └── .gitkeep                                # Inbound logosu vb. sabit görseller için
```

### Dosya Açıklamaları

| Dosya | Boyut | İçerik |
|-------|-------|--------|
| `SKILL.md` | ~310 satır | Ana akış: Step 0 (config) → Step 1 (veri çekme) → Step 2 (processing) → Step 3 (chart) → Step 4 (insight) → Step 5 (PPTX) → Step 6 (QA) |
| `slide-mapping.md` | ~465 satır | 23 slide'ın her biri için: API kaynağı, tablo kolon/satır yapısı, chart tipi/renk/eksen, insight cümlesi şablonu (değişkenli) |
| `design-system.md` | ~280 satır | Renk paleti (hex kodlu), font ailesi/boyut, 10 slide tipi layout şeması (ASCII), ortak elemanlar |
| `chart-specs.md` | ~265 satır | 4 chart tipi için matplotlib kodu: Grouped Bar (2 seri), Grouped Bar (3 seri), Combo Stacked Bar+Line, Dual Pie |
| `brand-profiles/*.json` | ~50 satır/dosya | Marka: domain, API ID'ler, brand regex, segmentler, rakipler, AI referral kaynakları |

---

## Marka Profili Şablonu

`references/brand-profiles/_template.json` dosyasını kopyalayıp düzenle:

```json
{
  "brand": "Marka Adı",
  "domain": "marka.com.tr",
  "brand_regex": "marka|Marka|MARKA",
  "ga4_property_id": "properties/XXXXXXXXX",
  "seomonitor_campaign_id": "XXXXX",
  "gsc_site_url": "sc-domain:marka.com.tr",
  "location_code": 2792,
  "language_code": "tr",
  "segments": [
    "Kategori 1",
    "Kategori 2"
  ],
  "competitors": [
    {"name": "rakip1", "keyword": "rakip1"},
    {"name": "rakip2", "keyword": "rakip2"}
  ],
  "competitor_domains": [
    "rakip1.com",
    "rakip2.com"
  ],
  "ai_referral_sources": [
    "chatgpt.com",
    "gemini.google.com",
    "perplexity.ai",
    "copilot.microsoft.com",
    "claude.ai"
  ]
}
```

### Gerekli Alanlar

| Alan | Açıklama | Nereden Bulunur |
|------|----------|----------------|
| `brand` | Marka adı (sunumda görünecek) | — |
| `domain` | Analiz edilecek domain | — |
| `brand_regex` | GSC'de brand query filtrelemesi için | Brand adının varyantları |
| `ga4_property_id` | GA4 API çağrıları için | GA4 Admin → Property Settings |
| `seomonitor_campaign_id` | SEOmonitor API çağrıları için | SEOmonitor → Campaign Settings |
| `gsc_site_url` | Search Console API için | GSC → Property seçimi |
| `segments` | Slide 7 ve 17 için kategori kırılımları | Marka ürün kategorileri |
| `competitors` | Slide 8 için rakip brand keyword'leri | Sektörel rakip analizi |

---

## Claude.ai'de Kullanım

Claude Code yerine claude.ai'de de kullanabilirsin:

1. Releases sayfasından `seo-monthly-report.skill` dosyasını indir
2. Claude.ai → `Settings > Capabilities` → Code execution açık olmalı
3. `Customize > Skills` → Upload skill → `.skill` dosyasını yükle
4. Toggle'ı aç ve yeni sohbette kullan

> claude.ai'de MCP entegrasyonu olmadığından, API verilerini CSV/Excel olarak yüklemeniz gerekir.

---

## Sunum Tasarımı

Skill, Inbound ajansı için özelleştirilmiş bir tasarım sistemi kullanır:

| Element | Değer |
|---------|-------|
| **Ana renk** | Coral `#FF7B52` (kapak, badge, vurgu) |
| **İkincil renk** | Koyu yeşil `#10332F` (section divider, tablo header) |
| **Arka plan** | Kırık beyaz `#FAF8F5` (saf beyaz değil) |
| **Başlık fontu** | Bricolage Grotesque ExtraBold |
| **Gövde fontu** | Calibri |
| **Slide boyutu** | 26.67 × 15.00 inch (16:9 widescreen) |

Detaylı tasarım kuralları `references/design-system.md` dosyasında.

---

## Katkıda Bulunma

- **Yeni marka profili** → `references/brand-profiles/` altına JSON ekle, PR at
- **Yeni slide tipi** → `references/slide-mapping.md`'ye slide spec ekle, `SKILL.md`'de slide listesini güncelle
- **Yeni chart tipi** → `references/chart-specs.md`'ye matplotlib kodu ekle
- **Tasarım değişikliği** → `references/design-system.md`'yi güncelle
- **Bug veya öneri** → Issue aç

## Lisans

MIT
