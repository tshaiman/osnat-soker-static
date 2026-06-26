# Wix → Custom Site Migration Plan
## osnat-soker.com → Astro + Tailwind + Cloudflare Pages

**Original site:** https://www.osnat-soker.com/  
**Type:** Solo psychotherapy practice — Hebrew (RTL), static, informational  
**Target:** Fully custom Astro site, hosted on Cloudflare Pages (free tier)  
**Owner:** Osnat Soker (אסנת סוקר), psychotherapist, Modi'in, Israel  

---

## Prerequisites (do once before starting)

```bash
# 1. Install the frontend-design plugin inside Claude Code
/plugin install frontend-design@claude-plugins-official

# 2. Verify Node.js >= 18
node --version

# 3. Work inside this folder
cd ~/src/wix-convert
```

---

## Architecture: Single-Page with Anchor Navigation

**IMPORTANT:** The original Wix site is a single-page application. All content lives on
one scrollable page. The nav links (בית | אודות | פסיכותרפיה | יצירת קשר) are anchor
links that scroll to sections on the same page — they do NOT navigate to separate URLs.

The new Astro site should mirror this: one `index.astro` page with all sections, and nav
links that scroll to `#about`, `#psychotherapy`, `#supervision`, `#contact` anchors.
This is simpler, loads faster, and matches the original UX exactly.

## Project Structure to Generate

```
wix-convert/
├── PLAN.md                  ← this file
├── SCREENSHOTS/             ← full-page screenshot of the site (single page)
│   ├── fullpage-desktop.png ← full-page screenshot at desktop width
│   └── fullpage-mobile.png  ← full-page screenshot at mobile width (optional)
├── public/
│   ├── images/
│   │   ├── osnat-portrait.jpg      ← download from Wix CDN (URL below)
│   │   ├── osnat-about.jpg         ← download from Wix CDN (URL below)
│   │   └── logo-text.png           ← download from Wix CDN (URL below)
│   └── favicon.ico
├── src/
│   ├── layouts/
│   │   └── Layout.astro            ← base HTML shell, RTL, fonts, sticky nav, footer
│   ├── components/
│   │   ├── Nav.astro               ← sticky nav with smooth-scroll anchor links
│   │   ├── Footer.astro
│   │   ├── ContactForm.astro       ← Formspree-powered
│   │   └── Section.astro           ← reusable <section id="..."> wrapper
│   ├── pages/
│   │   └── index.astro             ← THE ONLY PAGE — all sections in one file
│   └── styles/
│       └── global.css              ← CSS variables, RTL resets, scroll-behavior
├── astro.config.mjs
├── tailwind.config.mjs
└── package.json
```

---

## Image Assets (download before building)

These are the actual Wix-hosted images. Download them to `public/images/`:

```bash
cd ~/src/wix-convert/public/images

# Therapist portrait (used on About page) — 366×493px
curl -o osnat-portrait.jpg "https://static.wixstatic.com/media/c4c9dd_a8d7967b47ed46628c56e098f058dc44~mv2.jpg/v1/fill/w_366,h_493,al_c,q_80,usm_0.66_1.00_0.01,enc_avif,quality_auto/PHOTO-2020-12-15-11-12-48.jpg"

# Hero background / home image — 1280×1280px
curl -o osnat-home-bg.jpg "https://static.wixstatic.com/media/51e9fb7c13a74e5080b9ab96ca114627.jpg/v1/fill/w_1280,h_1280,al_c,q_85,usm_0.66_1.00_0.01,enc_avif,quality_auto/51e9fb7c13a74e5080b9ab96ca114627.jpg"

# Second portrait / about section — 382×365px
curl -o osnat-about.jpg "https://static.wixstatic.com/media/53a572_5c1240740e3b465d93deca0a7ff5c748~mv2.jpg/v1/crop/x_0,y_372,w_1365,h_1304/fill/w_382,h_365,al_c,q_80,usm_0.66_1.00_0.01,enc_avif,quality_auto/IMG_3807-2.jpg"

# Logo text asset — 382×160px
curl -o logo-text.png "https://static.wixstatic.com/media/a5df00_a33f9afeddce45849cbc3f5809489ccb~mv2.png/v1/fill/w_382,h_160,al_c,q_85,usm_0.66_1.00_0.01,enc_avif,quality_auto/Asset%201.png"
```

---

## Content Inventory (all pages, verbatim Hebrew)

### Navigation (all pages)
```
בית | אודות | פסיכותרפיה | יצירת קשר
```
Also in header: phone `050-5289895` and email `soker.osnat@gmail.com` as clickable links.

---

### Page 1: Home (index.astro)

**Section 1 — Rumi/Rilke Hero Quote (full-screen or near-full)**
```
חיה עכשיו
את השאלות
ושמא תגיע
לאט לאט
ביום מן הימים הרחוקים
לחיות את התשובה.

— ריינה מריה רילקה, "מכתבים אל משורר צעיר"
```
With a CTA button: `התקשרו 050-5289895`

**Section 2 — Why Therapy (מדוע אדם פונה לטיפול)**
```
העבודה הנפשית משולה ליציאה למסע ארוך ומסקרן,
שבו ידועה נקודת המוצא אך לא תמיד ידועה הדרך.

אין זה אומר שהדרך היא אינסופית, וחשוב לדעת שלמסע הזה
לא יוצאים לבד אלא בליווי ״מדריך טיולים״ מיומן
המכיר את תנאי השטח, המכשולים האפשריים והאתגרים.

טיפול הוא...
```

**Section 3 — About intro (נעים מאוד)**
```
שמי אסנת סוקר, בוגרת התכנית התלת שנתית בלימודים מתקדמים
"מצבים מנטלים ראשוניים", התכנית לפסיכותרפיה, הפקולטה לרפואה, אוניברסיטת ת"א.

בוגרת התכנית התלת שנתית בלימודים מתקדמים בנושא החשיבה הקלינית
והתאורטית של ביון, מטעם המרכז ללימודי פסיכותרפיה בגישה פסיכואנליטית, גהה.

בוגרת התכנית התלת שנתית ללימודי פסיכותרפיה בגישה פסיכואנליטית מטעם גהה.

בוגרת תואר שני בעבודה סוציאלית במגמה הטיפולית מטעם אוניברסיטת בר אילן (עו"ס קלינית).

במהלך לימודי נמניתי כמצטיינת לייצג את האוניברסיטה בכנסת.
```

**Section 4 — Services overview (two cards)**

Card A — פסיכותרפיה:
```
פסיכותרפיה היא שם נרדף לטיפול נפשי, הלקוחה מהמילה היוונית
פסיכו- נפש ותרפיה - טיפול. הכוונה לטיפול נפשי המתקיים
באמצעות דיבור/שיחות כשהקשר האנושי בין המטפל למטופל
הוא בבסיס העבודה הטיפולית.

אני עובדת בגישה פסיכודינמית (משלבת גם טכניקות קוגניטיביות
התנהגותיות) כשעיקר העבודה נעשית על הלא מודע
והעולם הפנימי של האדם.
```

Card B — הדרכה לאנשי טיפול:
```
העבודה בקליניקה עם מטופלים מחייבת הדרכה וליווי צמודים.
התדירות נקבעת בהתאם לשלב ולצרכיו של המודרך.

למודרך תינתן ההזדמנות להתבונן ולהביא באופן דיסקרטי
ופתוח את הקשיים, ההתלבטויות, ההתמודדויות והאתגרים
השונים הניצבים בפניו כמטפל ולהעמיק את ההבנות הטיפוליות שלו.
```

**Section 5 — Contact info + form**
```
כתובת הקליניקה: יער ירושלים 15/4, מודיעין
טלפון: 050-5289895
אימייל: soker.osnat@gmail.com
```
Plus a contact form with fields: שם (name), אימייל (email), הודעה (message), submit button: `שליחה`

---

### Page 2: About (about.astro)

**Hero quote:**
```
״אני מאמינה שהעבודה איתי יכולה לאפשר לאדם להעיז להיות הוא עצמו״
```

**Bio section (right: text, left: portrait photo)**
```
שמי אסנת סוקר, בעלת תואר שני בעבודה סוציאלית
במגמה הטיפולית מטעם אוניברסיטת בר אילן ופסיכותרפיסטית.

אני מטפלת בגישה פסיכודינמית פסיכואנליטית ומשלבת
גם כלים נוספים (cbt) על פי הצורך.

אני מאמינה שטיפול משמעותי ועמוק יעזור לאדם להיות
במגע עם מה שהוא לא יודע על עצמו ולהכיר את עצמו מבפנים.

אני בוגרת התכנית התלת שנתית בלימודים מתקדמים
במסלול "מצבים מנטלים ראשונים", הפקולטה לרפואה,
בי"ס לפסיכותרפיה, אוניברסיטת תל אביב, מתעסקת
בעבודתי הטיפולית בהתפתחויות הרגשיות המוקדמות
ובשיבושים של תחילת החיים, וכיצד אלו משפיעים על האדם בבגרותו.

אני מאמינה שהעבודה איתי יכולה לעזור לאדם להתחיל
לחשוב מה הסיבה שהוא במצוקה ומדוע היא התעוררה דווקא עכשיו.

מרבית האנשים מפתחים כל מיני סמפטומים רגשיים וגופניים
שלא היו שם קודם, או שהיו שם קודם ושכנו להם במעמקי הנפש,
המתינו בשקט עד שפרצו פתאום לכאורה ללא סיבה מוצהרת.

ההגעה לטיפול יכולה לעזור לאדם להתחיל להתבונן
על מה שקורה בתוכו, להעיז לפענח את החוויות הרגשיות שלו
כדי שבסופו של דבר הוא יוכל בעצמו לחפש מה יאפשר לו
להקל על הסבל שלו ומה יאפשר לו לשנות את מה שלא טוב לו
בחייו כדי שהוא יוכל לחיות חיים טובים, ממשיים ומלאי פוטנציאל.

אני מאמינה שיש לי את היכולות המקצועיות והאישיות
מתוך הניסיון העשיר שצברתי להביא את הפונים אלי
להתפתחות וגדילה נפשית משמעותית.

בנוסף, אני נותנת הדרכות בפסיכותרפיה לאנשי טיפול.
ההדרכה כוללת ליווי של המודרך בשלבי ההכשרה השונים שלו
ומתוך מקום של התבוננות משותפת.
```

**Contact mini-section (same as on home):**
```
אשמח לשוחח!
טל: 050-5289895
מייל: soker.osnat@gmail.com
יער ירושלים 15/4, מודיעין
```
Plus a contact form (same as home page).

---

### Page 3: Psychotherapy (psychotherapy.astro)

Page title: `פסיכותרפיה`

```
פסיכותרפיה היא שם נרדף לטיפול נפשי, הלקוחה מהמילה היוונית
פסיכו- נפש ותרפיה - טיפול. הכוונה לטיפול נפשי המתקיים
באמצעות דיבור/שיחות כשהקשר האנושי בין המטפל למטופל
הוא בבסיס העבודה הטיפולית.

ישנם כיום לא מעט זרמים וגישות טיפוליות וכל מטפל מאמץ
לעצמו בסופו של דבר את הגישה הטיפולית שמתוכה הוא עובד.

אני עובדת בגישה פסיכודינמית (משלבת גם טכניקות קוגניטיביות
התנהגותיות) כשעיקר העבודה נעשית על הלא מודע
והעולם הפנימי של האדם.
```

Also include this (from the about page, relevant to therapy):
```
זהו מרחב להתבוננות משותפת שבו "מחשבות מחפשות חושב" (ביון)
מעין חוות דעת שנייה שתאפשר הסתכלות והפריה מנקודות מבט נוספות.
```

Footer keywords (as subtle tag cloud or just meta tags):
```
פסיכותרפיה | יחסים בין אישיים | אינטימיות | חרדה | דכאון | אובדן | שכול
```

---

### Page 4: Supervision (supervision.astro)

Page title: `הדרכה לאנשי טיפול`

```
העבודה בקליניקה עם מטופלים מחייבת הדרכה וליווי צמודים.
התדירות נקבעת בהתאם לשלב ולצרכיו של המודרך.

למודרך תינתן ההזדמנות להתבונן ולהביא באופן דיסקרטי
ופתוח את הקשיים, ההתלבטויות, ההתמודדויות והאתגרים
השונים הניצבים בפניו כמטפל וללמד מאלו על מנת
להרחיב את ההבנות הטיפוליות שלו ולהעמיק אותן.
```

---

### Footer (all pages)

```
© כל הזכויות שמורות לאוסנת סוקר - פסיכותרפיסטית
פסיכותרפיה | יחסים בין אישיים | אינטימיות | חרדה | דכאון | אובדן | שכול
```

Social: Facebook icon (SVG) linking to https://www.facebook.com/soker.osnat
  Open in new tab: target="_blank" rel="noopener noreferrer"

---

## Design Specification

### Visual Direction
**Mood:** Calm, warm, trustworthy, intimate — a space where someone would feel safe reaching out.  
**NOT:** Clinical white, tech-blue, corporate, cold minimalism.  
**Reference aesthetic:** Like a thoughtful therapist's office — warm light, natural materials, quiet.

### Color Palette (corrected from screenshot analysis)
```css
:root {
  /* Backgrounds */
  --color-bg:        #F5F5F5;   /* light gray — default section bg */
  --color-bg-white:  #FFFFFF;   /* pure white sections */
  --color-bg-dark:   #2B2B2B;   /* near-black — used in "Why Therapy" section */

  /* Text */
  --color-text:      #333333;   /* main body text */
  --color-text-light:#FFFFFF;   /* text on dark sections */
  --color-text-muted:#777777;   /* secondary/captions */

  /* Accent — the CTA buttons and highlights are warm PINK/SALMON */
  --color-accent:       #C96B87;   /* warm pink — extracted from screenshot buttons */
  --color-accent-hover: #B55A76;

  /* Large therapy quote uses PURPLE/VIOLET */
  --color-quote-large:  #7B5EA7;   /* purple — for the big "על הטיפול" quote */

  /* Borders */
  --color-border:    #E0E0E0;
}
```

### Typography
```css
/* Load from Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Assistant:wght@300;400;600;700&family=Heebo:wght@300;400;500;700&display=swap');

:root {
  --font-primary: 'Heebo', sans-serif;     /* main font — used throughout */
  --font-size-base: 16px;
  --line-height-base: 1.8;
}

/* The original site uses sans-serif throughout (Heebo or similar).
   Large quotes use bold weight of the same font, not a different typeface. */
body, h1, h2, h3, p, nav { font-family: var(--font-primary); }
blockquote.large { font-weight: 700; font-size: clamp(1.4rem, 3vw, 2rem); }
```

### Logo
The logo has TWO parts (visible in screenshot top-right, since RTL):
1. Text: "אסנת סוקר | פסיכותרפיסטית" stacked or inline in dark text
2. A colorful decorative icon to its LEFT — circular flower/leaf illustration with multiple colors (pink, green, orange, purple)
   - Recreate this as an SVG or use the downloaded `logo-text.png` asset
   - If recreating as SVG: circular cluster of colorful dots/petals, ~50×50px

### Navigation (corrected from screenshot)
- Sticky top bar, WHITE background
- RTL layout:
  - FAR RIGHT: logo (icon + text "אסנת סוקר | פסיכותרפיסטית")
  - FAR LEFT: tiny phone icon + email icon (clickable), then nav links: בית | אודות | פסיכותרפיה
- Nav links in regular weight, dark text, no underline, hover in --color-accent
- Very thin bottom border on nav
- Mobile: hamburger menu

### Section Order (confirmed from screenshot, top to bottom)
1. **Nav bar** — white, logo right (RTL), nav links left, very thin gray divider below
2. **Hero** — beige/cream bg, Osnat portrait RIGHT (RTL), Rilke poem LEFT, pink pill CTA
3. **Why Therapy** — DARK gray/charcoal bg, two text columns LEFT + large italic quote RIGHT
4. **נעים מאוד (About me)** — white bg, two credential text columns + small accent dividers
5. **על הטיפול (About Therapy)** — white/light bg, LARGE PURPLE BOLD opening quote, then body text
6. **3-column services grid** — gray bg, three columns: פסיכותרפיה | הדרכה | additional info
7. **Contact form** — light bg, form LEFT, contact details RIGHT
8. **Footer** — dark bg, copyright + keywords

### Hero Section (corrected from screenshot)
- Two-column layout: LEFT = poem text, RIGHT = Osnat's photo (cropped, warm tones)
- Background: warm beige/cream with subtle texture or gradient
- Rilke poem in medium-weight sans-serif, large, dark text
- CTA button: pink (#C96B87), rounded pill shape, white text
  Text: "התקשרו, ראשית חינם כאן" or "התקשרו עכשיו"

### Section 2: Hero (confirmed from screenshot)
- **Background:** warm beige/cream — `#F0EBE3` or similar
- **Layout:** two columns, equal width
  - RIGHT column (RTL = visual right): Osnat's portrait photo, cropped roughly square,
    with a warm/golden tone. Image fills the column naturally. NO border-radius.
  - LEFT column: poem text stacked vertically, left-aligned (in RTL = text flows right-to-left)
- **Poem text:** each line on its own line, medium weight (~500), font-size ~1.3rem, dark text
- **Attribution:** smaller, muted, below the poem
- **CTA button:** pill shape (border-radius: 50px), background #C96B87 (pink), white text,
  label visible as something like "התקשרו, ראשית חינם כאן" — confirm exact label from Osnat
- No full-screen height — hero is maybe 50-60vh, natural content height

### Section 3: Why Therapy — DARK (confirmed from screenshot)
- **Background:** #3D3D3D or similar dark charcoal (NOT pure black)
- **All text:** WHITE
- **Layout:** two columns
  - LEFT column: body text paragraphs (regular weight, white)
  - RIGHT column: large italic blockquote in white or very light gray, larger font size:
    "האם תוכל לומר לי בבקשה באיזו דרך עלי לבחור מכאן ? ..."
    below it in smaller text the attribution (ממרחק גדול / קרול)
- **Pink CTA button** at bottom center (same pill style as hero)
- Section padding: generous top/bottom (~80px)

### Section 4: נעים מאוד (About Me) — (confirmed from screenshot)
- **Background:** WHITE
- **Layout:** two columns of text, no photo in this section (photo was in hero)
- Small colored accent icon/divider between heading and text (matches logo colors)
- Credential bullet points as flowing paragraphs (not a bulleted list)
- Ends with pink CTA button "קרא/י להתחיל בדרך" or similar

### Section 5: על הטיפול (About Therapy) — (confirmed from screenshot)
- **Background:** white or very light gray
- **Opening:** VERY LARGE bold purple quote spanning full width or most of it:
  "״החלטה לטיפול אינה מן הקלות וכרוכה בהתייסרות ובהתלבטויות קשה.״"
  Color: #7B5EA7 (purple/violet), font-weight: 700, font-size: clamp(1.5rem, 3.5vw, 2.4rem)
- Below: multiple paragraphs in regular dark text, single column, max-width ~800px centered
- Bold subheadings within the text (same purple or dark color)

### Section 6: 3-Column Services Grid (confirmed from screenshot)
- **Background:** light gray (#F5F5F5)
- **3 equal columns**, each with:
  - Small colored circle icon at top (matching logo palette — pink, green, orange)
  - H3 heading
  - Body text paragraph
- Column topics (right to left, RTL): פסיכותרפיה | הדרכה לאנשי טיפול | [third topic]
- Below each column: a small colored accent line or icon separator

### Section 7: Contact Form (confirmed from screenshot)
- **Background:** white or light gray
- **Layout:** two columns
  - RIGHT (RTL): contact form — label "צרו קשר" or "יצירת קשר" as heading,
    fields: שם (name), מייל (email), הודעה (textarea), button "שליחה" in pink
  - LEFT: contact details block:
    - "כתובת הקליניקה: יער ירושלים 15/4, מודיעין"
    - "טלפון: 050-5289895"
    - "אימייל: soker.osnat@gmail.com"
    - A small embedded Google Maps iframe or static map image (optional)

### Section 8: Footer (confirmed from screenshot SCREENSHOTS/footer detail)
- **Background:** #4A4A4A (medium-dark gray — slightly lighter than the "Why Therapy" charcoal)
- **Text color:** #C8CC3F (olive/lime yellow-green — all footer text uses this color)
- **Scroll-to-top button:** centered at the TOP of the footer — circle outline + up-chevron (↑)
  in the same olive color. Clicking scrolls to #home. Style:
  ```css
  width: 44px; height: 44px; border: 2px solid #C8CC3F; border-radius: 50%;
  display: flex; align-items: center; justify-content: center; cursor: pointer;
  color: #C8CC3F; margin: 0 auto 1.5rem;
  ```
- **Line 1:** "© כל הזכויות שמורות לאוסנת סוקר - פסיכותרפיסטית"
  font-size: 0.9rem, color: #C8CC3F
- **Line 2:** "פסיכותרפיה | יחסים בין אישיים | אינטימיות | חרדה | דכאון | אובדן | שכול"
  font-size: 0.85rem, color: #C8CC3F, letter-spacing: 0.03em
- **DO NOT include** the "עיצוב ובניית אתר - עדי ליניאל" line — omit entirely
- Padding: 2.5rem 1rem, text centered

### Contact Form
Use **Formspree** for zero-backend email:
1. Go to https://formspree.io → create free account → create form → get endpoint URL
2. Form action: `https://formspree.io/f/YOUR_FORM_ID`
3. Fields: `שם` (name, required), `אימייל` (email, required), `הודעה` (message, textarea, required)
4. On submit: show inline Hebrew success message `תודה! ההודעה נשלחה` (no page reload — use `fetch` POST)

---

## Step-by-Step Execution Prompt for Claude Code

**Copy this entire block and paste it into a fresh Claude Code session after installing the `frontend-design` plugin:**

---

```
Use the frontend-design skill.

I want to build a custom Astro website to replace an existing Wix site for an Israeli psychotherapist. 
The project folder already exists at ~/src/wix-convert. 
All image assets are in ~/src/wix-convert/public/images/ (already downloaded).
There is a detailed PLAN.md in ~/src/wix-convert/PLAN.md — read it first before doing anything.

Here is the full project brief:

SITE: Osnat Soker — Psychotherapy practice, Modi'in, Israel
LANGUAGE: Hebrew (RTL) — ALL text is in Hebrew, dir="rtl", lang="he"
FRAMEWORK: Astro with Tailwind CSS v4
DEPLOYMENT TARGET: Cloudflare Pages (static output, no SSR)

STEP 1 — Initialize project:
- Run: npm create astro@latest . -- --template minimal --install --no-git (inside ~/src/wix-convert)
- Add Tailwind: npx astro add tailwind
- Install additional packages: none needed beyond Astro + Tailwind

STEP 2 — Configure RTL + Hebrew fonts:
- In src/styles/global.css: set html { direction: rtl; } and import Frank Ruhl Libre (weights 300,400,500,700) and Heebo (weights 300,400,500) from Google Fonts
- In astro.config.mjs: set output: 'static'

STEP 3 — Create the design system in global.css:
Use these exact CSS variables:
  --color-bg: #FAF7F4
  --color-bg-alt: #F2EDE7
  --color-surface: #FFFFFF
  --color-border: #E8E0D8
  --color-text: #2C2420
  --color-text-muted: #7A6E68
  --color-accent: #8B6F5C
  --color-accent-hover: #7A5F4C
  --color-quote: #5C7A6F
  --font-serif: 'Frank Ruhl Libre', serif
  --font-sans: 'Heebo', sans-serif
Headings and blockquotes use serif. Body text uses sans. Base font size 18px, line-height 1.8.

STEP 4 — Add smooth scroll to global.css:
  html { scroll-behavior: smooth; }
  /* Offset for sticky nav height */
  [id] { scroll-margin-top: 80px; }

STEP 4b — Build these components:
  
  Nav.astro:
  - Sticky top, background white, subtle bottom border (1px, --color-border)
  - LEFT side (in RTL = visual right): site name "אסנת סוקר" in serif, linked to #top
  - RIGHT side (in RTL = visual left): anchor links:
      <a href="#home">בית</a>
      <a href="#about">אודות</a>
      <a href="#psychotherapy">פסיכותרפיה</a>
      <a href="#contact">יצירת קשר</a>
    + phone number 050-5289895 as tel: link in accent color
  - ALL links are same-page anchors (no page navigation)
  - Mobile: hamburger toggle, slide-down nav, closes on link click

  Footer.astro:
  - Background --color-bg-alt
  - "© כל הזכויות שמורות לאוסנת סוקר - פסיכותרפיסטית"
  - Keyword line: "פסיכותרפיה | יחסים בין אישיים | אינטימיות | חרדה | דכאון | אובדן | שכול"
  - Facebook icon (SVG) linking to #facebook placeholder
  - Centered, small text

  ContactForm.astro:
  - Fields: שם (name, text, required), אימייל (email, email, required), הודעה (textarea, required, 5 rows)
  - Submit button: "שליחה" in accent color
  - Use fetch() POST to Formspree endpoint https://formspree.io/f/REPLACE_WITH_FORM_ID
  - On success: show inline Hebrew message "תודה! ההודעה נשלחה" in --color-quote color
  - On error: show "אירעה שגיאה, אנא נסו שנית"
  - IMPORTANT: Add a comment at the top: <!-- TODO: Replace REPLACE_WITH_FORM_ID with actual Formspree form ID -->

  Layout.astro:
  - Accepts props: title (string), description (string)
  - Full HTML shell with <html dir="rtl" lang="he">
  - Import global.css
  - <slot /> between Nav and Footer
  - Open Graph meta tags using title + description props
  - Canonical URL using Astro.url

STEP 5 — Build src/pages/index.astro (ONE FILE, all 8 sections in order):

  CRITICAL: Read ~/src/wix-convert/SCREENSHOTS/ images first.
  The site is single-page. All sections live in index.astro. Each section has an id=
  anchor. Nav links use href="#sectionid" with smooth scroll.

  Section 1 — HERO (id="home"):
    bg: #F0EBE3 (warm beige/cream)
    Two equal columns, NO full-screen height (~55vh natural):
      RIGHT column: <img src="/images/osnat-portrait.jpg"> fills column, no border-radius
      LEFT column (RTL = visual left of photo):
        Poem lines (each on own line, font-weight 500, font-size 1.2rem, dark text, RTL):
          "חיה עכשיו"
          "את השאלות"
          "ושמא תגיע"
          "לאט לאט"
          "ביום מן הימים הרחוקים"
          "לחיות את התשובה."
        Attribution: ריינה מריה רילקה, "מכתבים אל משורר צעיר" (small, muted)
        CTA button: pill shape (border-radius: 50px), bg #C96B87, white text,
          label: "התקשרו, ראשית שיחה חינם" → href="tel:050-5289895"

  Section 2 — WHY THERAPY (id="why-therapy"):
    bg: #3D3D3D (dark charcoal)
    All text: WHITE
    H2 (white): "מדוע אדם פונה לטיפול ?"
    Two columns:
      LEFT column: body paragraphs in white, font-size 0.95rem, line-height 1.9
        "העבודה הנפשית משולה ליציאה למסע ארוך ומסקרן..."
        [full text from content inventory]
      RIGHT column: large italic blockquote in white, font-size 1.4rem:
        "האם תוכל לומר לי בבקשה באיזו דרך עלי לבחור מכאן ? ..."
        attribution below in small muted text
    Pink CTA button centered below both columns (same pill style)

  Section 3 — ABOUT ME (id="about"):
    bg: #FFFFFF
    H2: "נעים מאוד"
    Small decorative colored accent line under heading (4px, #C96B87, width 60px)
    Two columns of credential paragraph text, no photo in this section:
      [full credentials text from content inventory — שמי אסנת סוקר בוגרת...]
    Pink CTA button at bottom: "קבעו פגישת היכרות" → href="#contact"

  Section 4 — ABOUT THERAPY (id="psychotherapy"):
    bg: #FAFAFA (near white)
    Opening LARGE BOLD PURPLE quote (full width or 80% centered):
      "״החלטה לטיפול אינה מן הקלות
      וכרוכה בהתייסרות ובהתלבטויות קשה.״"
      style: color #7B5EA7, font-weight 700, font-size clamp(1.4rem,3.5vw,2.2rem),
             line-height 1.4, text-align center, margin-bottom 3rem
    Below: single column body text (max-width 780px centered):
      [full therapy text from content inventory — all paragraphs]
    Bold subheadings (same purple or dark, font-weight 700) where they appear in original

  Section 5 — SERVICES GRID (id="services"):
    bg: #F5F5F5 (light gray)
    H2 centered: "במה אני עוסקת" or no heading (match screenshot)
    3-column grid (stack to 1 col on mobile):
      Each column has:
        - Small circular colored icon SVG at top (~48px) — use simple colored circle
          with inner shape (leaf/flower): col1=#C96B87 (pink), col2=#7B5EA7 (purple), col3=#5B9E8A (green)
        - H3 heading
        - Body paragraph text
      Column 1 (rightmost, RTL): פסיכותרפיה — text from content inventory
      Column 2 (center): הדרכה לאנשי טיפול — text from content inventory
      Column 3 (leftmost): [if there is a 3rd service — check screenshot; if not, use 2 columns]

  Section 6 — CONTACT (id="contact"):
    bg: #FFFFFF
    H2: "יצירת קשר"
    Two columns (stack on mobile):
      RIGHT column (RTL): ContactForm component
      LEFT column: contact details
        כתובת הקליניקה: יער ירושלים 15/4, מודיעין
        טלפון: <a href="tel:050-5289895">050-5289895</a>
        אימייל: <a href="mailto:soker.osnat@gmail.com">soker.osnat@gmail.com</a>
        Google Maps embed (confirmed from screenshot — see SCREENSHOTS/gmaps.png):
          The business has a named Google Places pin: "אוסנת סוקר- פסיכותרפיסטית"
          Address: יער ירושלים 15, מודיעין מכבים רעות

          The map spans FULL WIDTH below the contact details (not inside the column).
          Height: ~300px. Interactive (zoom, satellite toggle visible in screenshot).

          Use this embed iframe — place it BELOW the two-column contact section,
          full width, no padding on sides:
          <iframe
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3383.5!2d34.9897!3d31.8989!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x1502b5b5b5b5b5b5%3A0x0!2z15nYp9eCINC10YDEg9GI15Qg15%2C%2F15DQtNC40L0g15nQvdC%2B15DQtdC%2B15DQtNClIDE1!5e0!3m2!1siw!2sil!4v1"
            width="100%" height="300" style="border:0; display:block;" allowfullscreen loading="lazy"
            title="מיקום הקליניקה של אסנת סוקר במודיעין">
          </iframe>

          NOTE FOR EXECUTING CLAUDE: The src above may not work exactly.
          To get the correct src: go to https://maps.google.com →
          search "אוסנת סוקר פסיכותרפיסטית מודיעין" → click Share → Embed a map →
          copy the iframe src attribute exactly and use it here.

STEP 6 — Animations (CSS only, no JS libraries):
Add to global.css:
  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .animate-on-load { animation: fadeInUp 0.6s ease-out both; }

Use IntersectionObserver in a small inline <script> to add class "visible" to elements with data-animate attribute as they scroll into view.

STEP 7 — SEO meta per page:
  index.astro:     title="אסנת סוקר | פסיכותרפיסטית | מודיעין", description="טיפול נפשי פסיכותרפויטי בגישה פסיכודינמית. קליניקה במודיעין. אסנת סוקר, פסיכותרפיסטית."
  about.astro:     title="אודות | אסנת סוקר", description="בוגרת אוניברסיטת תל אביב ובר אילן. פסיכותרפיסטית מומחית בגישה פסיכואנליטית."
  psychotherapy:   title="פסיכותרפיה | אסנת סוקר", description="טיפול פסיכודינמי ופסיכואנליטי. גישה המשלבת CBT. קליניקה במודיעין."
  supervision:     title="הדרכה לאנשי טיפול | אסנת סוקר", description="הדרכה מקצועית לפסיכותרפיסטים ואנשי טיפול. מודיעין."

STEP 8 — Run dev server and verify:
  npm run dev
  Check: http://localhost:4321
  Verify: RTL layout, all 4 pages load, mobile responsive, contact form submits (will fail without Formspree ID — that's OK)
  Fix any layout issues before declaring done.

STEP 9 — Build for production:
  npm run build
  Verify dist/ folder is generated cleanly with no errors.

STEP 10 — Git init:
  git init
  git add .
  git commit -m "feat: initial Astro migration of osnat-soker.com"

---
DO NOT:
- Add any English text to the UI (only Hebrew)
- Use purple, blue, or generic AI color palettes
- Use Inter or Roboto fonts
- Add features not in this spec (no blog, no booking, no auth, no CMS)
- Skip the RTL direction setup
- Generate placeholder Lorem Ipsum — use the exact Hebrew text from the PLAN.md

SCREENSHOTS NOTE:
If screenshots are available in ~/src/wix-convert/SCREENSHOTS/, examine them before
building to match the visual feel of the original as closely as possible.
```

---

## After the Build — Manual Steps

These require human action (not automatable by Claude):

1. **Formspree setup:**
   - Go to https://formspree.io → sign up free
   - Create a new form, point it to soker.osnat@gmail.com
   - Copy the form ID (looks like `xpzgkwrb`)
   - In `src/components/ContactForm.astro` replace `REPLACE_WITH_FORM_ID`

2. **Cloudflare Pages deploy:**
   - Push repo to GitHub
   - Go to https://pages.cloudflare.com → New project → Connect GitHub repo
   - Build command: `npm run build`
   - Output directory: `dist`
   - Deploy — Cloudflare gives you a free `.pages.dev` URL

3. **Domain transfer (optional):**
   - In Cloudflare Pages → Custom domains → add `osnat-soker.com`
   - Update DNS nameservers at your domain registrar to point to Cloudflare

4. **Facebook link:**
   - In `Footer.astro`, replace `#facebook` with the actual Facebook page URL

5. **Image optimization (optional, post-launch):**
   - Run `npx astro-imagetools` or use Astro's built-in `<Image>` component to serve next-gen formats

---

## Screenshots Available (already captured)

The original site is a SINGLE PAGE — the nav links are anchor links that scroll within
one long page. So one full-page screenshot captures the entire site.

These are already in `~/src/wix-convert/SCREENSHOTS/`:

- `full.png` (and copy `fullpage-desktop.png`) — full-page screenshot of the entire single-page
  site at desktop width (captured via Chrome DevTools "Capture full size screenshot").
  This shows ALL sections top-to-bottom: hero → why-therapy → about → about-therapy →
  services grid → contact → footer.
- `gmaps.png` — the interactive Google Maps embed at the bottom of the page, showing the
  named pin "אוסנת סוקר- פסיכותרפיסטית" at יער ירושלים 15, מודיעין.

The executing Claude session MUST read both images at the start and use them to calibrate
design decisions (font sizes, spacing, section proportions, exact color tones, layout).

Also read the actual photo assets in `~/src/wix-convert/public/images/` (already downloaded
manually) to understand each image's crop, orientation, and tone before placing them.
