# AirAware AI – Project Status (with IBM Bob)

> **AI for Sustainability · Virtual Internship Project**  
> Stack: TanStack Start (React 19, Vite 8, TypeScript), Tailwind CSS v4, Recharts, Radix UI  
> AI partner: **IBM Bob** (IBM's AI software-development assistant)

---

## Implemented Features

### 1. Air-Quality Dashboard (`/air-quality`)
- Current AQI displayed as a large number with a conic SVG gauge and the full 0–500 colour-banded scale
- AQI category label, summary description and specific advice text for all six EPA bands
- "Last updated" timestamp shown below the page heading
- Location displayed in the heading and updated on every search
- **Demo/Live badge** shown in the header — switches automatically when the Open-Meteo live API responds

### 2. AQI Display
- 0–500 US-EPA scale with six colour bands: Good / Moderate / Unhealthy for Sensitive Groups / Unhealthy / Very Unhealthy / Hazardous
- Semantic colour tokens (`--aqi-good` through `--aqi-hazardous`) used consistently across gauge, scale bar, pollutant chips and text

### 3. AQI Category
- Category determination in `categoryFor()` ([`src/lib/air-quality.ts`](src/lib/air-quality.ts))
- Each category includes: key, label, range, semantic colour token, one-line advice, and a plain-language summary
- Category label and colour applied to heading, gauge, and scale bar on the dashboard

### 4. PM2.5 and PM10 Information
- Dedicated pollutant cards on the Air Quality dashboard with value, unit, status chip and progress bar
- Separate PM2.5 vs PM10 line chart on the Trends page across 24h / 7d / 30d
- Both pollutants included in the chatbot context sent to the AI engine

### 5. Other Pollutant Information (when available)
- Six pollutants tracked: PM2.5, PM10, CO (mg/m³), NO₂ (µg/m³), SO₂ (µg/m³), O₃ (µg/m³)
- Live API (Open-Meteo) may not return every pollutant at every station; cards show `—` and "Data not available at this station" rather than a misleading zero
- Each card includes a plain-language description of the pollutant's source and effects

### 6. Air-Quality Trends (`/trends`)
- 24-hour, 7-day and 30-day range selector
- Summary statistics panel: average AQI, highest AQI, lowest AQI, average PM2.5
- Insight text comparing the current period with the previous period (improving / stable / worsening)
- AQI area chart with category bands
- PM2.5 vs PM10 comparative line chart
- 24-hour AQI forecast using linear regression on the last 72 hours, adjusted for the daily traffic cycle
- Forecast clearly labelled "Demonstration estimate only"

### 7. Location Selection
- `LocationSearch` component present on every page (Home, Dashboard, Trends)
- Tries Open-Meteo geocoding + air-quality API; falls back gracefully to deterministic demo data
- Refresh button to re-fetch on demand; shows loading spinner while fetching
- Error message displayed inline when a location is not found

### 8. AirAware Chatbot — powered by IBM Bob (`/assistant`)
- The chatbot presents as **IBM Bob**, IBM's AI software-development partner, throughout the entire UI
- Page title: "IBM Bob – AirAware AI Assistant"; heading: "IBM **Bob**"; subtitle: "AI Software-Development Partner · AirAware AI"
- Engine badge reads "IBM Bob · IBM Granite via watsonx.ai" or "IBM Bob · Demo Mode"
- Thinking indicator: "IBM Bob is thinking…"; input placeholder: "Ask IBM Bob about AQI, PM2.5, precautions…"
- Footer: "IBM Bob is powered by IBM Granite on watsonx.ai…" or "IBM Bob is in Demo Mode…"
- Full chat UI with user and IBM Bob message bubbles, auto-scroll, and keyboard-submit
- Required questions handled explicitly:
  - **"What does AQI mean?"** → explains the 0–500 scale with all six bands plus current context
  - **"What is PM2.5?"** → explains fine-particle definition, sources and lung-penetration risk
  - **"Why is the AQI high?"** → context-aware: reads current AQI, identifies elevated pollutants and explains typical sources
  - **"What can I do when air quality is poor?"** → serves per-AQI-category recommendations using current AQI
  - **"How can I reduce my environmental impact?"** → seven SDG-aligned sustainability tips
- Suggested questions chip bar with all five required questions plus "Explain today's AQI"
- Engine badge: **IBM Bob · IBM Granite** (green dot) when watsonx.ai is configured; **IBM Bob · Demo Mode** otherwise
- Demo engine: rule-based `generateAssistantReply()` introduces itself as IBM Bob; curated knowledge base with live context injection
- IBM Granite engine: system prompt instructs the model to introduce itself as IBM Bob; full context block (location, AQI, category, all six pollutants, timestamp, source label) prepended to every prompt

### 9. Sustainability Recommendations
- Recommendations panel on the Air Quality dashboard ("What should I do?") with three per-category recommendations
- Six categories of recommendations (Good → Hazardous) defined in `recommendationsFor()` in [`src/lib/air-quality.ts`](src/lib/air-quality.ts)
- SDG references (SDG 11, SDG 13) surfaced in the chatbot sustainability response, the About page, and the footer
- `SUSTAINABILITY_TIPS` constant for future use in additional surfaces

### 10. Responsible AI Safeguards
- **System prompt** (`AIRAWARE_SYSTEM_PROMPT` in [`src/lib/watsonx.server.ts`](src/lib/watsonx.server.ts)):
  - Instructs the model to use only loaded data, never invent measurements
  - Explicitly prohibits medical diagnosis or personalised treatment advice
  - Requires distinguishing measured data from estimates/predictions
  - Instructs to state data unavailability rather than guessing
- **Responsible AI disclosure panel** on the Assistant page: collapsible "How this AI works" section listing data source, AI engine, what the assistant will/won't do, no-medical-advice statement, and prediction disclaimer
- **Demo/Live data badge** on every page that shows data — user always knows whether readings are live or simulated
- **Disclaimer footer** below the chatbot: "General information only — not medical advice."
- **Notice banner** in the chat when Granite is unavailable, explaining that demo answers are being shown instead
- **Forecast disclaimer**: "Demonstration estimate only" chip on the forecast card

---

## Architecture

```
src/
├── routes/
│   ├── __root.tsx          # Shell + AirQualityProvider + SiteNav + SiteFooter
│   ├── index.tsx           # Home page with live glance card
│   ├── air-quality.tsx     # AQI dashboard + pollutant breakdown + recommendations
│   ├── trends.tsx          # Trend charts + forecast
│   ├── assistant.tsx       # Chat UI + engine badge + responsible AI panel
│   ├── about.tsx           # Project explanation, SDG alignment, future scope
│   └── api/
│       └── airaware-chat.ts  # Server handler: Granite → demo fallback
├── lib/
│   ├── air-quality.ts          # Types, AQI categories, pollutant metadata,
│   │                            #   demo data generator, analytics, recommendations
│   ├── air-quality.functions.ts # Server function: Open-Meteo live API
│   ├── assistant.ts             # Rule-based demo reply engine + SUGGESTED_QUESTIONS
│   └── watsonx.server.ts        # IBM IAM auth, Granite chat, AIRAWARE_SYSTEM_PROMPT
├── context/
│   └── air-quality-context.tsx  # React context: location, snapshot, loading, error
└── components/
    ├── demo-badge.tsx       # "Demo data" / "Live data" badge
    ├── location-search.tsx  # Search form + refresh + last-updated timestamp
    ├── site-nav.tsx         # Sticky top nav with mobile hamburger menu
    └── site-footer.tsx      # Footer with SDG alignment note
```

### Data flow

```
User searches location
  → LocationSearch → AirQualityContext.search()
    → fetchAirQuality() (src/lib/air-quality.ts)
      → fetchLiveAirQuality() server fn (Open-Meteo geocoding + AQ API)
        ✓ success → snapshot with source: "live"
        ✗ fails   → generateDemoSnapshot() → snapshot with source: "demo"
      → all pages re-render with new snapshot
```

### Chat flow

```
User types question
  → POST /api/airaware-chat  { question, context (AQI snapshot) }
    → readWatsonxConfig()
      ✓ keys present → generateWithGranite() → IBM IAM token → Granite chat
        ✗ error      → generateAssistantReply() + notice
      ✗ no keys     → generateAssistantReply() (demo) + notice
    → { answer, engine, notice? }
```

---

## IBM Bob's Role

IBM Bob is both the **AI software-development partner** that built this project **and the identity of the chatbot** that users interact with on the `/assistant` page.

### As development partner
- **Code generation**: All route components, context provider, data layer functions, and the watsonx.ai client were produced or refined with IBM Bob
- **Architecture decisions**: IBM Bob proposed the single-seam data-service pattern (one module owns all data-source logic), the context-injection approach for grounding Granite responses, and the responsible AI layering strategy
- **Responsible AI design**: IBM Bob authored the `AIRAWARE_SYSTEM_PROMPT` with explicit guardrails against medical advice and data invention, and designed the Responsible AI disclosure panel
- **Code quality**: IBM Bob enforced TypeScript strict types, the null-safe pollutant display, and the clean fallback chain (live API → demo data → inline error)
- **Documentation**: This `PROJECT_STATUS.md` was produced by IBM Bob based on a full codebase inspection

### As the chatbot identity
- The `/assistant` page presents IBM Bob as the named assistant: heading, engine badge, greeting message, thinking indicator, input placeholder, and footer all use the "IBM Bob" name
- When IBM Granite on watsonx.ai is active, the system prompt (`AIRAWARE_SYSTEM_PROMPT` in [`src/lib/watsonx.server.ts`](src/lib/watsonx.server.ts)) instructs the model to introduce itself as IBM Bob
- When running in Demo Mode, the rule-based engine in [`src/lib/assistant.ts`](src/lib/assistant.ts) introduces itself as IBM Bob in greetings and fallback replies
- Users asking "Who are you?", "What is Bob?", or "IBM Bob" receive an IBM Bob self-introduction in both engine modes

---

## Remaining Work

| Priority | Item |
|---|---|
| High | Connect a real air-quality API with an API key (IQAIR, CPCB, or OpenAQ paid tier) for locations where Open-Meteo AQ is sparse |
| High | Configure IBM watsonx.ai credentials (`IBM_WATSONX_API_KEY`, `IBM_WATSONX_PROJECT_ID`) to enable the Granite engine in production |
| Medium | Cache recently-searched locations in `localStorage` and show them as quick-select chips |
| Medium | Multi-city comparison view (side-by-side AQI cards for two or more locations) |
| Medium | Location-based AQI alerts (browser notification when AQI crosses a user-set threshold) |
| Low | Train a proper forecasting model on historical pollution and meteorological data |
| Low | Dark-mode toggle in the UI (CSS tokens are already defined; just needs a theme-switcher component) |
| Low | Internationalisation (i18n) for non-English speaking users |

---

## Known Limitations

| # | Limitation | Impact |
|---|---|---|
| 1 | **Demo data only** when live API is unavailable or returns sparse data | Readings are plausible but not real measurements; clearly labelled |
| 2 | **Open-Meteo optional pollutants** (CO, NO₂, SO₂, O₃) return `null` / `0` for some stations | Dashboard now shows `—` with "Data not available at this station" |
| 3 | **Demo chatbot has limited vocabulary** — unknown questions fall through to a generic reply | IBM Granite (when connected) handles free-form questions naturally |
| 4 | **Forecast is a linear regression** on 72 hours of demo data, not a real ML or weather model | Forecast card is explicitly labelled "Demonstration estimate only" |
| 5 | **No persistent location history** — refresh or navigation resets to default (Bengaluru, India) | Workaround: use the Location Search on any page |
| 6 | **No server-side rate limiting** on `/api/airaware-chat` | Acceptable for a student prototype; add before production |
| 7 | **IAM token is not cached** — each Granite request fetches a fresh IBM IAM token | One extra HTTPS round-trip per message; negligible for demo use |

---

## Testing Performed

### Dashboard
- ✅ Default location (Bengaluru, India) loads with demo data on first render
- ✅ AQI gauge, 0–500 scale, category label and summary displayed correctly
- ✅ All six pollutant cards render with value, unit, status chip and progress bar
- ✅ "Last updated" timestamp displayed in the page header
- ✅ Demo badge visible in top-right of dashboard header
- ✅ Searching a new city (e.g. Tokyo, London, New York) updates all cards correctly
- ✅ Pollutant cards show `—` and "No data" chip when a pollutant value is zero (live API gap scenario)

### Trends
- ✅ 24h / 7d / 30d range selector updates both charts and statistics
- ✅ AQI area chart and PM2.5 / PM10 line chart render with Recharts
- ✅ Insight text updates (improving / stable / worsening) based on period comparison
- ✅ Forecast chart renders with "Demonstration estimate only" chip
- ✅ Location search updates the entire trends view

### Chatbot
- ✅ "What does AQI mean?" → full 0–500 scale explanation + current conditions
- ✅ "What is PM2.5?" → particle size, health impact, sources + current PM2.5 value
- ✅ "Why is the AQI high today?" → current AQI, dominant pollutants, common causes
- ✅ "What can I do when air quality is poor?" → per-category action recommendations
- ✅ "How can I reduce my environmental impact?" → seven SDG-11/13 sustainability tips
- ✅ "Explain today's AQI in simple words." → current summary with practical advice
- ✅ Suggested question chips fire the correct question
- ✅ Engine badge shows "Demo Mode" when no watsonx keys are set
- ✅ Responsible AI disclosure panel expands and collapses correctly
- ✅ "Not medical advice" disclaimer visible below the chat

### Responsible AI
- ✅ Demo data badge visible on Home, Dashboard, and Trends pages
- ✅ Forecast card has explicit "Demonstration estimate only" badge
- ✅ Chatbot disclaimer footer always visible
- ✅ Responsible AI disclosure panel lists all six required safeguard points
- ✅ Chatbot does not invent AQI readings — returns "not loaded" message when context is null

### Responsive Design
- ✅ Mobile nav hamburger menu opens and closes correctly
- ✅ Dashboard grid reflows from 3-column to 2-column to 1-column on narrower widths
- ✅ Pollutant grid reflows from 3-column to 2-column to 1-column
- ✅ Hero section stacks vertically on small screens
- ✅ Chat input and send button remain usable at mobile width

### Error Handling
- ✅ Empty location search shows inline error "Please enter a city or location."
- ✅ Unknown location (e.g. "xyznotacity") falls back to demo data (no crash)
- ✅ Network error on fetch falls back to demo data with console warning
- ✅ Empty chatbot question — Send button disabled until text is entered
- ✅ Chatbot fetch error shows friendly inline error message
- ✅ 404 page rendered for unknown routes; root-level error boundary catches render errors

### Build & Lint
- ✅ `bun run lint` passes with no errors
- ✅ TypeScript compiles without errors (`tsc --noEmit`)
