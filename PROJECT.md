# MasonboroIsland.org — Project Context

> Picking this up in a future session? Start here. This file captures the state, decisions, and what's in-flight as of April 20, 2026.

## What this is

A coastal field guide for **Masonboro Island, NC** — the undeveloped barrier island between Wrightsville Beach and Carolina Beach. Water-access only, state reserve, wildlife sanctuary. Sister site to **SharkToothIsland.org** (also live, also Mozy Outdoors).

**Canonical domain:** masonboroisland.org (owned, not yet deployed)
**Current file:** `C:\Users\Admin\OneDrive\Documents\Claude\Projects\Masonboro Island\index.html`
**Local preview:** `python -m http.server 8092` from the folder, then `http://localhost:8092/`

## Core concept

A **3-part journey scene** across the top of the page (Mainland → The Crossing → The Island) with animated illustrated SVG elements, then three linear content sections below in the same order:

1. **Before You Go (Mainland)** — launch points, live tides, live weather, packing, rules, sky events
2. **The Crossing** — vessel cards, hazards, rules of the waterway
3. **On the Island** — wildlife, activities, camping, Leave No Trace
4. **Tonight's Sky** — live moon phase, meteor calendar, sun times, live rocket launches (with imminent-launch alert)
5. Email capture + footer

The whole design ethos: **field-guide editorial**, not touristy. Hand-drawn feel, palette-locked, live data where it adds real value.

## Current build status

### ✅ Complete
- Main scene hero (SVG with mainland/waterway/island composition + animated boat/kayak/birds)
- Tent added to island (hand-drawn)
- **Live APIs wired:**
  - NOAA tides (station 8658163 Wrightsville Beach)
  - Open-Meteo weather + dark-sky quality rating
  - Launch Library 2 upcoming rocket launches (filtered to Cape Canaveral/Kennedy, gated to night-visible only)
- Moon phase (client-computed)
- Meteor calendar (hardcoded annual peaks, picks next upcoming)
- Sun/twilight times (client-computed for Masonboro coords)
- Imminent-launch alert banner (≤36h shows a prominent callout)
- ClearDarkSky external link (real URL: `https://www.cleardarksky.com/c/Wlmgtn1NCkey.html?1`)
- Email capture via Web3Forms (same key as STI: `8ca6fa3e-a04a-4261-8937-e214a48f785f`)
- All content written for 3 sections

### 🟡 In progress: SVG graphic pack from Gemini
Being built in batches. Test page: `svg-test.html` (on localhost only — preview before integration).

| Batch | Status | Assets |
|---|---|---|
| Camping & Gear | ✅ 6/6 approved | tent-aframe, tent-dome, camp-stove-ring (campfire), cooler, dry-bag, lantern-hanging |
| Wildlife | ✅ 8/8 approved (2 revised) | pelican-flying, pelican-piling, shorebird-plover, egret-wading, dolphin-fin, dolphin-jumping, sea-turtle-profile, ghost-crab |
| Vegetation | ✅ 5/5 approved | sabal-palmetto, live-oak, sea-oats, marsh-cordgrass, driftwood |
| Watercraft | ✅ 7/7 approved | kayak-orange, kayak-yellow, kayak-green, sup-paddler, fishing-skiff, pontoon-boat, sailboat |
| Structure | ✅ 6/6 approved | boathouse-stilts, dock-short, dock-mid, dock-long, boat-ramp, fish-shack |
| Vehicles | ⏳ Next (3 assets) | pickup-truck, pickup-with-trailer, boat-on-trailer |
| Sky | ⏳ (clouds, moon phases, meteor, rocket trail) | |
| Human figures | ⏳ (standing, walking, crouching) | |

**All approved assets live in `svg-test.html`** with the inline SVG code. Grab them from there.

### ⏳ Integration passes not yet done
Once the full pack is in, there's a final integration session to replace my hand-drawn scene elements with the Gemini versions:
- Swap hand-drawn **sea oats** in main scene for Gemini's `sea-oats` (cleaner)
- Replace current **oak canopy silhouette** with multiple `live-oak` trees
- Add **palmetto trees** on island back
- Swap **kayak/SUP** paddlers in animation for Gemini's cleaner versions (3 kayak colors available — orange, yellow, green)
- Rebuild **mainland dock area** using Gemini's `boathouse-stilts` + dock variants + `boat-ramp` + `fish-shack`
- Use Gemini's `pickup-truck` in place of my simple truck silhouette
- Add wildlife: pelican flying, dolphin fin in water, egret at water's edge, ghost crab on sand

## Palette (matches STI family + coastal extensions)

```css
/* Sky */
--day-sky-top:    #9bc3d9;
--day-sky-mid:    #c2d9e2;
--day-sky-horizon:#e4d4b4;
--night-sky-top:  #0a1028;
--night-sky-mid:  #0f1738;

/* Water */
--water-main:     #4a7591;
--water-highlight:#5e88a5;
--water-shallow:  #3a5a6e;
--water-ripple:   #7ca2ba;

/* Treelines / greens */
--dark-green:     #2d4a3e;
--pine-green:     #1e3529;
--forest:         #1c2f27;
--island-forest:  #243b31;

/* Sand */
--sand-back:      #dcc699;
--sand-front:     #c4a66f;
--sand-light:     #f1e6cc;

/* Structure */
--wood-piling:    #4a3628;
--wood-dock:      #5c4532;
--wood-dark:      #2a1d15;

/* Accents */
--mb-accent:      #d97b2b;  /* orange — heading labels, CTAs */
--mb-accent-light:#f0a050;
--kayak-orange:   #c24f2b;
--kayak-yellow:   #e8b93a;
--sun:            #FFD25F;

/* Ink / text */
--mb-ink:         #1c1a17;
--mb-ink-mid:     #4a4640;
--mb-ink-light:   #7a756d;
--mb-rule:        #d5cfc5;

/* Status semantics */
--mb-good:        #4a7a3f;
--mb-warn:        #b8651a;
--mb-bad:         #a03520;

/* Base */
--mb-cream:       #f5f0e8;
--mb-paper:       #ede7db;
--mb-dark:        #0f2e3d;  /* dark teal — footer, structure */
--mb-teal:        #1a4f5e;
```

## Key coordinates (main scene SVG, viewBox 1600×560)

- Horizon line: **y=340** (unified across all three zones)
- Mainland: x=0–533
- Waterway: x=533–1066
- Island: x=1066–1600
- Water rectangle: x=533, y=340, width=533, height=220
- Near-island water strip: x=1066, y=520, width=534, height=40

## Animation conventions

SVG elements that animate (boat, kayaks, birds, clouds, ripples, rocket trail, sun pulse) use a **wrapper pattern**:

```html
<g transform="translate(X Y)">           <!-- positions -->
  <g class="animated-class-name">        <!-- animates via CSS -->
    <!-- paths -->
  </g>
</g>
```

This is because CSS `transform` on an SVG element *replaces* the `transform="translate(...)"` attribute instead of adding to it. Always wrap in an outer group that holds position.

## Live API reference

### NOAA CO-OPS (tides)
- Station: **8658163** Wrightsville Beach
- Endpoint: `https://api.tidesandcurrents.noaa.gov/api/prod/datagetter`
- Params: `product=predictions&datum=MLLW&interval=hilo&units=english&time_zone=lst_ldt&format=json`

### Open-Meteo (weather)
- Endpoint: `https://api.open-meteo.com/v1/forecast`
- Coords: **34.115, -77.845** (Masonboro Island center)
- Hourly params: `temperature_2m,weather_code,wind_speed_10m,wind_gusts_10m,cloud_cover,relative_humidity_2m,precipitation_probability`

### Launch Library 2 (rockets)
- Endpoint: `https://ll.thespacedevs.com/2.2.0/launch/upcoming/?limit=50&mode=list`
- **CRITICAL:** `mode=list` flattens the response — `location`, `pad`, `lsp_name` are direct string fields, NOT nested objects. Don't access `l.pad.location.name`.
- Filter by regex `/Cape Canaveral|Kennedy/i.test(l.location)` for visibility from NC coast
- Further gate by "night launches only" — launches between (sunset - 30min) and (sunrise + 30min) at Masonboro
- Imminent alert: ≤36h triggers banner

### ClearDarkSky
- No API. Link to: `https://www.cleardarksky.com/c/Wlmgtn1NCkey.html?1`

## Design decisions log

- **Day/night swipe toggle** was the original direction — **scrapped** because it was a gimmick. The 3-part journey structure (Mainland/Crossing/Island) matches real user mental model.
- **Full Gemini-drawn background** considered — rejected. Too disruptive to animation wrappers. Stayed with hand-drawn background + asset drop-ins.
- **SpaceflightNow vs Launch Library 2** — SFN has no API, hand-curated HTML. LL2 is the practical choice.
- **Dark sky "rating"** is a proxy derived from cloud + humidity + rain probability from Open-Meteo. Link out to ClearDarkSky for the real astronomer-grade forecast.
- **Unified horizon at y=340** — fixed the "stepping down left to right" look where sections had different silhouette heights.

## Files in this directory

| File | Purpose |
|---|---|
| `index.html` | Main page |
| `svg-test.html` | Gemini SVG pack evaluation — all approved assets live here |
| `PROJECT.md` | This file |
| `DEPLOY.md` | Instructions to put it online |

## Related projects

- **SharkToothIsland.org** — deployed live, same family, same palette spirit. Use as reference for architecture/patterns (FAQ with JSON-LD, evergreen pillar pages, Field Notes freshness layer).
- **Mozy Outdoors** — parent brand, `mozyoutdoors.com`.

## Open questions / backlog

- After full pack integration, consider: Field Notes-style freshness layer for Masonboro too (trip reports, seasonal conditions)
- Evergreen pillars to eventually write: What is Masonboro, Kayak Access Guide, Camping on Masonboro, Night-Sky Viewing Guide, Wildlife Watching Seasons
- SEO: schema.org/Organization, BreadcrumbList once sub-pages exist
- Google Search Console setup when domain goes live
