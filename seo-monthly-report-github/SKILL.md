---
name: seo-monthly-report
description: >
  Generate a complete monthly SEO performance presentation (PPTX) for any brand in the Inbound portfolio.
  Use this skill whenever the user wants to create, generate, or prepare a monthly SEO report, SEO
  presentation, aylık SEO sunumu, SEO değerlendirme sunumu, or any variant of "prepare the monthly
  SEO deck for [brand]". Also trigger when the user says things like "[brand] için [ay] sunumu hazırla",
  "SEO raporu oluştur", "aylık sunum", "monthly SEO deck", or references preparing slide decks with
  SEO metrics (search volume, GSC, GA4, visibility, rankings). This skill handles the full pipeline:
  data collection via Google APIs (GA4 Data API, Search Console API, Google Ads API) and
  SEOmonitor API through WHC platform, data processing, chart generation, insight writing,
  and PPTX creation. Works for all Inbound client brands.
---

# SEO Monthly Report Generator

End-to-end monthly SEO performance presentation builder for Inbound client brands.

## Before You Start

**Read these reference files in order:**

1. **Always read first:** `references/slide-mapping.md` — Slide-by-slide specification: data requirements, table structures, chart specs, insight templates
2. **Always read second:** `references/design-system.md` — Colors, fonts, layout rules, slide type templates
3. **Read when building charts:** `references/chart-specs.md` — Detailed chart rendering specs (matplotlib/plotly code patterns)
4. **Read the PPTX skill:** `/mnt/skills/public/pptx/SKILL.md` — For PPTX creation mechanics (pptxgenjs or python-pptx)

---

## Minimum Required Input

The user provides:

| Parameter | Required | Example | Notes |
|-----------|----------|---------|-------|
| **Brand name** | ✅ Yes | "VitrA" | Must match a known brand profile or be configured |
| **Report month & year** | ✅ Yes | "Şubat 2026" | Turkish or English accepted |

Everything else is derived automatically.

### Derived Parameters (auto-calculated)

```
[Ay]    = Report month (Turkish: Ocak, Şubat, Mart...)
[Yıl]   = Report year (2026)
[Yıl-1] = Previous year (2025)
[Yıl-2] = Two years ago (2024)
[Ay-1]  = Previous month (Turkish: Ocak → Aralık with year rollback)
[month] = English month name for API queries (January, February...)
```

---

## Step 0: Configuration & Keyword Setup

### 0a. Load Brand Profile

Check if a brand profile exists in `references/brand-profiles/[brand].json`. If it exists, load it silently. If not, ask the user for:

- **Domain** (e.g., vitra.com.tr)
- **GA4 Property ID**
- **SEOmonitor Campaign ID**
- **Brand keyword regex** for GSC filtering (e.g., `vitra|vitrA|VitrA`)
- **Competitor list** (brand names for Slide 8)

### 0b. Keyword Data Setup

**This is the first thing to clarify with the user.** Keyword data is needed for Slides 4–7. Ask upfront:

> "Keyword verileri için hangi kaynağı kullanayım?"
> 1. **SEOmonitor gruplarından çek** — Brand+Category ve Non-Brand keyword grupları SEOmonitor kampanyasında tanımlı
> 2. **CSV dosyası yükle** — Son 24 ay hacimli keyword listesi (brand, brand+cat, non-brand, segment grupları)
> 3. **WHC platform keyword verisi** — WHC'de marka ile ilişkili keyword bilgisi

**If CSV is chosen**, expected format:
```
keyword | segment | type | jan_2024 | feb_2024 | ... | [current_month]_[current_year]
klozet  | Vitrifiyeler | nonbrand | 74000 | 60500 | ...
vitra klozet | Vitrifiyeler | brand_cat | 6600 | 5400 | ...
```

Columns: `keyword`, `segment` (kategori grubu), `type` (brand / brand_cat / nonbrand), then monthly volume columns.

**If SEOmonitor is chosen**, pull keyword groups from the campaign and map segments automatically.

**If mixed or unclear**, ask: "Segment kırılımları (Armatürler, Vitrifiyeler vb.) CSV'den mi gelecek yoksa SEOmonitor gruplarından mı çekeyim?"

### 0c. Competitor List

For Slide 8, a competitor brand list is needed. Check in this order:
1. Brand profile'da tanımlı mı?
2. SEOmonitor kampanyasında rakip tanımlı mı?
3. Yoksa kullanıcıya sor: "Rekabet slide'ı için takip ettiğiniz rakip marka listesini paylaşır mısınız?"

---

## Step 1: Data Collection

Once configuration is complete, collect all data. Make API calls in batch — do not narrate each call individually.

### Data Collection Matrix

| Slide(s) | Data Source | API | Key Parameters |
|----------|------------|-----|----------------|
| 4, 5, 6 | Keyword search volumes | Google Ads API (Keyword Planner) | keyword list, location: Turkey (2792), 36 months |
| 7 | Segment keyword volumes | Google Ads API (Keyword Planner) | keyword groups × segments, 2 months + YoY |
| 8 | Competitor brand volumes | Google Ads API (Keyword Planner) | competitor keywords, 13 months |
| 10, 11 | GA4 traffic & organic | GA4 Data API (runReport) | channel group metrics, 13 months |
| 13 | GSC brand vs non-brand | Search Console API (searchAnalytics.query) | query dimension, brand regex filter, 3 periods |
| 14 | GSC impression/click trend | Search Console API | monthly aggregate, 36 months |
| 15 | Competitor visibility | SEOmonitor API | campaign visibility, competitors |
| 16 | Share of Clicks & AI SoV | SEOmonitor API | SoC + AI SoV data |
| 17 | Category visibility | SEOmonitor API | keyword group visibility |
| 18-19 | Top keywords & rankings | SEOmonitor API | keyword rankings sorted by impact/volume |
| 20 | AI referral traffic | GA4 Data API | source filter: chatgpt, gemini, perplexity... |
| 21 | GSC keyword performance | Search Console API | query dimension, YoY click change |
| 22 | GSC page performance | Search Console API | page dimension, YoY click change |

### API Authentication

Data is pulled from WHC platform where these APIs are connected. Tool calls can also be made directly via Google APIs when needed:

- **Google Ads API:** `google.ads.googleads` — Keyword Planner `generateHistoricalMetrics`
- **GA4 Data API:** `google.analytics.data.v1beta` — `runReport` method
- **Search Console API:** `webmasters.v3` — `searchAnalytics.query` method
- **SEOmonitor API:** REST API — WHC üzerinden bağlı

**Error handling:**
- If an API call fails, retry once. If still fails, log which slide is affected and continue with others.
- At the end, report which slides could not be generated due to missing data.
- Never skip a slide silently — always inform the user.

---

## Step 2: Data Processing

For each slide, process raw data into presentation-ready format. See `references/slide-mapping.md` for per-slide processing details.

### Universal Calculations

```
YoY % = (current_period - same_period_last_year) / same_period_last_year × 100
MoM % = (current_month - previous_month) / previous_month × 100
```

### Conditional Formatting Rules

For all tables with change percentages:
- **Positive values**: Green gradient background
  - Strong positive (>20%): `#57BB8A`
  - Medium positive (5-20%): `#9DD8BB`
  - Light positive (0-5%): `#C5E8D7`
- **Negative values**: Red gradient background
  - Strong negative (<-20%): `#E67C73`
  - Medium negative (-20% to -5%): `#EA948D`
  - Light negative (-5% to 0%): `#F2BCB7`
- **Zero/neutral**: White background

### Number Formatting

| Value Range | Format | Example |
|-------------|--------|---------|
| ≥ 1,000,000 | X.XM | 2.4M |
| ≥ 1,000 | X.XK or XX.XK | 33.1K, 823.0K |
| < 1,000 | Raw number | 587 |
| Percentages | %X or %X.X | %18, %3.5 |
| Currency (TRY) | ₺X.XXK or ₺X.XXM | ₺565.08K |

---

## Step 3: Chart Generation

Generate charts as PNG images using Python (matplotlib or plotly). See `references/chart-specs.md` for exact specifications per slide.

### Chart Types Used

| Slide | Chart Type | Key Config |
|-------|-----------|------------|
| 4, 5, 6 | Grouped Bar (2 series) | [Yıl-1] blue vs [Yıl] red, data labels on top |
| 11 | Combo: Stacked Bar + Line | Revenue grey bar + Sessions yellow bar + Transactions red line |
| 14 | Grouped Bar (3 series) | [Yıl-2] light grey, [Yıl-1] dark grey, [Yıl] orange |
| 16 | 2× Pie Chart | Google SoC + AI Search SoV side by side |

**Chart style rules:**
- White background, no border
- Font: Calibri or Arial
- Data labels above bars in K/M format
- Legend at bottom center
- Grid lines: light grey, horizontal only
- No chart title in the image (title is placed on slide separately)

---

## Step 4: Insight Generation

For each data slide, generate insight text using the templates in `references/slide-mapping.md`.

### Insight Writing Rules

1. **Language:** Turkish (formal, advisory tone — not imperative)
2. **Structure:** Short paragraphs or dash-bullet lists
3. **Highlighting rules in text:**
   - Positive percentages → green color (#27AE60)
   - Negative percentages → red color (#E74C3C)
   - Key metric names → bold
   - Brand/competitor names → bold
4. **Tone:** Factual and analytical. State what happened, not what should be done.
5. **Length:** 2-5 bullets per slide, each 1-2 sentences max.

### Dynamic Variables in Insight Templates

Templates use `{variable}` placeholders. Replace with calculated values:
```
{YoY_pct} → "%7.7"
{MoM_pct} → "%18.2"
{YoY_yön} → "azalırken" (if negative) / "artarken" (if positive)
{MoM_yön} → "azalmıştır" / "artmıştır" / "sabit kalmıştır"
```

Direction words logic:
- `{X_yön}` for mid-sentence: >0 → "artarken", <0 → "azalırken", =0 → "sabit kalırken"
- `{X_durum}` for end of sentence: >0 → "artmıştır", <0 → "azalmıştır", =0 → "sabit kalmıştır"

---

## Step 5: PPTX Generation

Read `/mnt/skills/public/pptx/SKILL.md` before starting PPTX creation.

### Slide Dimensions
- Width: 26.67 inches (custom wide) or standard `LAYOUT_WIDE` (13.33" × 7.5")
- If using pptxgenjs with custom size: `pres.defineLayout({ name:'CUSTOM', width: 26.67, height: 15.0 })`

### Slide Build Order (23 slides)

Build each slide according to its type (see `references/design-system.md`):

```
Slide  1: KAPAK          → coral bg, brand name, month/year, Inbound logo
Slide  2: İÇİNDEKİLER    → split layout, section list
Slide  3: SECTION DIVIDER → "01 - Arama Hacmi & Rekabet"
Slide  4: DATA            → Brand search volume (bar chart + table)
Slide  5: DATA            → Brand+Category volume (bar chart + table)
Slide  6: DATA            → Non-branded volume (bar chart + table)
Slide  7: DATA            → Segment breakdown (4 tables + insight panel)
Slide  8: DATA            → Competitor table + insight
Slide  9: SECTION DIVIDER → "02 - GA4 Trafik"
Slide 10: DATA            → GA4 channel performance (table + mini tables + insight)
Slide 11: DATA            → GA4 organic detail (combo chart + mini tables + insight)
Slide 12: SECTION DIVIDER → "03 - GSC Trafik, Sıralama, Visibility"
Slide 13: DATA            → GSC brand vs non-brand (KPI cards + YoY table + insight)
Slide 14: DATA            → GSC impression/click trend (2 bar charts + insight)
Slide 15: DATA            → Competitor visibility (list/table + insight)
Slide 16: DATA            → Share of Clicks & AI SoV (2 pie charts + insight)
Slide 17: DATA            → Category visibility changes (list + insight)
Slide 18: DATA            → Top impact keywords (table/screenshot)
Slide 19: DATA            → Top & rising keywords (2 panel table)
Slide 20: DATA            → AI referral traffic (table + insight)
Slide 21: DATA            → GSC keyword gainers/losers (2 tables)
Slide 22: DATA            → GSC page gainers/losers (2 tables)
Slide 23: KAPANIŞ         → coral bg, "Teşekkürler"
```

### Common Slide Elements (apply to every data slide)

1. **Background:** Warm white (#FAF8F5)
2. **Breadcrumb:** Top-left, coral (#FF7B52), Bricolage Grotesque SemiBold ~24pt
3. **Inbound icon:** Bottom-left, orange ring + white inner circle
4. **Source badge:** Bottom-left next to icon, coral bg rounded rect, white Ubuntu 14pt text

---

## Step 6: QA

After generating the PPTX:

1. Convert to images: `python scripts/office/soffice.py --headless --convert-to pdf output.pptx && pdftoppm -jpeg -r 150 output.pdf slide`
2. Visually inspect each slide thumbnail
3. Check: text overflow, missing data, chart alignment, table formatting
4. Fix issues and re-verify

---

## Output

Deliver:
1. **PPTX file:** `[brand]_SEO_Degerlendirme_[Ay]_[Yıl].pptx`
2. **Brief summary:** Which slides were generated successfully, any slides skipped due to missing data
3. **Data notes:** Any anomalies in the data (e.g., API returning empty results for certain periods)

---

## Error Recovery & Partial Generation

If some API data is unavailable:
- Generate all slides that have data
- For slides with missing data, create a placeholder slide with the correct layout but a note: "Bu slide için veri çekilemedi: [reason]"
- Report to user which slides need manual data entry

---

## Quick Reference: File Locations

```
references/slide-mapping.md     → Per-slide data specs, tables, charts, insights
references/design-system.md     → Colors, fonts, layouts, slide types
references/chart-specs.md       → Chart rendering code patterns
references/brand-profiles/      → Brand config JSONs (keyword lists, competitors, IDs)
```
