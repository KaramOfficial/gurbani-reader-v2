# Gurbani Reader — design plan (frontend-design two-pass, pass 1)

Subject: Sikh scripture (Gurbani) with word-synced audio (Sehaj Paath by Bhagat
Jaswant Singh Ji). Audience: a reader doing Nitnem / Sehaj Paath daily, mostly
on a phone, sometimes tablet/desktop. The product's one job: make reading
Gurbani feel like holding a well-made book — and make listening effortless.

Reference frame pinned by the brief: Apple Books. Its words win where they
conflict with generic defaults: paper themes (light / sepia / dark), chrome
that disappears, a library of covers, typographic calm.

## The idea (from the subject)
A printed pothi page: Gurbani set in a serif Gurmukhi face on a quiet page
with real margins, and every Bani given a designed "cover" — its Gurmukhi
name set large on a deep, still colour field — so the library reads as a
shelf of books, not a list of files. One memorable element: the page itself.
Everything around it stays quiet.

## Colour (per theme, Apple Books-style themes are the product feature)
- Light: page #FCFCFA, ink #1C1A17, desk #ECEAE4
- Sepia: page #F5EDD8, ink #46351F, desk #E7DCC0
- Dark:  page #17181A, ink #E9E5D9, desk #0C0C0E
- Accent (all themes, used sparingly: progress, active, links): deep kesari
  #B25F06 (light/sepia), #E08A2E (dark). Vishraam colours per readgurbani
  (#A0728E large, #C9A634 medium, #D18D5B small; lightened on dark).
- Covers: deep fields drawn from a quiet set (indigo #232A3A, maroon #3A2326,
  forest #22332A, kesari-deep #4A2E12, slate #2A2D33, plum #33222E, teal
  #1F3030, brown #38302A) with the Bani name in warm paper white serif.

## Type
- Gurmukhi reading text: "Noto Serif Gurmukhi" (Google Fonts) — the printed
  pothi feel; line-height ~1.9, size user-adjustable 18–30px.
- Latin display (cover titles, headings): "Source Serif 4" — bookish, not a
  SaaS sans. UI chrome (buttons, labels): system sans (-apple-system stack)
  so controls feel native per platform, like Apple Books' own chrome.
- Type scale: cover name 28/1.15; page title 30; section head 15 sans,
  letterspaced small; body 21 default; captions 12.5.

## Layout
- Library (home): header (ੴ mark + "Gurbani Reader"), a Continue Reading
  card when a place is saved, then the shelf: cover grid — phone 2 columns,
  tablet 3–4, desktop 4–5 with a left rail (Library / Sehaj Paath sections)
  at ≥1200px. Below the Banis, "Sehaj Paath" — an Ang entry card + picker.
- Reader: the page is a centred column (measure ~66ch / max 680px) on the
  desk colour; on desktop/tablet the page is a visible sheet (soft shadow,
  rounded 10px) — the one bold object. On phone the sheet fills the screen.
- Desktop page layouts (Aa setting "Pages: One / Two", ≥1000px wide):
  - One: the single scrolling sheet above.
  - Two — the Guru Granth Sahib spread, from open-book photographs and
    readgurbani.com: two pages side by side on the desk, a soft spine
    shadow down the centre gutter, each page's text block inside the
    traditional thin bordered frame, and the folio at the top outer
    corner of each page (ਅੰਗ ੧੬੨ / ਅੰਗ ੧੬੩). In Ang mode every Ang is
    exactly one page, as in the printed book, so spreads pair consecutive
    Angs; in Bani mode the line stream is paginated by measured height
    into numbered pages (lines never split). Spreads stack in the scroll;
    auto-scroll brings the spoken line's spread into view; ←/→ keys and
    edge click zones turn spreads. The frame also appears, quietly, on
    the single desktop page.
- Chrome: top bar (back, title, Aa settings, fullscreen on desktop) and the
  mini-player (cover thumb, title, −15/+15, play/pause, progress hairline)
  fade out while reading/playing; tap or scroll-up brings them back.
- Settings popover (Apple Books "Aa"): theme swatches Light/Sepia/Dark,
  text-size slider, follow-along (Off/Sentence/Word), auto-scroll toggle.
- Motion: only answers actions — view crossfade, sheet open/close, player
  rise. No scroll-jacking, no fade-in-on-scroll cards (taste-skill never list).

## Behaviour carried over (proven in the first Reader)
Same public data (checker JSON + Gursevak page audio). Tap word = light
highlight; double-tap = play from word, highlight expires at word end;
catch-up line flash after auto-scroll returns; follow-along 3-way; auto-scroll
(~35% anchor); infinite Ang stream + Nitnem chain stream (Japji → Jaap →
Tav Prasad Savaiye → Chaupai → Anand → Sukhmani) with the same minimal
separator; vishraam symbols hidden, words coloured; place remembered.

## Anti-slop checks (taste-skill / impeccable pass at the end)
Screenshots at 1440 / 834 / 412 in all three themes; every visual renders at
every width; equal content edges; no purple/lavender; no generic card kit;
fresh-context juror scoring against a 9.2 bar (Cook loop) before handover.

## Gemini recheck — reconciled (2026-11-22)
Gemini reviewed the live v1 and the rebuild brief. Adopted into this plan:
- Separate the two mental models for good: Library (what to read) and the Aa
  popover (how it looks) never share a drawer; app branding steps away in the
  reader — the Bani is the interface.
- Gemini's calibrated theme tokens (Paper #FAF9F6, Puratan Gutka sepia
  #F4ECD8 ink #2C2218, Amrit Vela dark #121212 ink #E8E3D9, kesari accents
  #B45309 / #9A3412 / #D97706, active-word wash #F2E6C9 / #2B2316); no pure
  white/black anywhere; <meta name="theme-color"> syncs with the theme.
- Audio becomes a two-state floating pill: collapsed 48px glass pill with an
  integrated progress hairline; expanded card with scrubber, −15/+15 and a
  segmented speed control (no native <select> anywhere).
- Tap centre of the page toggles translucent chrome; toolbar auto-fades.
- Gurmukhi typography law: never italic, never letter-spacing on Gurmukhi
  (breaks the shirorekha); line-height 1.85; optical size 22px phone /
  26px desktop; breathing room around ੴ and ਮਹਲਾ headers; ਰਹਾਉ lines get a
  quiet semibold emphasis (detected from the text).
- Covers at Gutka 2:3 proportion; Continue Reading hero with progress
  hairline; attribution lives in the Library footer / player info, not in a
  settings drawer.
- Zero-spinner aim: data JSON cached (Cache API) after first fetch so a
  return visit paints text immediately.
- Keyboard: Space play/pause, ←/→ turn spreads / seek.
Not adopted (no data or out of scope for v2.0): transliteration/translation
layers, Larivaar toggle, Raga index, bookmarks/search.
