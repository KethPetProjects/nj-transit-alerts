# NJ Transit Delay Alerts — Frontend

## Project Overview
A train delay notification service for NJ Transit commuters. Two interfaces:
1. **index.html** — Traditional form-based subscription
2. **nova.html** — AI-powered conversational interface (Claude Haiku)

## Architecture
```
Azure Static Web App (this repo) → Backend API (Azure Container App)
```

## This Repo — Frontend
- **Type:** Static HTML/CSS/JavaScript (no framework)
- **Deployed:** Azure Static Web App (auto-deploys on git push to main)
- **Live URL:** https://black-plant-0162ad510.4.azurestaticapps.net

## Partner Repo — Backend
- **GitHub:** https://github.com/KethPetProjects/nj-transit-backend (or similar)
- **Local path:** ~/Downloads/train-alerts
- **API URL:** https://train-alerts-api.livelyhill-7b9b1325.westus2.azurecontainerapps.io

## Files
- `index.html` — Main subscription form (traditional UI)
- `nova.html` — Nova AI chat interface (Claude Haiku powered)
- `privacy.html` — Privacy policy (SMS compliance for Twilio A2P)
- `terms.html` — Terms & conditions (SMS compliance for Twilio A2P)

## Deploy Workflow
```bash
# Automatic! Just push to main:
git add .
git commit -m "your message"
git push

# Azure Static Web App auto-deploys in 1-2 minutes
```

## Nova AI Assistant
Nova is an AI-powered chat interface built with:
- **AI:** Claude Haiku via backend proxy `/nova/chat` (NOT direct Anthropic API)
- **Design:** Dark theme, JetBrains Mono + Syne fonts, railway aesthetic
- **State machine:** Guides users through subscription flow
- **Back navigation:** User can say "go back", "undo", "change" at any step

### Nova Flow
```
Welcome → Pick Line (chips) → Pick Station (chips) → Morning Time (text) 
→ Morning Train (cards) → Evening Time (text) → Evening Train (cards) 
→ Phone → Consent Checkbox → Summary → Confirm → Verify Code → Done
→ FAQ mode (post-signup)
```

### Nova Features
- Weather-aware greetings (Open-Meteo API, free, no key needed)
- Time-aware greetings (Good morning/afternoon/evening)
- Sarcastic/friendly personality about NJ Transit delays
- Back navigation from any step
- Smart no-trains-found messaging (shows nearest alternatives)
- Post-signup FAQ mode with chips
- 000000 = universal test code (test mode banner shown)

## index.html Features
- Line → Station → Train selection (corridor-first UI)
- SMS consent checkbox (Twilio A2P compliance)
- Phone formatting with backspace support
- Subscribe + Unsubscribe tabs
- Links to privacy.html and terms.html
- Nova AI feature card/banner at top

## NJ Transit Lines & Stations
Hardcoded in both index.html and nova.html:
- NE: Northeast Corridor (Trenton → NY Penn)
- NC: North Jersey Coast
- RV: Raritan Valley
- ME: Morris & Essex
- MC: Montclair-Boonton
- ML: Main Line
- BC: Bergen County
- PV: Pascack Valley
- AC: Atlantic City
- GS: Gladstone Branch

## API Calls Made from Frontend
```javascript
const API_BASE = 'https://train-alerts-api.livelyhill-7b9b1325.westus2.azurecontainerapps.io';

GET  /trains/{station_code}        // Load trains for station
POST /subscribe                    // Subscribe
POST /verify                       // Verify code
GET  /subscription/{phone}         // Check subscription
POST /unsubscribe/request          // Request unsub code
POST /unsubscribe/confirm          // Confirm unsub with code
POST /nova/chat                    // Nova AI messages (backend proxy)
```

## SMS Compliance (Twilio A2P)
Twilio campaign submitted — awaiting approval. Key compliance elements:
- Consent checkbox on index.html before subscribe button
- Consent card in nova.html before phone confirmation
- privacy.html has full SMS disclosure section
- terms.html has full SMS terms with STOP/HELP instructions
- Both pages publicly accessible (no login required)

## Styling — index.html
- Font: Outfit (Google Fonts)
- Primary color: #E53935 (NJ Transit red)
- Background: #FAFAFA
- Clean, professional, mobile responsive

## Styling — nova.html
- Fonts: Syne (headers) + JetBrains Mono (body)
- Background: #0A0A0F (near black)
- Accent: #E63946 (red), #4CC9F0 (blue), #06D6A0 (green)
- Animated grid background
- Dark chat bubbles
- Mobile optimized (media queries for 640px and 375px)
- iOS zoom prevention (font-size: 16px on inputs)

## Testing
- Use `000000` as verification code (works for subscribe and unsubscribe)
- Test banner shown at top of nova.html in test mode
- Weather loads from Open-Meteo (NYC coordinates hardcoded)

## Known Issues / TODO
- Nova state machine can get confused if user skips steps
- Train line filtering could be improved (currently keyword matching)
- No loading state on index.html train dropdowns

## Important: Never Do This
- Never put API keys directly in HTML files (use backend proxy)
- Never remove the SMS consent checkbox (Twilio compliance)
- Never remove privacy.html or terms.html links
- Never change API_BASE without updating both index.html and nova.html
