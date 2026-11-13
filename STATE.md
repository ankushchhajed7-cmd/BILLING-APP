# STATE — Ankush Kirana Billing PWA

Live: GitHub Pages of `ankushchhajed7-cmd/BILLING-APP`
Files: `index.html` (whole app), `sw.js` (network-first cache), `manifest.json`, `icon-192.png`, `icon-512.png`

## Current version: v9.1 (13-11-2026) — branch `feature-v9.1`

### v9.1 — fix: Bolke Bill stuck on "Sun raha hoon 0:00"
- Seen on the phone: the screen opened, Pause button showed "▶ Chalu", timer stayed 0:00, nothing heard, Done did nothing visible.
- Cause: the phone's voice service refused the mic (`not-allowed` / mic busy). The app paused itself and showed a toast, but the toast was hidden behind the full-screen panel.
- Fix: red warning box on the screen with the exact reason and what to do; on a permission refusal it asks for the mic once and restarts; separate messages for mic busy (screen recording / call), no internet, unsupported language; toasts now above the panel; empty Done explains why.
- `sw.js` cache `bill-v1.3`.
- Note: screen recording with mic audio can block the voice service on Android — test without it.

## v9.0 (13-11-2026) — merged to main

### v9.0 — "Bolke pura bill banao" (VoiceKhata-style)
- Full-screen mic. Say customer, mobile and ALL items at once → **Done** → draft bill → fix any line → **Bill me daalo**.
- Draft bill: tap a line to change product (top 5 matches + "Naya item"), qty, unit, rate, or delete. **Aur bolo** adds more by voice.
- Voice language for this screen: **Hindi (hi-IN) by default**, Marathi / English selectable on the screen. (Old setting was English, which caused "bolta kuch, sunta kuch".)
- Numbers: Hindi 1–100, Marathi common numbers, sau/hazar/shambhar, dedh, dhai, adich, sava, sadhe, paune, paav, aadha. "Ek sau bees" = 120.
- "150 rupaye ka jeera" → quantity worked out from the saved rate.
- Mobile: 10 digits, pairs, or "double nau".
- Product matching: English name + Marathi name + **Bolchal naam** (new product field) + built-in kirana dictionary (चीनी/शक्कर/साखर = sugar, तेल = oil, तांदूळ/चावल = rice …) + sound-alike matching (कोलगेट → Colgate, पारले जी → Parle G).
- **Learning:** when a product is corrected in the draft, the spoken words are saved as that product's bolchal naam.
- **AI mode (optional):** Report → settings → Gemini API key + model (default `gemini-2.5-flash`) + Free/AI switch. The audio (16 kHz WAV) goes straight to Gemini with the product + customer list; JSON comes back. The key stays in the phone's storage only.
- Cart now shows the unit (kilo, packet, jar…). WhatsApp text/image unchanged.
- Old one-item-at-a-time voice is kept as a small button.
- Dashboard "Voice" tile now opens the new screen.
- `sw.js` cache bumped to `bill-v1.2`.

### Earlier
- v8.3 Vyapar-style dashboard + storage wipe fix
- v8.2 voice overlay fix · v8.1 compact layout · v8.0 Ocean Teal theme
- v7.x voice billing (step-by-step), rate-by-voice, backup reminder
- v6.x and before: product master (~1186 products), customers, purchase/supplier module, WhatsApp Marathi/English bills, poster

## Settings stored (localStorage `ak_settings`)
- `bvlang` — Bolke Bill language (`hi-IN` default)
- `aiKey`, `aiModel`, `aiOn` — Gemini AI mode
- `vlang` — old voice flow language (unchanged)

## Decisions
- Free mode uses the phone's speech engine with short auto-restarts (not `continuous=true`, which duplicates text on Android).
- AI mode records raw audio itself; the phone speech engine is not run at the same time (both on one mic conflict on Android).
- Nothing goes into the bill without the draft screen — every voice result is checked first.
- New products from voice are added to the product list (same as typing a new name).

## Pending / ideas
- Test on the counter with real customers; note any word it still mishears → add it in that product's **Bolchal naam**.
- Confirm the Gemini model name works on the live key (change it in settings if Google renames models).
- Maybe: unit on WhatsApp bill text/image; voice "discount" and "received" like VoiceKhata.
