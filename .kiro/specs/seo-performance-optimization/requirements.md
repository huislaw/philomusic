# Requirements Document

## Introduction

Philomusic And Art Centre operates a single-page HTML website (`staging.html`) for a music and fine art school located in Pandan Indah, Kuala Lumpur, Malaysia. The site already has basic SEO foundations (title, meta description, canonical URL, Open Graph partial tags, JSON-LD MusicSchool schema). This feature covers a comprehensive set of SEO, performance, accessibility, and mobile-usability improvements to maximise organic search visibility, Core Web Vitals scores, and conversion rate on the existing single-page site.

---

## Glossary

- **Page**: The single HTML file (`staging.html`) that constitutes the entire website.
- **Head_Block**: The `<head>` element of the Page.
- **JSON-LD_Block**: The existing `<script type="application/ld+json">` element in the Head_Block.
- **Nav**: The `<header>` element containing the sticky navigation bar.
- **Hamburger_Menu**: A mobile-visible toggle button that opens/closes the Nav_Links on small screens.
- **Nav_Links**: The `<ul class="nav-links">` list of anchor links inside the Nav.
- **Hero_Section**: The `<section class="hero">` element.
- **Programmes_Section**: The `<section class="services-container">` element.
- **Lead_Magnet_Section**: The `<section class="lead-magnet-section">` element.
- **Info_Section**: The `<section class="info-section">` element.
- **Footer**: The `<footer>` element.
- **Floating_WhatsApp**: The fixed-position WhatsApp anchor link at the bottom-right corner.
- **Logo_Image**: The `<img>` element inside `.logo` that renders `img/philomusic_logo.jpg`.
- **Card**: Each `<div class="card">` element inside the Programmes_Section.
- **Card_Icon**: The `<span class="card-icon">` element inside each Card that currently contains an emoji.
- **CLS**: Cumulative Layout Shift — a Core Web Vitals metric measuring visual stability.
- **LCP**: Largest Contentful Paint — a Core Web Vitals metric measuring perceived load speed.
- **FCP**: First Contentful Paint — time until the first content is rendered.
- **WCAG**: Web Content Accessibility Guidelines version 2.1 at Level AA.
- **Open_Graph**: The set of `<meta property="og:…">` tags in the Head_Block.
- **Twitter_Card**: The set of `<meta name="twitter:…">` tags in the Head_Block.
- **Structured_Data**: The JSON-LD_Block in the Head_Block.
- **Sitemap_Reference**: A `<link rel="sitemap">` or equivalent reference to the sitemap in the Head_Block.
- **Robots_Meta**: A `<meta name="robots">` tag in the Head_Block.
- **Favicon**: The set of `<link rel="icon">` and `<link rel="apple-touch-icon">` elements in the Head_Block.
- **WebP**: The WebP image format for lossy/lossless image compression.
- **Preload_Hint**: A `<link rel="preload">` element that instructs the browser to fetch a resource early.
- **Font_Loading_Strategy**: The technique used to load Google Fonts without blocking rendering.

---

## Requirements

---

### Requirement 1: Complete Open Graph and Social Sharing Meta Tags

**User Story:** As a business owner, I want my website links to display a rich preview card with image, title, and description when shared on Facebook, WhatsApp, LinkedIn, or any other social platform, so that more people click through to the site.

#### Acceptance Criteria

1. THE Head_Block SHALL contain an `og:image` meta tag with an absolute HTTPS URL pointing to a representative image (minimum 1200×630 px) of the school.
2. THE Head_Block SHALL contain an `og:url` meta tag whose value is exactly `https://www.philomusic.com.my/`.
3. THE Head_Block SHALL contain an `og:site_name` meta tag with the value `Philomusic And Art Centre`.
4. THE Head_Block SHALL contain an `og:locale` meta tag with the value `en_MY`.
5. THE Head_Block SHALL contain a `twitter:card` meta tag with the value `summary_large_image`.
6. THE Head_Block SHALL contain a `twitter:title` meta tag whose value is character-for-character identical to the `og:title` content attribute value.
7. THE Head_Block SHALL contain a `twitter:description` meta tag whose value is character-for-character identical to the `og:description` content attribute value.
8. THE Head_Block SHALL contain a `twitter:image` meta tag whose value is character-for-character identical to the `og:image` content attribute value.
9. WHEN the Page HTML is fetched by an Open Graph scraper (e.g., Facebook Sharing Debugger), THE scraper SHALL return a non-empty title, non-empty description, and a non-empty image URL in the parsed object.

---

### Requirement 2: Favicon and Touch Icon

**User Story:** As a visitor, I want to see the school's branding icon in my browser tab and on my home screen when I bookmark the site, so that the school feels professional and trustworthy.

#### Acceptance Criteria

1. THE Head_Block SHALL contain a `<link rel="icon" type="image/png" href="…">` element where the `href` value resolves to a reachable PNG file of at least 32×32 px.
2. THE Head_Block SHALL contain a `<link rel="apple-touch-icon" href="…">` element where the `href` value resolves to a reachable PNG file of at least 180×180 px.
3. WHEN a user opens the Page in a current-version Chromium-based browser, Firefox, or Safari, THE browser tab SHALL display the school's favicon image rather than the default blank-page icon.
4. WHEN a user on an iOS device adds the Page to their home screen, THE home screen shortcut SHALL display the apple-touch-icon image rather than a screenshot of the page.

---

### Requirement 3: Robots Meta Tag and Sitemap Reference

**User Story:** As a business owner, I want search engine crawlers to be clearly instructed to index and follow the page, and to be able to discover the sitemap, so that the site is fully crawlable and indexable.

#### Acceptance Criteria

1. THE Head_Block SHALL contain a `<meta name="robots">` tag whose `content` attribute includes all five of the following directives: `index`, `follow`, `max-snippet:-1`, `max-image-preview:large`, and `max-video-preview:-1`. The `-1` values indicate no numeric limit.
2. THE Head_Block SHALL contain `<link rel="sitemap" type="application/xml" title="Sitemap" href="/sitemap.xml">`.
3. THE Head_Block SHALL NOT contain any `<meta name="robots">` tag whose `content` attribute includes a `noindex` or `nofollow` directive, so that the crawl instruction in criterion 1 is never overridden on the same page.

---

### Requirement 4: Logo Image Dimension Attributes (CLS Prevention)

**User Story:** As a visitor, I want the page layout to remain visually stable as it loads, so that content does not jump around and disrupt my reading experience.

#### Acceptance Criteria

1. THE Logo_Image element SHALL have an explicit `width` attribute whose integer value matches the image file's actual intrinsic pixel width (as reported by an image metadata tool or browser DevTools).
2. THE Logo_Image element SHALL have an explicit `height` attribute whose integer value matches the image file's actual intrinsic pixel height (as reported by an image metadata tool or browser DevTools).
3. WHEN the Page is loaded in a browser with network throttling set to Slow 3G and the Logo_Image file has not yet been fetched, THE browser SHALL reserve a layout space in the Nav area whose aspect ratio equals `width ÷ height` from the attributes, without causing a measurable layout shift after the Logo_Image file arrives. The CSS-applied display size may differ from the intrinsic size via the `height: 48px; width: auto` rule; this does not invalidate the criterion provided both attributes are present with correct intrinsic values.
4. THE Logo_Image element SHALL retain the `alt` attribute with its existing value `Philomusic And Art Centre` without any modification.

---

### Requirement 5: Non-Blocking Google Fonts Loading

**User Story:** As a visitor, I want the page to display content quickly even on a slow connection, so that I do not have to wait for fonts before I can read the page.

#### Acceptance Criteria

1. THE Head_Block SHALL load the Google Fonts stylesheet using the `rel="preload"` + `onload` swap pattern: the `<link>` element SHALL have `rel="preload"`, `as="style"`, and an `onload` attribute that sets `this.onload=null;this.rel='stylesheet'` so the stylesheet is fetched without blocking rendering, then applied asynchronously.
2. THE Google Fonts URL used in criterion 1 SHALL include the `&display=swap` query parameter so each font face has `font-display: swap` applied by Google's CDN.
3. THE Head_Block SHALL include an inline `<noscript>` element containing a `<link rel="stylesheet">` pointing to the same Google Fonts URL, so users with JavaScript disabled receive the web fonts.
4. THE Head_Block SHALL contain `<link rel="preconnect" href="https://fonts.googleapis.com">` and `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`, both placed before the font preload link in document order.
5. WHEN the Page's HTML source is parsed, THE `<link rel="stylesheet">` for Google Fonts SHALL NOT appear in the initial parse without being gated behind the `onload` swap, ensuring the stylesheet does not block the render tree.

---

### Requirement 6: Critical Asset Preloading

**User Story:** As a visitor, I want the most important visual elements — the hero heading font and the logo — to appear as quickly as possible, so that the page feels fast and responsive on first load.

#### Acceptance Criteria

1. THE Head_Block SHALL contain `<link rel="preload" as="image" href="img/philomusic_logo.jpg">` so the browser begins fetching the logo before the body is parsed.
2. IF a woff2 file URL for the Cinzel font (weight 700) is obtainable from Google Fonts CDN at build time, THEN THE Head_Block SHALL contain `<link rel="preload" as="font" type="font/woff2" crossorigin href="…">` pointing to that woff2 URL.
3. WHEN the Page is loaded and the browser's network waterfall is inspected in DevTools, THE Logo_Image fetch SHALL begin before the first `<img>` element in the body is parsed, confirming the preload hint is effective.

---

### Requirement 7: Image Optimisation and Lazy Loading

**User Story:** As a visitor on a mobile device or slow connection, I want images to load efficiently, so that the page does not waste my data or make me wait unnecessarily.

#### Acceptance Criteria

1. THE Logo_Image element SHALL include `loading="eager"` and `decoding="async"` attributes, and SHALL have explicit `width` and `height` attributes matching the image's intrinsic pixel dimensions (consistent with Requirement 4).
2. WHERE any `<img>` elements are added to the Page at a vertical position below 360×640 CSS pixels from the top of the viewport on a mobile screen, THOSE `<img>` elements SHALL have `loading="lazy"`.
3. THE Logo_Image SHALL be served using a `<picture>` element structure: a `<source type="image/webp" srcset="img/philomusic_logo.webp">` element followed by the original `<img src="img/philomusic_logo.jpg" …>` as fallback, so browsers that support WebP receive the smaller WebP file unconditionally.
4. THE Logo_Image `<img>` element SHALL include `decoding="async"` to prevent the image decode from blocking the main thread.

---

### Requirement 8: Mobile Hamburger Navigation Menu

**User Story:** As a mobile visitor, I want to be able to access the navigation links on my phone, so that I can jump to any section of the page without scrolling through the entire content.

#### Acceptance Criteria

1. ON page load, THE Nav_Links SHALL be in the hidden state on screens with a viewport width of 768 px or below (i.e., `display: none` or equivalent CSS that removes them from the visual and interaction layer).
2. THE Nav SHALL contain a Hamburger_Menu `<button>` element that is visible (not `display:none`, not `visibility:hidden`, not `opacity:0`) on screens with a viewport width of 768 px or below.
3. THE Hamburger_Menu button SHALL be hidden (`display:none`) on screens with a viewport width above 768 px.
4. WHEN a user taps or clicks the Hamburger_Menu button while the Nav_Links are hidden, THE Nav_Links SHALL become visible as a full-width panel that drops below the Nav bar (i.e., positioned directly under the header, spanning the full viewport width).
5. WHEN a user taps or clicks the Hamburger_Menu button while the Nav_Links are visible, THE Nav_Links SHALL return to the hidden state.
6. WHEN a user taps or clicks any anchor link inside the Nav_Links while the Nav_Links are visible, THE Nav_Links SHALL transition to the hidden state.
7. WHEN a user taps or clicks anywhere outside the Nav and outside the Hamburger_Menu button while the Nav_Links are visible, THE Nav_Links SHALL transition to the hidden state.
8. IF the Nav_Links are hidden, THE Hamburger_Menu button SHALL have `aria-label="Open navigation menu"` and `aria-expanded="false"`.
9. IF the Nav_Links are visible, THE Hamburger_Menu button SHALL have `aria-label="Close navigation menu"` and `aria-expanded="true"`.
10. THE Nav_Links SHALL remain visible at all times on screens wider than 768 px, matching the existing desktop layout.

---

### Requirement 9: ARIA Landmarks and Semantic Accessibility

**User Story:** As a user relying on a screen reader, I want to navigate between the major sections of the page using landmark shortcuts, so that I do not have to listen to the entire page sequentially.

#### Acceptance Criteria

1. THE `<header>` element SHALL remain a native `<header>` element (implicitly carrying the `banner` landmark role). No `role="banner"` override is required.
2. THE Nav_Links SHALL be wrapped in a `<nav aria-label="Main navigation">` element that is a child of the `<header>`, so screen readers expose a named navigation landmark.
3. THE Hero_Section `<section>` element SHALL have `id="about"` (already present) and `aria-labelledby="hero-heading"`. THE `<h1>` inside Hero_Section SHALL have `id="hero-heading"`.
4. THE Programmes_Section `<section>` element SHALL have `id="programmes"` (already present) and `aria-labelledby="programmes-heading"`. THE `<h2>` inside Programmes_Section SHALL have `id="programmes-heading"`.
5. THE Lead_Magnet_Section `<section>` element SHALL have `id="trial"` and `aria-labelledby="trial-heading"`. THE `<h2>` inside Lead_Magnet_Section SHALL have `id="trial-heading"`.
6. THE Info_Section `<section>` element SHALL have `id="hours"` (already present) and `aria-labelledby="info-heading"`. A visually hidden `<h2 id="info-heading">` (hidden using the CSS clip pattern: `position:absolute; width:1px; height:1px; overflow:hidden; clip:rect(0 0 0 0); white-space:nowrap`) containing the text `Location and Hours` SHALL be inserted as the first child of Info_Section.
7. THE `<footer>` element SHALL remain a native `<footer>` element (implicitly carrying the `contentinfo` landmark role). No `role` attribute is required.
8. WHEN a screen reader user activates the landmarks list (e.g., NVDA `R` key or JAWS `F6` key), THE landmarks list SHALL present at minimum: the banner landmark, the named "Main navigation" landmark, the Hero_Section landmark labelled by "Nurturing Musical Excellence & Creative Fine Art", the Programmes_Section landmark labelled by "Our Music & Art Programmes", the Lead_Magnet_Section landmark labelled by "Unsure Which Instrument Fits Your Child Best?", the Info_Section landmark labelled by "Location and Hours", and the contentinfo landmark.

---

### Requirement 10: Accessible Card Icons

**User Story:** As a user relying on a screen reader or high-contrast mode, I want the programme card icons to not interfere with content comprehension, so that the programmes are described through text rather than decorative emoji.

#### Acceptance Criteria

1. EACH Card_Icon `<span>` element SHALL have `aria-hidden="true"` so that assistive technologies skip the emoji character entirely.
2. EACH Card's `<h3>` element SHALL contain a non-empty text string that names the programme, and EACH Card's `<p>` element SHALL contain a non-empty description, such that the programme is identifiable to a user who cannot see the Card_Icon emoji.
3. EACH emoji character used as a heading prefix in the Info_Section (📍 and ⏰) SHALL be wrapped in a `<span aria-hidden="true">` element, or replaced with an inline SVG that carries `aria-hidden="true"`, so screen readers do not read out the emoji Unicode description. The emoji or SVG SHALL remain visible to sighted users (not hidden via CSS).

---

### Requirement 11: Comprehensive Alt Text Strategy

**User Story:** As a user relying on a screen reader or as a search engine crawler, I want all meaningful images to have descriptive alternative text, so that content is accessible and images contribute to SEO.

#### Acceptance Criteria

1. THE Logo_Image `alt` attribute SHALL have the value `Philomusic And Art Centre logo`.
2. WHERE any `<img>` elements that convey informational content (photographs, illustrations, diagrams) are added to the Page, EACH such `<img>` SHALL have an `alt` attribute whose value is a non-empty string of 125 characters or fewer that describes the image subject and context.
3. WHERE any `<img>` elements that are purely decorative (no informational content) are added to the Page, THOSE `<img>` elements SHALL have `alt=""` (empty string, not omitted) so screen readers skip them without announcing a filename.

---

### Requirement 12: Enhanced Local SEO Structured Data

**User Story:** As a business owner, I want my Google Business Profile and local search listing to show rich information such as ratings, geo-coordinates, and a direct URL, so that local searchers can find and contact the school more easily.

#### Acceptance Criteria

1. THE JSON-LD_Block `@type "MusicSchool"` object SHALL include a `"url"` property with the value `"https://www.philomusic.com.my/"`.
2. THE JSON-LD_Block SHALL include a `"geo"` property of `@type "GeoCoordinates"` with `"latitude": 3.1116` and `"longitude": 101.7469` (coordinates for Pandan Indah, Kuala Lumpur).
3. THE JSON-LD_Block SHALL include a `"sameAs"` array containing at minimum the school's Google Maps listing URL. Additional official social media or directory profile URLs MAY be included.
4. THE Head_Block SHALL contain an `og:image` meta tag with an absolute HTTPS URL (per Requirement 1, criterion 1). THE JSON-LD_Block SHALL include an `"image"` property whose value is the character-for-character identical URL used in the `og:image` meta tag.
5. THE JSON-LD_Block SHALL include a `"currenciesAccepted"` property with the value `"MYR"`.
6. THE JSON-LD_Block SHALL include an `"areaServed"` property whose value is an array containing exactly the three strings: `"Pandan Indah"`, `"Cheras"`, and `"Kuala Lumpur"`.
7. THE JSON-LD_Block SHALL include an `"aggregateRating"` property of `@type "AggregateRating"` with a `"ratingValue"` between 1.0 and 5.0 inclusive, `"bestRating": 5`, and `"ratingCount"` of at least 1.
8. WHEN the JSON-LD_Block is submitted to Google's Rich Results Test, THE test SHALL report zero errors and zero warnings against the Schema.org MusicSchool specification.

---

### Requirement 13: Page-Level Performance Budget and Meta

**User Story:** As a business owner, I want the page to achieve strong Core Web Vitals scores so that Google's ranking algorithm rewards the site with better visibility.

#### Acceptance Criteria

1. WHEN the Page is audited using Google PageSpeed Insights in Lighthouse lab mode with the mobile device preset, THE Performance score SHALL be 90 or above.
2. WHEN the Page is audited using Google PageSpeed Insights in Lighthouse lab mode with the mobile device preset, THE Largest Contentful Paint (LCP) value SHALL be 2.5 seconds or below.
3. WHEN the Page is audited using Google PageSpeed Insights in Lighthouse lab mode with the mobile device preset, THE Cumulative Layout Shift (CLS) score SHALL be 0.1 or below.
4. WHEN the Page is audited using Google PageSpeed Insights in Lighthouse lab mode with the mobile device preset, THE Total Blocking Time (TBT) value SHALL be 200 ms or below.
5. WHEN the Page is audited using Google PageSpeed Insights in Lighthouse lab mode with the mobile device preset after all changes from Requirements 4, 5, and 6 are applied, THE reported CLS score SHALL be numerically lower than the CLS score recorded in the same tool before those changes were applied.
6. THE Page's total HTML document size (uncompressed, as reported by the browser's DevTools Network panel) SHALL remain below 50 KB after all changes are applied.

---

### Requirement 14: Canonical URL Protocol Consistency

**User Story:** As a business owner, I want all self-referencing URLs in the Page to use HTTPS rather than HTTP, so that search engines treat the HTTPS version as the authoritative URL and avoid duplicate content signals.

#### Acceptance Criteria

1. THE `<link rel="canonical">` element in the Head_Block SHALL have `href="https://www.philomusic.com.my/"`.
2. THE Head_Block SHALL contain an `og:url` meta tag (which is currently absent from staging.html) with `content="https://www.philomusic.com.my/"`.
3. THE JSON-LD_Block `@type "MusicSchool"` object SHALL contain a `"url"` property with the value `"https://www.philomusic.com.my/"` (consistent with Requirement 12, criterion 1).
4. THE value of the `href` attribute of the canonical link, the `content` attribute of the `og:url` tag, and the `"url"` property of the MusicSchool JSON-LD object SHALL all be the character-for-character identical string `https://www.philomusic.com.my/`.
