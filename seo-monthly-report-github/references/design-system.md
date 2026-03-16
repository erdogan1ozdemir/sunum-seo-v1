# SEO Aylık Değerlendirme — Sunum Design Mapping (Generic)

> Bu dosya tüm Inbound markaları için ortak tasarım şablonudur.
> Marka-spesifik veriler `[brand]`, `[domain]`, `[Ay]`, `[Yıl]` placeholder'ları ile belirtilmiştir.
> Her marka için `config/brand_profile.json` dosyasından keyword listeleri, rakipler, segmentler ve API credential'ları okunur.

---

## 1. GENEL BİLGİLER

| Özellik | Değer |
|---------|-------|
| **Format** | Google Slides → PPTX (veya python-pptx/pptxgenjs ile oluşturma) |
| **Slide Boyutu** | 26.67 × 15.00 inch (EMU: 24387175 × 13716000) — Widescreen 16:9 |
| **Toplam Slide** | 24 slide (standart akış) |

---

## 2. RENK PALETİ

### Ana Renkler

| Rol | HEX | RGB | Kullanım |
|-----|-----|-----|----------|
| **Coral (Primary Accent)** | `#FF7B52` | 255,123,82 | Kapak/kapanış bg, breadcrumb metin, source badge bg, vurgulu rakamlar |
| **Koyu Yeşil (Dark Teal)** | `#10332F` | 16,51,47 | Section divider bg, tablo header bg, breadcrumb metin (TOC sağ panel) |
| **Kırık Beyaz (Warm White)** | `#FAF8F5` | 250,248,245 | İçerik slide bg (saf beyaz DEĞİL) |
| **Siyah** | `#000000` | 0,0,0 | Ana gövde metin |
| **Beyaz** | `#FFFFFF` | 255,255,255 | Kapak/divider metin, tablo header metin |

### Data Renkleri

| Rol | HEX | Kullanım |
|-----|-----|----------|
| **Negatif (kırmızı)** | `#E74C3C` | Düşüş yüzdeleri, kırmızı metin vurgu |
| **Pozitif (yeşil)** | `#27AE60` | Artış yüzdeleri, yeşil metin vurgu |
| **Yeşil gradient (tablo)** | `#57BB8A` → `#9DD8BB` → `#C5E8D7` → `#E4F4EC` | Conditional formatting: güçlü pozitif → hafif pozitif |
| **Kırmızı gradient (tablo)** | `#E67C73` → `#EA948D` → `#F2BCB7` → `#FDF7F7` | Conditional formatting: güçlü negatif → hafif negatif |
| **Sarı/Amber** | `#FFD666` / `#FFF8E1` | Chart seri rengi (organic sessions), uyarı notu bg |
| **Gri** | `#B7B7B7` | Chart seri rengi (revenue bar), inaktif veri |
| **Mavi (chart)** | `#4472C4` | Chart seri rengi (önceki yıl bar) |
| **Kırmızı (chart)** | `#FF0000` | Chart seri rengi (güncel yıl bar), line seri |
| **Turuncu** | `#F4845F` | Öne çıkan metrik vurguları |

---

## 3. YAZI TİPLERİ

| Font | Ağırlık | Kullanım | Boyut (pt) |
|------|---------|----------|------------|
| **Bricolage Grotesque** | ExtraBold | Slide başlıkları (h1), büyük KPI sayılar | 52-75pt |
| **Bricolage Grotesque** | SemiBold | Breadcrumb/section label | 24-30pt |
| **Bricolage Grotesque** | Regular | Alt başlık, insight paragraf | 21-29pt |
| **Outfit** | SemiBold | Section divider watermark numara | ~400pt |
| **Outfit** | ExtraLight | Kapak başlığı, kapanış "Teşekkürler" | 86-96pt |
| **Outfit** | Regular | Kapak alt başlık "[Ay] [Yıl]" | 86pt |
| **Calibri** | Regular | Tablo verileri, gövde metin | 14-20pt |
| **Inter Medium** | Medium | KPI kart etiketleri | 14-16pt |
| **Ubuntu** | Regular | Source badge metin | 14pt |
| **Arial** | Regular | Fallback | — |

---

## 4. SLIDE TİPLERİ

### A) KAPAK (Slide 1)

- **BG:** Coral (#FF7B52) düz
- **Sol:** Yarı-saydam konsantrik daire motifi (açık coral tonu)
- **Orta:** "[brand] | Aylık SEO Değerlendirme" (Outfit ExtraLight, ~95pt, beyaz) + "[Ay] [Yıl]" (Outfit Regular, ~86pt, beyaz)
- **Alt orta:** Inbound wordmark logosu (beyaz)

### B) İÇİNDEKİLER (Slide 2)

- **Sol yarı:** Coral bg + daire motifi + "SUNUM AKIŞI" (Outfit ExtraLight, ~96pt, beyaz) + sol alt Inbound ikonu
- **Sağ yarı:** Kırık beyaz bg + numaralı bölüm listesi (01, 02, 03...)
- **Bölüm başlıkları:** Bricolage SemiBold, koyu yeşil (#10332F)

### C) SECTION DIVIDER (Slide 3, 9, 12)

- **BG:** Koyu yeşil (#10332F)
- **Orta:** Bölüm başlığı (Bricolage ExtraBold, ~74pt, beyaz)
- **Üst/Alt:** Kısa coral (#FF7B52) yatay dash (rounded rect, ~60px genişlik)
- **Sol alt:** Watermark numara (Outfit SemiBold, ~400pt, yarı-saydam #254E49)

### D) VERİ + CHART (Slide 4, 5, 6)

```
┌─────────────────────────────────────────────────────────┐
│ [Breadcrumb | Alt Breadcrumb]  (coral, Bricolage Semi)  │
│                                                         │
│ [Insight Başlık Cümlesi — büyük, bold, renkli vurgulu]  │
│ (Bricolage ExtraBold ~52pt, siyah + kırmızı vurgu)     │
│                                                         │
│ ┌─────────────────────────────────────────────────┐     │
│ │ GROUPED BAR CHART                               │     │
│ │ • 12 ay × 2 seri ([Yıl-1] mavi, [Yıl] kırmızı)│     │
│ │ • Data labels her bar üstünde (33.1K format)    │     │
│ │ • Legend alt orta                                │     │
│ └─────────────────────────────────────────────────┘     │
│                                                         │
│ ┌─────────────────────────────────────────────────┐     │
│ │ TABLO: [brand] | Jan | Feb | ... | Dec          │     │
│ │        [Yıl-2] | 33.1K | 33.1K | ...           │     │
│ │        [Yıl-1] | 33.1K | 27.1K | ...           │     │
│ │        [Yıl]   | 33.1K | (boş) | ...           │     │
│ │ Header: Koyu gri, conditional color hücreler    │     │
│ └─────────────────────────────────────────────────┘     │
│                                                         │
│ ◯ [Source: Keyword Planner]                             │
└─────────────────────────────────────────────────────────┘
```

### E) VERİ + 4 TABLO + INSIGHT (Slide 7)

```
┌───────────────────────────────────────┬─────────────────┐
│ [Breadcrumb]                          │                 │
│ [Başlık]                              │                 │
│                                       │ - Insight 1     │
│ ┌──────────┐ ┌──────────┐            │ - Insight 2     │
│ │NonBrand  │ │NonBrand  │            │ - Insight 3     │
│ │YoY Tablo │ │MoM Tablo │            │ - Insight 4     │
│ ├──────────┤ ├──────────┤            │ - Insight 5     │
│ │Brand+Cat │ │Brand+Cat │            │ - Insight 6     │
│ │YoY Tablo │ │MoM Tablo │            │                 │
│ └──────────┘ └──────────┘            │                 │
│ ◯ [Source: Keyword Planner]           │                 │
└───────────────────────────────────────┴─────────────────┘
```

### F) TABLO + INSIGHT (Slide 8 — Rekabet)

```
┌─────────────────────────────────────────────────────────┐
│ [Breadcrumb] → [Başlık: [brand] & Rakip Arama Hacmi]   │
│                                                         │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Brand | Jan25 | Feb25 | ... | Dec25 | Jan26 | YoY|MoM│
│ │ trendyol | 11.1M | ... conditional color YoY/MoM   │ │
│ │ [brand]  | bold satır, vurgulanmış                  │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                         │
│ - Insight bullet 1 (bold keyword + renkli yüzde)        │
│ - Insight bullet 2                                      │
│ ◯ [Source: Keyword Planner]                             │
└─────────────────────────────────────────────────────────┘
```

### G) KPI KART + TABLO + INSIGHT (Slide 10, 13)

```
┌───────────────────────────────┬─────────────────────────┐
│ [Breadcrumb] → [Başlık]      │                         │
│                               │ Mini Tablo 1 (MoM)     │
│ ┌──────────────────────────┐  │ Mini Tablo 2 (YoY)     │
│ │ ANA TABLO (büyük)        │  │                         │
│ │ Kanallar × Metrikler     │  │ Insight paragraf        │
│ │ veya                     │  │ (renkli vurgulu metin)  │
│ │ 3 satır KPI kart paneli  │  │                         │
│ └──────────────────────────┘  │                         │
│                               │                         │
│ ALT YoY TABLO                 │                         │
│ ◯                             │                         │
└───────────────────────────────┴─────────────────────────┘
```

### H) 2 PIE CHART + INSIGHT (Slide 16)

```
┌─────────────────────────────────────────────────────────┐
│ [Breadcrumb] → [Başlık]                                 │
│                                                         │
│ ┌────────────────────┐  ┌────────────────────┐          │
│ │  🔍 Google SoC     │  │  🌐 AI Search SoV  │          │
│ │   [PIE CHART]      │  │   [PIE CHART]      │          │
│ │   Büyük dilim      │  │   Büyük dilim      │          │
│ │   etiketli         │  │   etiketli         │          │
│ └────────────────────┘  └────────────────────┘          │
│ Legend: Alt kısımda tüm siteler                         │
│                                                         │
│ - Insight bullet 1, 2, 3                                │
│ ◯ [Kaynak: SEOmonitor]                                  │
└─────────────────────────────────────────────────────────┘
```

### I) 2 TABLO YAN YANA (Slide 22, 23 — GSC Keyword/Sayfa Perf.)

```
┌─────────────────────────────────────────────────────────┐
│ [Breadcrumb] → [Başlık]                                 │
│                                                         │
│ ┌──── ✅ İyileşenler ─────┐ ┌──── ❌ Kötüleşenler ────┐│
│ │ Keyword/Page | Clicks   │ │ Keyword/Page | Clicks   ││
│ │             | Prev|Curr │ │             | Prev|Curr ││
│ │             | Chg% |... │ │             | Chg% |... ││
│ │ 10 satır               │ │ 10 satır               ││
│ └─────────────────────────┘ └─────────────────────────┘│
│                                                         │
│ - Insight                                               │
└─────────────────────────────────────────────────────────┘
```

### J) KAPANIŞ (Son Slide)

- Kapak'ın ayna görüntüsü
- Coral bg, daire motifi SAĞ tarafta
- "Teşekkürler" (Outfit ExtraLight, ~86pt, beyaz, ortalanmış)
- Üst/alt beyaz dash çizgiler

---

## 5. ORTAK ELEMANLAR

| Eleman | Konum | Format |
|--------|-------|--------|
| **Breadcrumb** | Sol üst | Bricolage SemiBold, 24-30pt, coral (#FF7B52) |
| **Inbound ikon** | Sol alt | Turuncu halka + beyaz iç daire |
| **Source badge** | Sol alt (ikon yanı) | Coral bg (#FF7B52), beyaz metin, Ubuntu 14pt, rounded rect |
| **Insight metin** | Sağ panel veya alt kısım | Bricolage Regular 21-29pt, dash bullet, bold keyword + renkli yüzde |
| **Daire motifi** | Kapak sol / Kapanış sağ / İçindekiler sol | Yarı-saydam, bg renginin açık tonu |

---

## 6. STANDART SLIDE AKIŞI (23 Slide)

```
 1  KAPAK — [brand] | Aylık SEO Değerlendirme — [Ay] [Yıl]
 2  İÇİNDEKİLER — Sunum Akışı
 3  SECTION DIVIDER — 01 Arama Hacmi & Rekabet
 4  VERİ — Brand Arama Hacmi (Grouped Bar + Tablo)
 5  VERİ — Brand + Category Hacmi (Grouped Bar + Tablo)
 6  VERİ — Non-Branded Hacim (Grouped Bar + Tablo)
 7  VERİ — Segment Kırılım (4 Tablo + Insight)
 8  VERİ — Rekabet Tablosu (Büyük Tablo + Insight)
 9  SECTION DIVIDER — 02 GA4 Trafik
10  VERİ — GA4 Kanal Performansı (Tablo + Mini Tablo + Insight)
11  VERİ — GA4 Organik Detay (Combo Chart + Mini Tablo + Insight)
12  SECTION DIVIDER — 03 GSC Trafik, Sıralama, Visibility
13  VERİ — GSC Brand & Non-Brand (KPI Kart + Tablo + Insight)
14  VERİ — GSC Impression/Click Trend (2 Grouped Bar + Insight)
15  VERİ — Competitor Visibility (Tablo/Liste + Insight)
16  VERİ — Share of Clicks & AI SoV (2 Pie Chart + Insight)
17  VERİ — Kategori Bazında Visibility (Liste + Insight)
18  VERİ — Visibility Top Keywords (Screenshot/Tablo)
19  VERİ — Top & Rising Keywords (2 Panel Tablo)
20  VERİ — AI Referral Trafik (Tablo + Insight)
21  VERİ — GSC Keyword Performansı (2 Tablo: İyileşen/Kötüleşen)
22  VERİ — GSC Sayfa Performansı (2 Tablo: İyileşen/Kötüleşen)
23  KAPANIŞ — Teşekkürler
```

---

## 7. MARKA PROFİLİ CONFIG YAPISI

Her marka için `config/[brand]_profile.json`:

```json
{
  "brand": "VitrA",
  "domain": "vitra.com.tr",
  "brand_regex": "vitra|vitrA|VitrA",
  "ga4_property_id": "properties/XXXXXXXXX",
  "seomonitor_campaign_id": "XXXXX",
  "keywords": {
    "brand": ["vitra"],
    "brand_category": ["vitra banyo", "vitra klozet", ...],
    "nonbrand": ["klozet", "lavabo", "banyo dolabı", ...]
  },
  "segments": {
    "Armatürler": {"nonbrand": [...], "brand_cat": [...]},
    "Banyo Mobilyaları": {"nonbrand": [...], "brand_cat": [...]},
    ...
  },
  "competitors": ["trendyol", "hepsiburada", "ikea", "koçtaş", ...],
  "competitor_domains": ["trendyol.com", "hepsiburada.com", ...]
}
```
