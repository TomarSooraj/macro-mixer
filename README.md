# 🎛️ Macro Mixer

**The economy on a mixing board.** Nine economic dials — gold, bonds, inflation, the dollar and more — wired the way real financial markets behave. Slide any one and watch the rest respond, then click **Explain** under any dial for the math *and* a plain-English version.

It's a teaching toy: a hands-on way to build intuition for how macro factors push and pull on each other.

---

## Quick start

It's a **single, self-contained HTML file** — zero dependencies, no build step, no server.

- **Just open it:** double-click `macro-mixer.html` (opens in your default browser).
- **Or host it:** drop the file on any static host (GitHub Pages, Netlify drop, S3, an intranet share). That's the whole deploy.

> If you want to preview it through a local server instead of `file://`, run `python -m http.server` in this folder and visit `http://localhost:8000/macro-mixer.html`.

---

## What you can do

- **Grab any slider.** Whatever you move becomes *the cause*; the other eight recompute from neutral as its *effects*, rippling through 2nd- and 3rd-order links (e.g. tension → oil → inflation → yields).
- **Click "Explain"** under a dial to open a panel with two tabs:
- 🧸 **ELI5** — plain, everyday language.
- 📐 **Math** — the actual equation/mechanism (Fisher equation, present value/duration, real-yield opportunity cost, etc.).
- It covers what the factor *is* and every relationship it has, with ⚡ marking links that can flip sign by regime.
- **"Why did this move?"** narrates the chain reaction in words after every change.
- **Preset scenarios** — one-click historical setups: Stagflation '70s, 2008 Crisis, COVID Crash '20, Rate-Hike Cycle '22, War Shock — plus Reset.

---

## The nine factors

| | Factor | What it is |
|---|---|---|
| 🏦 | **Policy Rate** | The central bank's overnight rate — the master dial for the cost of money. |
| 🔥 | **Inflation** | How fast prices are rising (expected CPI). |
| 📈 | **Bond Yield** | The 10-year government bond interest rate. |
| 🧾 | **Bond Price** | What that 10-year bond costs — the mechanical inverse of its yield. |
| 🪙 | **Gold** | The classic safe / inflation-hedge asset. |
| 📊 | **Stocks** | The broad equity market — the "risk-on" asset. |
| 💵 | **US Dollar** | Dollar strength (DXY). |
| 🛢️ | **Oil** | Crude oil — energy that powers the economy and feeds inflation. |
| ⚔️ | **Geopolitical Tension** | The world's fear / risk-off dial. |

---

## How it works (the model)

### The relationships
Each pair of factors is linked by a **signed correlation** — they either move *together* (+) or *opposite* (−), with a strength. The signs were validated by three independent reviewer passes (a rates desk, a commodities/FX desk, and an academic-macro lens) and adversarially fact-checked. Highlights:

- **Bond yield ↔ bond price:** inverse *by definition* (present value / duration). Near-locked.
- **Inflation → yields:** the Fisher equation (nominal = real + expected inflation).
- **Inflation → policy rate:** the central bank's Taylor-rule reaction.
- **Gold ↔ real yields:** gold pays no coupon, so it competes with the *real* yield on bonds (nominal − inflation). This is why gold can fall even as inflation rises — if the central bank pushes nominal rates up faster.
- **Geopolitical tension:** safe-haven bid for gold, the dollar and Treasuries; risk-off for stocks; supply premium for oil.

### The engine
A **directed influence network solved by damped iteration.** When you pin a slider, every other factor is nudged toward the weighted sum of its neighbours, repeatedly, until the system settles. Properties:

- **Stable** — it's a contraction with clamping, so it never oscillates or blows up.
- **Multi-hop** — captures indirect effects (tension raises oil, which raises inflation, which lifts yields…).
- A couple of links are deliberately **asymmetric** to respect causation: inflation drives the policy rate *up strongly* (the bank reacts), but the policy rate only cools inflation *weakly and with a lag*.

---

## Accuracy & honest caveats

This is a model of **typical co-movement, not a forecast.** Real markets are noisy and regime-dependent. Built-in honesty notes (also shown in the app footer):

- Links marked **⚡ can flip sign** by regime — e.g. stocks vs. yields move *opposite* in inflation scares but *together* in growth scares.
- **Gold tracks real yields**, which is why the "inflation hedge" only works when inflation outpaces nominal rates.
- **Rate hikes cool inflation only with long lags**, so that link is intentionally weak.
- There's **no explicit growth/GDP factor**, so demand-driven crises (2008, COVID) are represented by *which* sliders their presets pin, rather than by a dedicated dial.

---

## Customizing it

Everything lives in the `<script>` block near the bottom of the file:

- **`FACTORS`** — the nine dials (id, name, emoji, ELI5 + Math blurbs).
- **`EDGES`** — the display relationships (sign, strength, ⚡ regime flag, and the ELI5/Math explanations shown in the drawer).
- **`W`** — the directed influence matrix the engine actually solves (`[from, to, weight]`).
- **`PRESETS`** — each scenario is just a map of `{ factorId: deviation }` to pin.

Change a number, refresh, done. To add a factor, add it to `FACTORS`, add its edges to `EDGES` and `W`, and it renders automatically.

---

*A single-file educational build — no frameworks, no tracking, no network calls.*
