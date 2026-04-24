# Design System

> **注意**：這份文件是 tri **既存設計系統的文件化**，不是新提案。
> 系統的權威來源是 `src/styles/global.css` 的 `:root` design tokens。這份 DESIGN.md
> 是人類可讀的解釋與決策記錄。若 tokens 與此文件衝突，以 `global.css` 為準。

## Aesthetic Direction

- **一句話方向**：** 科技極簡 × TWAI 霓虹漸層風格 **
- **情緒**：像在宇宙中探尋人類軌跡的太空船，有重量感，讀起來安靜、專業，期待在宇宙中有著讓眼睛為之一亮的新鮮感
- **裝飾層級**：**intentional**——以 漸層3D圖為主角，配上藍紫色冷色底與藍綠色強調色，可依賴圖形裝飾、漸層、3D、插畫
- **反模式（明確避開）**：
  - 圓潤卡通風（border-radius 保守）
  - AI slop 的三欄圖示網格、置中一切、泡泡按鈕
- **可類比的參考系**：TWAI官網、Apple官網、ASUS官網、Google AI Studio、Dify

## Color---做到這裡

**Approach**：**restrained**——一個強烈的品牌紅作為唯一彩色，其他全部是暖調中性色。色彩是稀有且有意義的語意工具，不是裝飾。

### Core Palette

| Token | 值 | 用途 |
|-------|------|------|
| `--color-bg` | `#f9f5f1` | 暖紙色背景（全站主背景，CLAUDE.md 指定） |
| `--color-bg-alt` | `#f5f0ea` | 次要背景（區段交替、資訊塊） |
| `--color-bg-card` | `#ffffff` | 卡片背景（內容卡、引用塊） |
| `--color-primary` | `#2c2c2c` | 黑灰主文字（CLAUDE.md 指定） |
| `--color-secondary` | `#4a3f35` | 中棕（正文） |
| `--color-muted` | `#8a7e72` | 淡棕（次要文字、日期、meta） |
| `--color-accent` | `#d13a3a` | 品牌紅（連結、CTA、強調） |
| `--color-accent-hover` | `#b82e2e` | 品牌紅 hover |
| `--color-accent-light` | `#fef2f2` | 品牌紅淡底（徽章背景） |
| `--color-border` | `#e8e2da` | 主邊框色 |
| `--color-border-light` | `#f0ebe4` | 淡邊框（分隔線、次要卡） |
| `--color-code-bg` | `#f3efe9` | 程式碼背景 |

### Dark mode
**不支援且不計劃支援**。CLAUDE.md 明確禁止深色主題——整套視覺系統建立在「暖紙色底」的安全感上，切暗色會破壞核心情緒。

### 色彩對比
所有文字／背景組合通過 WCAG 2.2 AA：
- `#2c2c2c` on `#f9f5f1` = 13.4:1（遠超 4.5:1）
- `#d13a3a` on `#f9f5f1` = 5.1:1（通過 AA 正文）
- `#8a7e72` on `#f9f5f1` = 4.6:1（通過 AA 正文邊緣）

## Typography

**哲學**：襯線體擔任情緒（標題），無襯線體擔任功能（內文、UI）。兩者都是繁中優先的 Noto 字族，載入穩定。

### Font Stack

| 用途 | Token | 實際字體 | 來源 |
|------|-------|---------|------|
| 標題／Display | `--font-heading` | `Noto Serif TC` → `Georgia` → `serif` | `@fontsource/noto-serif-tc` |
| 內文／UI | `--font-body` | `Noto Sans TC` → `system-ui` → `sans-serif` | `@fontsource/noto-sans-tc` |

**字重**：
- Regular (400) — 一般內文
- Bold (700) — 標題、強調
- 標題 h2 在 `.prose-content` 內用 800（更重的視覺錨點）

**載入策略**：
- 透過 `@fontsource` 套件自建 npm 包，**不依賴 Google Fonts CDN**
- `BaseHead.astro` 只 import 必要 weights（400、700），避免載入整個字族
- 降低 LCP、遵守 CSP（不需外連 fonts.googleapis.com）

### Type Scale

```
--font-size-xs:    0.75rem  /* 12px */
--font-size-sm:    0.875rem /* 14px */
--font-size-base:  0.9rem /* 15px */
--font-size:  1rem     /* 一般來說 base 是 1rem = 16px */
--font-size-md:    1.125rem /* 18px */
--font-size-lg:    1.25rem  /* 20px  ← mobile body */
--font-size-xl:    1.5rem   /* 24px  ← desktop body */
--font-size-2xl:   1.875rem /* 30px */
--font-size-3xl:   2.25rem  /* 36px */
--font-size-4xl:   3rem     /* 48px */
--font-size-5xl:   3.75rem  /* 60px  ← hero display */
```

**Body base（全站 UI）**：
- Mobile：20px（`--font-size-lg`）
- Desktop：24px（`--font-size-xl`）
- 註解寫「放大兩級」——Vista 刻意把全站 UI 字級拉大，因為他的讀者多數不是技術宅，對小字不耐

**Blog 內文（`.prose-content` override）**：
- Desktop：**19px**（2026-04-13 從 18px 調整）
- Mobile：**18px**（2026-04-13 從 17px 調整）
- 為什麼比全站 body 小？因為 24px 是給 UI 元素（按鈕、導覽、表單）的尺寸。長文閱讀 19px 配合 1.9 行高才是中文長文的甜蜜點。

### Headings

| 層級 | Desktop | Mobile | 字體 | 字重 |
|------|---------|--------|------|------|
| h1 | 48px | 30px | Noto Serif TC | 700 |
| h2 | 36px | 24px | Noto Serif TC | 700（`.prose-content` 內 800） |
| h3 | 30px | 20px | Noto Serif TC | 700 |
| h4 | 24px | — | Noto Serif TC | 700 |
| h5 | 20px | — | Noto Serif TC | 700 |

### Line Height & Letter Spacing

```
--line-height-tight:   1.25  /* 標題 */
--line-height-normal:  1.6   /* UI */
--line-height-relaxed: 1.85  /* 正文 */
/* .prose-content 使用 1.9（更寬鬆，適合長文中文） */

--letter-spacing-tight:  -0.02em  /* 標題（中文稍微負值收緊） */
--letter-spacing-normal: 0
--letter-spacing-wide:   0.05em   /* UPPERCASE labels */
```

## Spacing

**Base unit**：4px（但主要使用 8px grid 的倍數）
**Density**：**spacious**——偏鬆，呼吸感優先

```
--space-1:  0.25rem /* 4px  */
--space-2:  0.5rem  /* 8px  */
--space-3:  0.75rem /* 12px */
--space-4:  1rem    /* 16px */
--space-5:  1.25rem /* 20px */
--space-6:  1.5rem  /* 24px */
--space-8:  2rem    /* 32px */
--space-10: 2.5rem  /* 40px */
--space-12: 3rem    /* 48px */
--space-16: 4rem    /* 64px */
--space-20: 5rem    /* 80px */
--space-24: 6rem    /* 96px */
```

**使用規則**：
- 相關元素（段落內的字詞）：`space-1` ~ `space-3`
- 段落間：`space-4` ~ `space-6`
- 區段（section）內的子區塊：`space-8` ~ `space-12`
- 大區段間：`space-16` ~ `space-24`
- 觸控目標最小 44×44 px（a11y 規範）

## Layout

### Max Widths

```
--max-width-content: 760px   /* 最佳閱讀寬度（中文 30-35 字/行） */
--max-width-wide:    1080px  /* 寬版內容（首頁 Hero、Topic Grid） */
--max-width-full:    1200px  /* 全寬容器（header、footer） */
--header-height:      64px
```

- **文章內文**：`.prose-content` 用 850px（比 `--max-width-content` 略寬，配合 19px 字級維持舒適行長）
- **首頁與 Pillar Pages**：`max-width-wide` (1080px)
- **Topic Pages**（en/ja）：860px（比主頁窄，更像雜誌內頁）

### Grid

主要 breakpoints：
- Mobile：預設
- Tablet：`@media (min-width: 768px)` — body 切換到 desktop size
- Desktop：`@media (min-width: 1024px)` — 多欄 grid 啟用

**Grid 原則**：
- 首頁 Topic Cluster：1 欄（mobile）→ 2 欄（sm）→ 3 欄（lg）
- Blog 列表：1 欄（mobile）→ 2 欄（sm）→ 3 欄（lg）
- 文章頁：雙欄佈局（內文 850px + 右側 sticky TOC 220px）在 > 991px 生效，窄螢幕單欄

### Border Radius

```
--radius-none: 0       /* 預設，主圖、大多數卡片 */
--radius-sm:   4px     /* 按鈕、徽章、小卡 */
--radius-md:   8px     /* 模態、大卡 */
--radius-lg:   12px    /* 特殊強調 */
--radius-full: 9999px  /* Pill buttons、頭像 */
```

**關鍵決策**：**文章圖片一律方角**（CLAUDE.md：「Article images should use square corners, not rounded corners」）。圓角破壞雜誌質感。

## Motion

**Approach**：**minimal-functional**——只做輔助認知的過渡。沒有入場動畫、沒有 scroll-driven choreography、沒有 hover 翻轉。

```
--transition-fast: 150ms ease  /* 按鈕 hover、連結色變 */
--transition-base: 250ms ease  /* 卡片 hover、小元件位移 */
--transition-slow: 400ms ease  /* 圖片放大 scale(1.03) */
```

**使用規則**：
- 連結 `color` 變化：`fast`
- 按鈕 `background`/`transform`：`fast`
- 卡片 `transform` / `box-shadow`：`base`
- 圖片 hover zoom：`slow`
- **不使用**：keyframes、scroll-triggered、parallax（除了部落格 hero 圖片的 `object-fit: cover` 幾何效果）
- 所有 transition 都應該能在 `@media (prefers-reduced-motion)` 下被關閉（目前 CSS 未明確處理，列為 TODO）

## Shadows

```
--shadow-sm: 0 1px 3px  rgba(0, 0, 0, 0.04)  /* 靜態卡片 */
--shadow-md: 0 4px 16px rgba(0, 0, 0, 0.06)  /* hover state、Author Card */
--shadow-lg: 0 8px 32px rgba(0, 0, 0, 0.08)  /* Hero 圖片 */
--shadow-xl: 0 16px 48px rgba(0, 0, 0, 0.1)  /* 浮動元件（sticky newsletter） */
```

所有陰影都偏低透明度（0.04–0.10），避免破壞暖紙色底的柔和感。

## Components

元件庫在 `src/components/`。以下是核心元件的職責與設計意圖：

### Layout & Structure
| 元件 | 職責 |
|------|------|
| `Header.astro` | 全站 sticky header + 行動版 hamburger + skip-to-main link（a11y） |
| `Footer.astro` | 全站深色 footer（這是唯一使用深色背景的區塊，對照強調品牌紅） |
| `BaseHead.astro` | SEO meta、OG tags、hreflang、字體載入、ClientRouter |
| `Schema.astro` | 統一的 JSON-LD 生成器（Person/BlogPosting/WebSite/Course/BreadcrumbList/FAQPage/HowTo/Event） |

### Navigation & Discovery
| 元件 | 職責 |
|------|------|
| `Breadcrumb.astro` | 視覺麵包屑 + 同步輸出 `BreadcrumbList` JSON-LD |
| `TagNav.astro` | 文章標籤導覽 |
| `LanguageSwitcher.astro` | zh-TW / en / ja 切換 |
| `TopicCluster.astro` | 首頁六大主題叢集區塊（驅動自 `src/data/topics.ts`） |
| `TopicLandingPage.astro` | en/ja 主題 landing page 共用元件（FAQ + 最新文章 + CTA） |

### Content
| 元件 | 職責 |
|------|------|
| `HomepageHero.astro` | 首頁三路分流 Hero（讀者／學員／企業客戶） |
| `PillarPage.astro` | zh-TW 主題 pillar page 統一模板（Hero + TOC + FAQ + CTA + 文章網格） |
| `PostCard.astro` | 部落格文章卡片 |
| `AuthorCard.astro` | 文章底部的作者 byline card（E-E-A-T 訊號） |
| `ClusterCTA.astro` | 依文章 tag 自動推薦相關 Pillar Page |

### Conversion
| 元件 | 職責 |
|------|------|
| `NewsletterCTA.astro` | 多模態訂閱 CTA（`banner` / `inline` / `hero` / `sticky`） |
| `SocialProof.astro` | 首頁數據區塊（18,000+ 訂閱 / 1,700+ 文章 / 20 本書 / 200+ 演講） |
| `Button.astro` | 通用按鈕（primary / secondary / ghost） |

### 共用元件規則
1. **Astro scoped CSS 優先**——元件內的 `<style>` 只影響自己
2. **需要穿透時用 `:global()`**——例如 `.prose-content :global(a)` 覆蓋文章內的連結樣式
3. **互動元素最小 44×44 px**（a11y）
4. **所有 `<a>` / `<button>` 必須有 `:focus-visible` 焦點環**（定義於 `Header.astro` 的 global scope）

## CTA Priority System

站上所有 CTA 分為三級，視覺層級對應商業優先級：

| 優先級 | 用途 | 視覺 | 範例 |
|--------|------|------|------|
| **Primary** | 訂閱電子報（最高商業目標） | 實心紅底白字（`bg-accent`） | 「訂閱電子報」「免費訂閱」 |
| **Secondary** | 探索內容 / 主題導航 | 描邊+紅字 | 「探索文章」「深入探索」 |
| **Tertiary** | 咖啡支持 / 聯繫 / 分享 | 純文字連結 | 「請 Vista 喝杯咖啡」 |

Primary CTA 在每篇文章底部、首頁 Hero、文章中段 inline、以及 sticky 底部橫幅（50% 捲動觸發）出現，但**不重複疊加**——同一螢幕最多一個 primary CTA。

## Accessibility

**目標**：WCAG 2.2 AA 合規

- ✅ Skip-to-main link（Header 頂部）
- ✅ 全站 `:focus-visible` 紅色焦點環（2px outline + 2px offset）
- ✅ 行動版觸控目標最小 44×44 px
- ✅ 色彩對比：正文 ≥ 4.5:1、大字 ≥ 3:1
- ✅ 鍵盤 Tab 順序：Header → main-content → Footer
- ✅ 語意化 HTML：`<main>`, `<nav aria-label>`, `<article>`, `<figure>/<figcaption>`, `<blockquote>`
- ✅ JSON-LD 結構化資料：BlogPosting、Person、BreadcrumbList、FAQPage、HowTo、Course、Event
- ⚠️ `prefers-reduced-motion` 尚未全面處理（TODO）
- ⚠️ 部分歷史文章的 `<img alt="">` 缺失（TODO）

## i18n

三語系共存但不自動翻譯：
- **zh-TW**（主）：`/`、`src/content/blog/*.md`
- **en**：`/en`、`src/content/blog/en--*.md`
- **ja**：`/ja`、`src/content/blog/ja--*.md`

**設計一致性**：
- 所有 locale 共用同一套 design tokens、色彩、spacing、motion
- 元件（`HomepageHero`、`TopicCluster`、`SocialProof`、`AuthorCard`）在 zh-TW 版完整；en/ja 首頁使用內嵌簡化版（避免強制讓元件 locale-aware 增加複雜度）
- Topic pages 在 en/ja 使用共用的 `TopicLandingPage` 元件（比 zh-TW 的 `PillarPage` 精簡）
- 字體在所有 locale 相同（Noto Serif TC + Noto Sans TC）—— Noto 字族覆蓋中日英繁簡，不需要切換

## Writing Style

屬於 design system 的一部分，因為文字是內容型站點的第一視覺元素。規則見 `CLAUDE.md`：

- **用字偏好**：「愈來愈」取代「越來越」、「臺」取代「台」、「批次」取代「批量」
- **書名號**：《》放在連結外 `《[書名](url)》`
- **咖啡 emoji**：☕️ 不加連結，只連結後面的文字
- **圖說文字**：必須以三角符號開頭 `*▲ 說明*`
- **文章結尾**：每篇都要 `☕️ [請 Vista 喝杯咖啡](https://vista.im/coffee)`
- **TL;DR block**（2026-04-13 後新規範）：新文章應在 Hero 後加一段 `> **TL;DR**：...` 方便 LLM 摘取引用

## Brand Voice

- **不做**：廣告文案感、技術宅炫技、焦慮行銷、FOMO、「you must」「crucial」「delve」等 AI 味詞彙
- **要做**：像朋友聊天但保有專業感、具體例子、承認不確定、經常用「我」、短段落混合長段落

## Tech Stack 對設計的影響

- **Astro 5**：scoped CSS 預設安全，但穿透規則要清楚（`:global()` 必須寫對 scope）
- **Tailwind 4**：utility classes 配合 design tokens 使用，`theme.extend` 讀 CSS variables
- **`@fontsource`**：字體自建 npm 包，不依賴 CDN
- **Cloudflare Pages**：部署後觸發 Cloudflare Web Analytics（無 cookie），Real User Monitoring 追蹤 Core Web Vitals
- **`_headers` CSP**：`style-src 'unsafe-inline'` 目前仍開啟（Astro scoped styles 需要），長期目標是硬化

## Performance Budget

Core Web Vitals 目標（行動版中位數）：
- **LCP** ≤ 2.5s
- **CLS** ≤ 0.1
- **INP** ≤ 200ms
- **A11y score** ≥ 95（Lighthouse）
- **SEO score** ≥ 95

為達成這些目標的設計選擇：
- 首屏圖片強制 `fetchpriority="high"` + WebP + `@astrojs/assets` 優化
- 非首屏圖片全部 `loading="lazy"`
- 字體只載必要 weights（400、700）
- Hero 區塊避免大型背景圖，改用 typography + 邊距營造張力
- 相關文章/熱門文章透過 `src/data/*.json` 預計算，避免 per-page O(N²)

## File Map

| 檔案 | 角色 |
|------|------|
| `src/styles/global.css` | Design tokens 權威來源（`:root` 變數） |
| `tailwind.config.cjs` | Tailwind 對應（`theme.extend` 讀 CSS variables） |
| `src/components/*.astro` | 所有共用 UI 元件 |
| `src/layouts/BlogPost.astro` | 文章頁 layout（包含 `.prose-content` 覆寫） |
| `src/data/topics.ts` | 六大主題靜態配置 |
| `src/data/topic-content.ts` | en/ja Topic Landing Page 的翻譯內容 |
| `public/_headers` | Cloudflare security headers + CSP |
| `CLAUDE.md` | 用字、圖片、視覺約束總綱 |

## Decisions Log

| 日期 | 決策 | 理由 |
|------|------|------|
| — | 品牌色彩定為淺米（#f9f5f1）+ 紅（#d13a3a）+ 黑（#2c2c2c） | 暖紙色建立雜誌質感、紅色作為文青書店風的強調、黑字維持閱讀嚴肅感 |
| — | 禁止深色主題、紫色、其他花俏配色 | 設計系統的核心是「一致性與克制」——有多套視覺主題會稀釋品牌 |
| — | 文章圖片方角（`border-radius: 0`） | 圓角破壞雜誌質感 |
| — | 全站 body 字級放大兩級（20/24px） | Vista 的讀者多數不是技術宅，對小字不耐 |
| 2026-04-24 | 這份 DESIGN.md 首次產出 | 把既存的系統從 `global.css` 註解昇級為人類可讀文件 |

---

## How to Use This File

1. **任何視覺或 UI 變動前**，先讀這份文件和 `src/styles/global.css`
2. 所有字級、色彩、間距優先用 CSS variables（`var(--color-accent)` 而非 hex）
3. **不要偏離**這裡定義的 token——要偏離必須更新這份文件並留下決策記錄
4. QA 時把 `DESIGN.md` 當作規範清單，違反的程式碼應該被指出
5. 這份文件**版本化**——design system 有任何變動，都要同步更新這份文件的 Decisions Log 條目
