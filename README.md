# Interval / Ranges Calculator

A single-file, offline-capable web utility for date intervals, durations, unit conversion, travel time, and measuring closed SVG shapes. Everything runs locally in the browser: no backend, no account, no build step.

> **Delivery:** one self-contained HTML file (`interval-ranges-calculator.html`) containing the markup, CSS, and vanilla JavaScript.
> **Interface language:** English.

---

## Table of contents

1. [Concept and principles](#concept-and-principles)
2. [Quick start](#quick-start)
3. [Tabs and features](#tabs-and-features)
   - [Universal Converter](#1-universal-converter)
   - [Duration Calculator](#2-duration-calculator)
   - [Travel Time](#3-travel-time)
   - [Area Calculator](#4-area-calculator)
4. [Shared interface features](#shared-interface-features)
5. [Date picker and calendar systems](#date-picker-and-calendar-systems)
6. [Calculation rules and accuracy](#calculation-rules-and-accuracy)
7. [Validation and error handling](#validation-and-error-handling)
8. [Privacy and security](#privacy-and-security)
9. [Accessibility](#accessibility)
10. [Browser support](#browser-support)
11. [Project structure and code organization](#project-structure-and-code-organization)
12. [Testing checklist](#testing-checklist)
13. [Known limitations](#known-limitations)
14. [Roadmap ideas](#roadmap-ideas)
15. [License](#license)

---

## Concept and principles

The app is built around one idea: **a number is only useful if you know what it means.** Calendar arithmetic, fixed elapsed time, and approximate conversions are different things, and the interface keeps them separate.

1. **Calculation accuracy.** Calendar-aware results (months, years) are distinguished from fixed-duration results (exact milliseconds) and from approximate conversions (average years and months).
2. **Transparent results.** Notes under each result explain the convention used, such as the 24-hour day, the average Gregorian year, or the sampling method.
3. **Ease of use.** Results update automatically when a valid input changes. There is no Calculate button.
4. **Single-file delivery.** HTML, CSS, and JavaScript live in one file.
5. **Privacy by design.** All calculations run locally. Nothing you enter is transmitted.
6. **Progressive enhancement.** The only external resource is the Inter web font, which has a system-font fallback. Core calculations work with no network access.
7. **Responsive design.** Two-column layout on desktop, single column on mobile, with no horizontal page scrolling.

---

## Quick start

No installation or build is required.

- **Open locally:** double-click `interval-ranges-calculator.html`, or drag it into any modern browser.
- **Host statically:** upload the file to any static host (GitHub Pages, Netlify, Cloudflare Pages, an S3 bucket, and so on). For GitHub Pages, rename it to `index.html`.
- **Optional local server:** `python3 -m http.server 8000`, then open `http://localhost:8000/interval-ranges-calculator.html`.

The only value stored in the browser is your theme choice (`localStorage` key `irc-theme`).

---

## Tabs and features

The navigation has four tabs. **Universal Converter** is leftmost and highlighted in green, and it opens first.

### 1. Universal Converter

Convert a value between units inside a selected category.

| Category | Notes |
|---|---|
| **Length & distance** | nm to km, mils, twips, points, picas, CSS pixels, inches, hands, feet, yards, fathoms, furlongs, miles, nautical miles, astronomical units, light-years, parsecs |
| **Paper & design sizes** | A0–A10, B0–B6, C4/C5/C6/DL envelopes, US Letter/Legal/Tabloid/Executive/Statement/Junior Legal, business cards, 4×6, 5×7, 8×10 photos |
| **Area** | mm² through mi², hectares, acres |
| **Volume** | metric, US customary (tsp, tbsp, fl oz, cup, pint, quart, gallon), imperial, in³, ft³, oil barrel |
| **Mass & weight** | mg to tonnes, oz, lb, stone, US short ton, long ton, carat, grain |
| **Temperature** | Celsius, Fahrenheit, Kelvin, Rankine |
| **Speed** | m/s, km/h, mph, knots, ft/s, speed of light |
| **Pressure** | Pa, kPa, MPa, bar, mbar, atm, psi, mmHg, torr |
| **Energy** | J, kJ, cal, kcal, Wh, kWh, BTU, ft·lbf, eV |
| **Power** | mW, W, kW, MW, mechanical and metric horsepower, BTU/h |
| **Force** | mN, N, kN, lbf, kgf, dyne |
| **Angle** | degrees, radians, gradians, arcminutes, arcseconds, turns |
| **Digital storage** | bits, bytes, decimal units (KB, MB, GB, TB, PB), binary units (KiB, MiB, GiB, TiB, PiB) |
| **Frequency** | Hz, kHz, MHz, GHz, rpm |
| **Fuel economy** | L/100 km, km/L, mpg (US), mpg (imperial) |

**Details**

- Linear categories convert through a base unit using exact defined factors (for example 1 in = 25.4 mm, 1 lb = 0.45359237 kg, 1 US gallon = 3.785411784 L, 1 atm = 101,325 Pa).
- **Temperature** converts through kelvin using exact offset formulas. Values below absolute zero are rejected.
- **Fuel economy** is an inverse relationship, so the value must be greater than 0.
- **Length** adds a **nearest ruler fraction** row (rounded to 1/64 inch, marked exact or rounded).
- **Paper & design sizes** has its own form: paper size, portrait or landscape, display unit (mm, cm, in, pt, pica, or pixels), and DPI. It shows width × height in every unit plus pixels at 72, 96, and your DPI, the area in cm², and the aspect ratio. Pixel sizes are rounded to whole pixels.
- Results display to 12 significant digits to avoid floating-point noise.

### 2. Duration Calculator

One tab with a **Calculation** dropdown that switches between four sub-calculators.

#### Date difference

- Input mode: **Date only** or **Date & time**. Optional **Include end date** toggle.
- Result: calendar breakdown (years, months, days, and time parts in date-time mode), total calendar days, weeks and remaining days, and the equivalent hours, minutes, seconds, and milliseconds.
- If the end date is before the start date, the app shows the interval magnitude and states the direction explicitly.
- Controls: **Use current**, **Swap dates**, **Clear**, **Copy results**.

#### Duration converter

- Convert between milliseconds, seconds, minutes, hours, days, and weeks (fixed units: 1 day = 24 h, 1 week = 7 days).
- Shows a normalized breakdown, for example `90,061 seconds` → `1 day, 1 hour, 1 minute, 1 second`, with an optional weeks component.
- Internally uses integer milliseconds. Decimal input is rounded to the nearest millisecond and the interface says so.

#### Date arithmetic

- Add or subtract years, months, weeks, days, hours, minutes, seconds, and milliseconds from a base date (optionally with time).
- Shows the result date, the day of the week, a summary of the applied duration, and the interval between the original and result dates.
- Month-end clamping applies: Jan 31 + 1 month = Feb 28 (or Feb 29 in a leap year). Feb 29, 2024 + 1 year = Feb 28, 2025.

#### Time unit converter

- Units: millisecond through millennium.
- **Fixed duration** basis: exact conversions among milliseconds, seconds, minutes, hours, days, and weeks (month and larger units are disabled).
- **Average Gregorian year** basis: approximate conversions that involve months, years, decades, centuries, and millennia, using 1 year = 365.2425 days and 1 month = 1/12 year. Approximate results are labeled as such.

### 3. Travel Time

Estimate trip duration from distance and speed.

- **Distance** in kilometers, miles, nautical miles, or meters.
- **Travel mode presets** (typical average speeds, all editable):

| Mode | km/h | Mode | km/h |
|---|---|---|---|
| Ox cart | 4 | Motorcycle | 50 |
| Walking | 5 | Car | 60 |
| Donkey (loaded) | 5 | Bus (intercity) | 70 |
| Horse carriage | 8 | Train (conventional) | 80 |
| Camel (riding) | 10 | Helicopter | 200 |
| Running (jogging) | 10 | High-speed train | 250 |
| Bicycle | 15 | Airplane (commercial) | 850 |
| Horse (single rider) | 20 | Supersonic jet (Concorde cruise) | 2,180 |
| Cargo ship (~14 knots) | 26 | Rocket (low Earth orbit speed) | 28,000 |
| | | Fastest spacecraft (Parker Solar Probe, record) | 692,000 |

- **Custom speed:** edit the speed field or its unit (km/h, mph, knots, m/s) and the mode switches to *Custom speed*.
- **Departure (optional):** enter a local date and time to get an arrival time.
- **Results:** duration (days, hours, minutes, seconds), total hours/minutes/seconds, distance and speed in two units, arrival time, and the time the same distance would take in every preset mode.

> The Parker Solar Probe figure is its record peak speed relative to the Sun (692,000 km/h on 24 December 2024), not a sustained speed. Preset speeds are idealized averages; the calculation is distance ÷ constant speed and ignores stops, traffic, boarding, and acceleration.

### 4. Area Calculator

Measure the perimeter and area of **one closed SVG shape**, then scale the result to real-world sizes. The SVG is only the basis for boundary and perimeter, so organic shapes such as maps are supported.

**Input.** Import a `.svg` file or paste SVG code.

**Accepted:** exactly one shape, which must be closed:

- `<path>` with a single subpath that starts with one `M` and ends with a single `Z` (lines, curves, and arcs are allowed)
- `<rect>` (including rounded corners), `<circle>`, `<ellipse>`
- `<polygon>` with at least 3 points
- `<polyline>` only if its last point equals its first point

**Rejected as invalid (with an inline message):**

- More than one shape, or no supported shape
- `<line>`, open paths, or paths with multiple subpaths (including shapes with holes)
- `<text>`, `<image>`, `<use>`, `<foreignObject>`, `<script>`
- Malformed SVG, zero-size shapes, unsupported path characters, unsupported transforms

**Shape preview.** A collapsible box at the top of the tab, rendered as native SVG so it stays sharp at any zoom.

- Zoom with a logarithmic slider (5% to 500,000%), the mouse wheel, or `+` / `-` keys.
- Drag to pan, or use the arrow keys. `0` or **Fit to view** fits the shape to the current box.
- Drag the bottom-right corner to make the box taller. The default height is 260 px.

**Measurements in pixels.** Perimeter and area are reported in SVG user units, treated as pixels, plus the bounding box. Transforms on the shape and its ancestors are applied (`matrix`, `translate`, `scale`, `rotate`, `skewX`, `skewY`).

**Physical size at a DPI.** Using the DPI field (default 96), the perimeter and area are converted to millimeters, centimeters, meters, kilometers, inches, feet, yards, miles, nautical miles, points, picas, and the matching square units, hectares, and acres.

**Real-world scale (optional).** Provide one real measurement that you know from outside the app:

1. Choose **Real perimeter** or **Real area**.
2. Enter the value and unit.
3. The app computes the pixel-to-real scale and estimates the other quantity.

```
Real perimeter given:  k = real_perimeter / perimeter_px         (meters per pixel)
Real area given:       k = sqrt(real_area / area_px)             (meters per pixel)
Estimated perimeter  = perimeter_px × k
Estimated area       = area_px × k²
Map scale 1 : N      = k / (0.0254 / DPI)
```

Output includes the map scale `1 : N`, the real size of one pixel, and the real perimeter and area in every listed unit. Real-world estimates assume a single uniform scale and ignore map projection distortion, so treat them as approximations.

---

## Shared interface features

- **Automatic updates.** Results change as soon as inputs are valid.
- **Copy per row.** Each result row has a small **Copy** button that copies that value with its unit and context (for example `Real perimeter (given), Kilometers (km): 40075`). The main result also has its own Copy button.
- **Copy results.** The button at the bottom of each result card copies the full result as readable text (calculation type, inputs, result, rows, notes).
- **Reset / Clear.** Restores the current calculator's defaults.
- **Themes.** Light (default), Dark, and System. Switching theme never changes calculation values or the active tab.
- **Compact results.** Smaller text and a two-column list on wide screens, with grouped headings in the Area Calculator.

---

## Date picker and calendar systems

Date fields are plain text inputs (`YYYY-MM-DD`, or `YYYY-MM-DD HH:MM[:SS[.mmm]]` in date-time mode) with a **Pick date** button.

- Type a year within the supported range and press **Go to** (or Enter). The twelve months of that year appear, with the label `Set to year: <year>` at the bottom. The ‹ and › buttons move one year at a time.
- Choose a month, then a day. The input is filled and the result updates. Existing time-of-day is preserved in date-time mode.
- **Supported range:** Gregorian years **1400 to 3000** (constants `YMIN` and `YMAX`). Dates outside it are disabled in the picker and rejected in the inputs.
- **Calendar systems:** Gregorian (Masehi), Islamic Hijriyah (Umm al-Qura), Islamic Hijriyah (civil/tabular), Chinese lunisolar, Hebrew, Persian (Solar Hijri), Buddhist (Thai), Indian National (Saka), Coptic, and Ethiopic. Systems the browser does not support are hidden.
- **Gregorian equivalent is always shown** under the months (for example `Year 1445 (Islamic …) = 19 Jul 2023 – 6 Jul 2024`), with tooltips for each month and day.
- The input always stores the Gregorian date, so every calculation in the app is calendar-system independent.

---

## Calculation rules and accuracy

### Calendar vs. elapsed time

- **Date-only** values are calendar dates, not instants. Differences use UTC day arithmetic, so daylight saving time cannot affect them.
- **Date-time** values are interpreted in the browser's local time zone. Elapsed time is the real millisecond difference between the two timestamps, so it can differ from calendar-day counts across a daylight-saving change. The app shows the active time zone and a note when the UTC offset changes.

### Calendar breakdown algorithm

1. Apply the largest number of whole months from the earlier date that does not overshoot the later one (years are months ÷ 12).
2. Apply whole days.
3. The remainder becomes hours, minutes, seconds, and milliseconds.

Month additions clamp to the last day of the target month.

### Date arithmetic order

Years, then months, then weeks and days (calendar operations), then hours, minutes, seconds, and milliseconds as one elapsed-time adjustment.

### Fixed and approximate conversions

- Fixed: 1 s = 1,000 ms, 1 min = 60 s, 1 h = 60 min, 1 day = 24 h, 1 week = 7 days.
- Approximate: 1 average Gregorian year = 365.2425 days. A calendar month or year has no universal fixed duration; for exact results tied to real dates use Date difference or Date arithmetic.

### Geometry

- Perimeter uses the browser's exact path length (`getTotalLength`). For non-uniform transforms it uses the sum of transformed sample segments.
- Area uses the shoelace formula over 10,000 samples along the path and is multiplied by the transform determinant.
- Self-intersecting outlines are not detected, so use a simple outline.
- SVG user units are treated as pixels. A scaled `viewBox` or CSS size changes the real pixel size.

### Calendars

- Gregorian dates are computed directly with proleptic Gregorian rules for every year.
- Other calendars use the browser's built-in `Intl` data. Lunar calendars can differ by a day from local moon sighting, and Umm al-Qura is table-based and most reliable for 1300–1600 AH.

---

## Validation and error handling

- Empty required inputs show a neutral prompt until you start typing.
- Invalid dates, non-finite numbers, negative values where disallowed, whole-number-only fields, and excessively large values produce an inline message that starts with "Error:" and explains how to fix the input.
- Invalid input never produces a misleading result; results are replaced by a prompt to fix the field.
- Results outside the supported date range (years 1400–3000) are rejected.

---

## Privacy and security

- No analytics, tracking, accounts, or calculation APIs. Dates, durations, SVG content, and results are never sent anywhere.
- Persistence is limited to the theme preference. There is no calculation history.
- Imported SVG is parsed with `DOMParser` into a detached document and is **never inserted into the page**. The preview is rebuilt from the extracted geometry only, and scripts, images, and `<use>` references are rejected.
- The one external request is the Google Fonts stylesheet for Inter. If it is blocked, a system font is used.

---

## Accessibility

- Semantic HTML with explicit labels for every control.
- Tab navigation supports left and right arrow keys; all buttons and the date picker are keyboard operable.
- Visible focus outlines, and validation errors are announced and do not rely on color alone.
- Results use `aria-live` regions. The date picker is a labeled dialog that closes with Esc.
- Respects browser zoom, narrow screens, and `prefers-reduced-motion`.

---

## Browser support

Current versions of Chrome, Edge, Firefox, and Safari on desktop and mobile. Required features: ES2020 JavaScript, CSS grid and custom properties, `DOMParser`, SVG geometry methods (`getTotalLength`, `getPointAtLength`), `ResizeObserver`, and `Intl.DateTimeFormat` with calendar support (non-Gregorian calendars only).

---

## Project structure and code organization

```text
.
├── interval-ranges-calculator.html   # the entire application
├── README.md
└── .gitignore
```

Inside the HTML file:

1. Document structure and metadata
2. Internal CSS (design tokens, light and dark themes, responsive layout)
3. Interface markup for each tab
4. JavaScript, in this order:
   - Utilities: number parsing, formatting, errors, result rendering and copy
   - Calendar engine: date parsing, month and day arithmetic, breakdown
   - Calculators: date difference, duration, date arithmetic, time units
   - Universal Converter data tables and paper sizes
   - Travel Time presets
   - SVG measurement (shape-to-path, transforms, sampling, scale)
   - Shape preview (zoom, pan, fit)
   - Date picker and calendar systems
   - Tab navigation, theme, and wiring

To adjust limits, edit the constants near the date picker: `YMIN` and `YMAX` for the supported years. Travel presets live in the `MD` array, and Universal Converter units live in the `CT` object.

---

## Testing checklist

| Scenario | Expected result |
|---|---|
| 2026-01-01 → 2026-01-02 | 1 calendar day; 2 dates when inclusive counting is on |
| 2024-02-28 → 2024-03-01 | 2 calendar days |
| Add 1 month to 2026-01-31 | 2026-02-28 |
| Add 1 month to 2024-01-31 | 2024-02-29 |
| Add 1 year to 2024-02-29 | 2025-02-28 |
| Convert 90,061 seconds | 1 day, 1 hour, 1 minute, 1 second |
| 1 week in hours | 168 |
| 1 average Gregorian year in days | ≈ 365.2425 (marked approximate) |
| End date before start date | Correct magnitude with an explicit direction note |
| Empty or invalid date, invalid number | Inline message, no misleading result |
| Temperature below absolute zero | Rejected |
| SVG with two shapes, an open path, or `<line>` | Invalid input message |
| 1 Ramadan 1445 (Umm al-Qura) | 11 March 2024 |
| Theme switch | Values and the active tab are unchanged |
| Narrow mobile viewport | No horizontal page scroll; all controls reachable |
| Offline | Everything works except the Inter web font |

---

## Known limitations

- Date range is limited to Gregorian years 1400–3000.
- Time zone is the browser's local zone only; arbitrary zones are not supported.
- Unit conversions between average calendar units are approximations by design.
- SVG: one closed shape only, no holes, no CSS-based transforms, no self-intersection detection.
- Map scale estimates assume uniform scale and do not model projection distortion.
- Lunar calendar month starts follow calculated data, not local sightings.

---

## Roadmap ideas

- Business-day calculator with configurable weekends and holiday data
- Multi-date interval comparison and anniversary calculator
- Saved presets and export to TXT or CSV
- SVG shapes with holes, and multi-shape comparison
- Additional locale-specific calendar displays

---

## License

No license has been specified yet. Add a `LICENSE` file (for example MIT) before publishing publicly.
