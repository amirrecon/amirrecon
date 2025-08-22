# TradeCore (CoinEx) — ready-to-use files

Drop these files into your repo to showcase a **backend** professionally (no frontend needed). They’re tailored to your profile (AmirRecon: trading bot, FastAPI, CoinEx, Telegram channel CodePhreak).

---

## 1) `README.md`

```md
# TradeCore (CoinEx)

> AI‑assisted crypto trading backend with FastAPI, WebSocket cache, SQLite, and risk controls. Read‑only demo for employers.

[![Build](https://img.shields.io/badge/build-passing-success)](#)
[![Python](https://img.shields.io/badge/python-3.10%2B-blue)](#)
[![FastAPI](https://img.shields.io/badge/FastAPI-uvicorn-009688)](#)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](#)

## ✨ Features
- Live **market stream** (WebSocket → in‑memory cache → REST)
- **Order execution** with SL/TP, retries, idempotency
- **Risk engine** (exposure caps, max loss/day, cooldowns)
- **Exchange adapter:** CoinEx (pluggable interface)
- **Logging** + structured events; optional Discord/Telegram hooks
- **Docker** image; one‑command run for demo

## 🧱 Tech Stack
- Python 3.10+, FastAPI, Pydantic v2, httpx
- SQLAlchemy + SQLite (PostgreSQL ready)
- APScheduler (jobs), WebSocket
- Pytest + httpx for API tests

## 🏗️ Architecture
```

client (panel/CLI) → REST/WS → FastAPI app → services → adapters → CoinEx API
↘ SQLAlchemy (DB) ↘ cache (in‑mem)

````

## 🚀 Quickstart (local)
```bash
git clone https://github.com/AmirRecon/tradecore-fastapi.git
cd tradecore-fastapi
cp .env.example .env   # add keys if you need write ops (demo is read‑only)
uvicorn app.main:app --reload --port 8000
# open http://localhost:8000/docs
````

### Docker

```bash
docker build -t tradecore .
docker run -it --rm -p 8000:8000 --env-file .env tradecore
```

## 🔐 Environment

```
API_KEY=...            # not required for read‑only demo
API_SECRET=...
DB_URL=sqlite:///./local.db
```

## 📚 API

| Method | Path             | Description            |
| -----: | ---------------- | ---------------------- |
|    GET | /health          | Liveness/versions      |
|    GET | /market/candles  | OHLCV with params      |
|    GET | /trades?limit=50 | Recent trades          |
|   POST | /orders/buy      | Place buy (needs keys) |
|     WS | /ws/market       | Live ticker/candles    |

**Example**

```bash
BASE=https://demo.amirrecon.dev
curl -sS "$BASE/health"
```

## 🧪 Tests

```bash
pytest -q
```

## 🔒 Security Notes

* No secrets in repo; `.env` is gitignored.
* Demo is **read‑only**; state‑changing endpoints disabled.
* Strict validation + rate‑limiting.

## 📎 Links

* Live Demo: [https://demo.amirrecon.dev](https://demo.amirrecon.dev) (read‑only)
* API Docs: [https://demo.amirrecon.dev/docs](https://demo.amirrecon.dev/docs)
* GitHub: [https://github.com/AmirRecon/tradecore-fastapi](https://github.com/AmirRecon/tradecore-fastapi)
* Telegram: [https://t.me/CodePhreak](https://t.me/CodePhreak)

## 📝 License

MIT © 2025 AmirRecon

````

---

## 2) `/docs/index.html` (one‑file landing + API tester)

Save as `/docs/index.html` to publish on **GitHub Pages**. Replace URLs if different.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>TradeCore (CoinEx) • Backend Demo</title>
  <meta name="description" content="AI‑assisted crypto trading backend with FastAPI, WebSocket cache, and risk controls." />
  <style>
    :root{ --bg:#0b0f14; --panel:#111820; --ink:#e6edf3; --muted:#9fb0c3; --brand:#4cc38a; --card:#0f141b; --border:#1b2530; --radius:16px; }
    *{box-sizing:border-box} html,body{height:100%}
    body{margin:0;background:linear-gradient(180deg,var(--bg),#070a0e 60%);color:var(--ink);font:15px/1.6 system-ui,Segoe UI,Roboto,Ubuntu,Inter,Arial}
    a{color:var(--brand)} code,kbd,pre{font-family:ui-monospace,SFMono-Regular,Menlo,Consolas,Monaco,monospace}
    .wrap{max-width:1050px;margin:0 auto;padding:28px}
    .hero{display:grid;grid-template-columns:1.2fr .8fr;gap:24px;align-items:center}
    .card{background:linear-gradient(180deg,var(--panel),var(--card));border:1px solid var(--border);border-radius:var(--radius);box-shadow:0 10px 30px rgba(0,0,0,.35)}
    .hero .left{padding:28px}
    .kicker{letter-spacing:.2em;color:var(--muted);text-transform:uppercase;font-size:12px}
    h1{margin:.2em 0 .1em;font-size:38px;line-height:1.15}
    .sub{color:var(--muted);margin:8px 0 18px}
    .badges{display:flex;flex-wrap:wrap;gap:8px;margin:12px 0 18px}
    .badge{border:1px solid var(--border);border-radius:999px;padding:6px 10px;color:var(--muted);background:#0c131b}
    .cta{display:flex;gap:12px}
    .btn{display:inline-flex;align-items:center;gap:8px;padding:10px 14px;border-radius:12px;text-decoration:none;background:var(--brand);color:#02130b;font-weight:600;border:0}
    .btn.ghost{background:#0c131b;color:var(--ink);border:1px solid var(--border)}
    .hero .right{padding:20px}
    .stat{padding:16px;border-bottom:1px dashed var(--border)}
    .stat:last-child{border-bottom:0}
    .stat b{display:block;font-size:20px}
    .grid{display:grid;grid-template-columns:repeat(3,1fr);gap:18px;margin-top:28px}
    .tile{padding:18px}
    .tile h3{margin:4px 0 6px}
    .muted{color:var(--muted)}
    .tester{margin-top:30px;padding:18px}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    label{font-size:13px;color:var(--muted)}
    input,select{width:100%;padding:10px 12px;border-radius:10px;background:#0c131b;color:var(--ink);border:1px solid var(--border)}
    .send{margin-top:12px}
    pre{margin:0;background:#0b1220;padding:14px;border-radius:12px;border:1px solid var(--border);overflow:auto}
    .cols{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    .copy{position:relative}
    .copy button{position:absolute;right:8px;top:8px;border:1px solid var(--border);background:#0c131b;color:var(--ink);padding:6px 10px;border-radius:8px;cursor:pointer}
    .pill{display:inline-block;padding:4px 8px;border-radius:999px;font-size:12px;border:1px solid var(--border);background:#0c131b;color:var(--muted)}
    details{border:1px solid var(--border);border-radius:12px;background:#0c131b}
    summary{cursor:pointer;padding:12px 14px}
    details .inner{padding:10px 14px}
    footer{margin:40px 0 12px;color:var(--muted);display:flex;gap:12px;align-items:center}
    @media (max-width:900px){ .hero{grid-template-columns:1fr} .grid{grid-template-columns:1fr} .row,.cols{grid-template-columns:1fr} }
  </style>
</head>
<body>
  <div class="wrap">
    <section class="hero">
      <div class="left card">
        <div class="kicker">Backend • FastAPI • Python</div>
        <h1>TradeCore (CoinEx)</h1>
        <p class="sub">AI‑assisted crypto trading backend with FastAPI, WebSocket cache, and risk controls.</p>
        <div class="badges">
          <span class="badge">Python 3.10+</span>
          <span class="badge">FastAPI</span>
          <span class="badge">SQLAlchemy</span>
          <span class="badge">WebSocket</span>
          <span class="badge">Docker</span>
        </div>
        <div class="cta">
          <a class="btn" href="https://demo.amirrecon.dev/docs" target="_blank" rel="noopener">Open API Docs</a>
          <a class="btn ghost" href="https://github.com/AmirRecon/tradecore-fastapi" target="_blank" rel="noopener">View Source</a>
        </div>
      </div>
      <div class="right card">
        <div class="stat"><b>p95 latency</b><span class="muted">~ XX ms @ 128 conn</span></div>
        <div class="stat"><b>Uptime</b><span class="muted">99.9% (demo)</span></div>
        <div class="stat"><b>Security</b><span class="muted">env‑based secrets • validation • rate‑limit</span></div>
      </div>
    </section>

    <section class="grid">
      <div class="tile card"><h3>Exchange Adapters</h3><p class="muted">CoinEx ready; swap adapters without touching core logic.</p></div>
      <div class="tile card"><h3>Orders + Risk</h3><p class="muted">SL/TP, trailing stop, idempotent retries, backoff.</p></div>
      <div class="tile card"><h3>Streaming</h3><p class="muted">WebSocket market feed with in‑memory cache → REST.</p></div>
    </section>

    <section class="tester card">
      <h2>API Tester <span class="pill">read‑only</span></h2>
      <div class="row">
        <div>
          <label>Base URL</label>
          <input id="base" placeholder="https://demo.example.com" value="https://demo.amirrecon.dev">
        </div>
        <div>
          <label>Endpoint</label>
          <select id="ep">
            <option value="/health">GET /health</option>
            <option value="/trades?limit=5">GET /trades?limit=5</option>
            <option value="/signals/latest">GET /signals/latest</option>
          </select>
        </div>
      </div>
      <div class="send"><button class="btn" id="go">Send request</button></div>
      <div class="cols" style="margin-top:14px">
        <div class="copy"><pre id="req"><code># Request will appear here…</code></pre><button data-copy="req">Copy</button></div>
        <div class="copy"><pre id="res"><code># Response will appear here…</code></pre><button data-copy="res">Copy</button></div>
      </div>
      <details style="margin-top:12px">
        <summary>cURL / Python snippets</summary>
        <div class="inner cols">
          <div class="copy"><pre id="curl"><code>curl -sS https://demo.amirrecon.dev/health</code></pre><button data-copy="curl">Copy</button></div>
          <div class="copy"><pre id="py"><code>import httpx
r = httpx.get("https://demo.amirrecon.dev/health", timeout=15)
print(r.json())</code></pre><button data-copy="py">Copy</button></div>
        </div>
      </details>
    </section>

    <footer>
      <span>Made by AmirRecon • </span>
      <a href="https://github.com/AmirRecon" target="_blank" rel="noopener">GitHub</a>
      <span>·</span>
      <a href="https://t.me/CodePhreak" target="_blank" rel="noopener">Telegram</a>
    </footer>
  </div>
  <script>
    const go = document.getElementById('go');
    const base = document.getElementById('base');
    const ep = document.getElementById('ep');
    const req = document.getElementById('req');
    const res = document.getElementById('res');
    const curl = document.getElementById('curl');
    const py = document.getElementById('py');
    function fmtCurl(url){ return `curl -sS ${url}` }
    function fmtPy(url){ return `import httpx\nr = httpx.get(\"${url}\", timeout=15)\nprint(r.json())` }
    async function send(){
      const url = (base.value || '').replace(/\/$/, '') + ep.value;
      req.innerText = `GET ${url}`;
      curl.innerText = fmtCurl(url);
      py.innerText = fmtPy(url);
      try{
        const r = await fetch(url, { headers: { 'Accept': 'application/json' } });
        const txt = await r.text();
        try{ res.innerText = JSON.stringify(JSON.parse(txt), null, 2); }
        catch{ res.innerText = txt; }
      }catch(err){ res.innerText = `# Request failed\n${err}`; }
    }
    go.addEventListener('click', send);
    document.querySelectorAll('[data-copy]').forEach(btn => {
      btn.addEventListener('click', () => {
        const id = btn.getAttribute('data-copy');
        const el = document.getElementById(id);
        const text = el.innerText; navigator.clipboard.writeText(text);
        btn.textContent = 'Copied'; setTimeout(() => btn.textContent = 'Copy', 900);
      });
    });
  </script>
</body>
</html>
````

---

## 3) Publish & share (super short)

1. **GitHub Pages**: push `/docs/index.html` → *Settings → Pages → Source: docs/* → wait 1–2 min.
2. **Pin in Telegram** (FA):

```
🔹 TradeCore (CoinEx) — بک‌اند تریدینگ
— دمو (Read‑only): https://demo.amirrecon.dev
— Docs: https://demo.amirrecon.dev/docs
— سورس: github.com/AmirRecon/tradecore-fastapi
• FastAPI + WebSocket • SL/TP + ریسک منیجمنت • بدون کلید (فقط رید)
کوتاه تست: curl -sS https://demo.amirrecon.dev/health
```

3. **Security for demo**: read‑only mode، rate‑limit، بدون کلید، IP allowlist یا Basic Auth، لاگ‌ها بدون دیتا حساس.

That’s it. Copy → paste → replace URLs if needed. Ready for employers.
