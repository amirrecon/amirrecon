<!-- PROFILE HEADER -->

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=220&color=0:0EA5E9,100:22C55E&text=AmirRecon&fontAlign=50&fontAlignY=40&desc=Python%20Backend%20%7C%20AI%20%7C%20Cybersecurity&descAlign=50&descAlignY=65" alt="AmirRecon header"/>
</p>

<p align="center">
  <a href="https://github.com/amirrecon">
    <img alt="GitHub followers" src="https://img.shields.io/github/followers/amirrecon?style=for-the-badge&logo=github">
  </a>
  <a href="https://t.me/CodePhreak">
    <img alt="Telegram" src="https://img.shields.io/badge/Telegram-@CodePhreak-27A7E7?style=for-the-badge&logo=telegram&logoColor=white">
  </a>
  <a href="#contact">
    <img alt="Open to work" src="https://img.shields.io/badge/Open%20to%20Work-Backend%20%26%20Security-22C55E?style=for-the-badge&logo=sparkfun&logoColor=white">
  </a>
</p>

<!-- TYPING LINE -->

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=22C55E&center=true&vCenter=true&width=800&lines=FastAPI+%2B+WebSockets+%2F+AI+agents+%2F+exchange+APIs;Security-first+backend+engineering;Crypto+trading+bots%2C+observability%2C+resilience;Learning+bug+bounty+%26+offensive+security" alt="typing"/>
</p>

---

## ⚡ About me

* Python backend dev focused on **FastAPI**, **async I/O**, and **low-latency** systems.
* Building **AI-assisted crypto trading bots** (CoinEx integrations, risk mgmt, SQLite/PG logs).
* Into **cybersecurity**: network, Linux, automation, and bug bounty practice.
* Currently leveling English → **A2 → B2**, and preparing to move abroad (CA/US).

> I like clean architecture, measurable reliability, and shipping secure-by-default code.

---

## 🧰 Tech Stack

<div align="center">

<!-- Languages -->

<img src="https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white&style=for-the-badge"/>
<br/>
<img src="https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Linux-000000?logo=linux&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?logo=githubactions&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white&style=for-the-badge"/>
<br/>
<img src="https://img.shields.io/badge/CoinEx%20API-0466C8?logo=bitcoin&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/WebSockets-111827?logo=socket.io&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/AsyncIO-3B82F6?style=for-the-badge"/>

</div>

---

## 🧪 Current Projects

<table>
  <tr>
    <td width="33%" valign="top">
      <h3>Trading Bot (AI‑driven)</h3>
      <ul>
        <li>FastAPI + WebSocket gateway</li>
        <li>CoinEx integration (tickers, orders)</li>
        <li>Risk mgmt: SL/TP, position sizing</li>
        <li>SQLite/PG logs + analytics</li>
      </ul>
      <a href="#">➡ Repo (private / WIP)</a>
    </td>
    <td width="33%" valign="top">
      <h3>ShadowLink</h3>
      <ul>
        <li>Multi-hop routing (IR→DE→FR→Tor)</li>
        <li>Multi‑layer encryption & token access</li>
        <li>Failover + monitoring roadmap</li>
      </ul>
      <a href="#">Overview (design notes)</a>
    </td>
    <td width="33%" valign="top">
      <h3>GhostCLI</h3>
      <ul>
        <li>Handy automation CLI</li>
        <li>Clean DX, portable modules</li>
      </ul>
      <a href="https://github.com/amirrecon/GhostCLI">View Repo</a>
    </td>
  </tr>
</table>

---

## 🧭 Architecture taste

* **Clean boundaries**: `app/`, `core/`, `adapters/`, `domain/`, `infra/`.
* **Observability**: structured logging, trace IDs, metrics, health probes.
* **Resilience**: retries with jitter, timeouts, circuit breakers, idempotency keys.
* **Security**: least privilege, secret hygiene, dependency pinning + SCA.

```mermaid
flowchart LR
  subgraph Client
    UI[Web/CLI]
  end
  subgraph Backend[FastAPI]
    GW[WS/HTTP Gateway] --> SVC[Services]
    SVC --> MQ[(Broker/Redis)]
  end
  subgraph Workers
    WRK1[Signals] --> DB[(SQLite/PG)]
    WRK2[Order Exec] --> EX[(CoinEx API)]
  end
  UI -->|WS/REST| GW
  SVC --> WRK1 & WRK2
```

---

## 🧷 Snippets I reuse a lot

<details>
  <summary><b>Production logging (uvicorn + stdlib)</b></summary>

```python
import logging
from uvicorn.config import LOGGING_CONFIG

LOGGING_CONFIG["formatters"]["default"]["fmt"] = (
    "%(asctime)s %(levelprefix)s %(name)s [%(process)d] "
    "trace=%(X-Trace-Id)s %(message)s"
)
logging.config.dictConfig(LOGGING_CONFIG)
```

</details>

<details>
  <summary><b>Resilient HTTP client (httpx + retry)</b></summary>

```python
import httpx, asyncio
from backoff import expo, on_exception

@on_exception(expo, (httpx.ReadTimeout, httpx.ConnectError), max_time=30)
async def fetch(url: str):
    async with httpx.AsyncClient(timeout=5) as client:
        r = await client.get(url)
        r.raise_for_status()
        return r.json()
```

</details>

<details>
  <summary><b>FastAPI health & version</b></summary>

```python
from fastapi import FastAPI
from importlib.metadata import version

app = FastAPI()

@app.get("/health")
async def health():
    return {"ok": True, "service": "trading-backend", "version": version("trading-backend")}
```

</details>

---

## 🐞 Bug Bounty / Security Notes

* Practicing **recon → exploit → report** flow.
* Interests: IDOR, authz bypass, SSRF, unsafe deserialization, race conditions.
* Tools I like: **Burp**, **ffuf**, **nmap**, **dirsearch**, **httpx**, **Rustscan**.

---

## 📊 GitHub Stats

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=amirrecon&show_icons=true&theme=transparent" height="150"/>
  <img src="https://github-readme-streak-stats.herokuapp.com?user=amirrecon&theme=transparent" height="150"/>
</p>

<p align="center">
  <img src="https://github-profile-trophy.vercel.app/?username=amirrecon&theme=flat&no-frame=true&row=1&column=6" />
</p>

---

## 🎯 Now / Goals

* ✅ Stabilize trading bot pipeline (signals → orders → logs).
* 🔒 Harden deployments (secrets, SCA, SBOM, CI checks).
* 🗣️ English practice to B2.
* 🌍 Prep for CA/US opportunities.

---

## 🤝 Work with me <a id="contact"></a>

* I build **secure, production‑ready backends** and **automation**.
* DM on Telegram **@CodePhreak**, or open an issue/discussion.

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=22C55E&height=8"/>
</p>
