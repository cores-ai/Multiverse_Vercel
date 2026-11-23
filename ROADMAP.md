# 🗺️ Multiverse Deployment Roadmap & Architectural Blueprint

> **Status:** Stage 1 Complete (Auto-Harvest Loop Fixed & Repositories Segregated)  
> **Target Date:** Final Production Release  
> **Architect:** Momo & Master Engineer

---

## 🛑 1. Root Cause & Emergency Fix: Unwanted Auto-Creation Loop

### Problem
Previously on Vercel, visiting or refreshing the dashboard triggered repetitive OTP requests and account creation loops via Mailu and Cloudflare D1.
- **Root Cause:** In `components/WorkerCanvas.jsx`, an active `useEffect` (`handleGenerateCluster`) automatically triggered on mount whenever an unassigned repository existed or fleet slots were full, executing a full harvest pipeline (`/api/tempmail` -> `/api/railway/harvest` -> `/api/accounts`).
- **Fix Applied:** Gated the trigger with `ENABLE_AUTO_CLUSTER_HARVEST = false`. Auto-harvesting is strictly halted until the entire production pipeline is deployed and tested.

---

## 📁 2. Segregated Repository Architecture

```
d:\Projects\Git Repo\
├── Multiverse_Vercel/          # [Frontend & API Control Center] -> Deploy to Vercel
│   ├── app/                    # Next.js 16 App Router & Route Handlers
│   ├── components/             # Cyberpunk Telemetry & Canvas UI
│   ├── lib/                    # AES-256 Crypto, Cloudflare D1, MongoDB
│   └── package.json            # Clean Next.js dependencies (No Playwright overhead)
│
├── Multiverse_Railway/         # [Autonomous 24/7 Daemon] -> Deploy to Railway
│   ├── Dockerfile              # Debian 12 + Playwright Firefox stealth container
│   ├── auto_railway_daemon.js  # 24/7 autonomous background poller & D1 sync
│   ├── stealth_injections.js   # RTX 3060 WebGL & canvas hardware spoofing
│   ├── token_harvester.js      # Headless Turnstile & OTP bypass engine
│   └── package.json            # Dedicated worker dependencies
│
└── ROADMAP.md                  # This Master Plan
```

---

## 🚀 3. Deployment & Integration Workflow

### Phase 1: Repository Linking (Awaiting GitHub URLs)
- [x] Repositories segregated and initialized locally (`main` branch committed).
- [ ] Receive **Multiverse_Vercel** GitHub remote URL from user.
- [ ] Receive **Multiverse_Railway** GitHub remote URL from user.
- [ ] Execute `git push -u origin main` on both repositories.

### Phase 2: Vercel Configuration (`Multiverse_Vercel`)
1. Import `Multiverse_Vercel` repository into Vercel.
2. Add Environment Variables:
   - `CLOUDFLARE_ACCOUNT_ID`
   - `CLOUDFLARE_D1_DATABASE_ID`
   - `CLOUDFLARE_API_TOKEN`
   - `MONGODB_URI`
   - `ENCRYPTION_MASTER_KEY`
   - `GITHUB_TOKEN`
   - `WEBHOOK_SECRET`
3. Click **Deploy** (Fast, lightweight build with zero browser binary lag).

### Phase 3: Railway Configuration (`Multiverse_Railway`)
1. In Railway, click **New Project** -> **Deploy from GitHub repo** (`Multiverse_Railway`).
2. Add Environment Variables:
   - `CLOUDFLARE_ACCOUNT_ID`
   - `CLOUDFLARE_D1_DATABASE_ID`
   - `CLOUDFLARE_API_TOKEN`
   - `DAEMON_PORT=4000`
   - `POLL_INTERVAL_MS=4000`
3. Railway automatically detects `Dockerfile`, installs Firefox, and runs `CMD ["node", "auto_railway_daemon.js"]` continuously 24/7.

### Phase 4: Verification & Final Polish
- [ ] Test bidirectional sync: Railway Daemon updates Cloudflare D1 -> Vercel UI reflects changes instantly.
- [ ] Confirm no unwanted account loops occur on idle.
- [ ] Zero-downtime Blue-Green migration test at ≤ $2.00 balance.
