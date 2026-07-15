# Deploying to leylinesofpower.com

**Status: not yet live.** Code is committed locally; this doc is the step-by-step to get it onto GitHub, hosted on Vercel, and serving at the bare `leylinesofpower.com` domain (with `play.leylinesofpower.com` already live in the separate `LoP-Simulator` repo, per that repo's own `DEPLOYMENT.md`).

This is a static Astro build with no backend of any kind — no Supabase, no env vars, no server code — so it's an even simpler deploy than the simulator.

## 1. Push to GitHub

1. Go to [github.com/new](https://github.com/new), create a new repository (e.g. `lop-website`). Leave it empty — no README/`.gitignore`/license (this repo already has those).
   - Public or private both work with Vercel's free tier.
2. Copy the repository URL GitHub gives you (looks like `https://github.com/your-username/lop-website.git`).
3. Tell me that URL and I'll add it as the git remote and push the initial commit — pushing is something I check with you before doing, so just say go once the empty repo exists.

## 2. Create a Vercel project

1. Go to [vercel.com](https://vercel.com) and sign in — if you used "Continue with GitHub" for the simulator project already, the same login already has access to import this new repo too.
2. **Add New → Project**, then **Import** the GitHub repo you just pushed.
3. Vercel auto-detects Astro — the defaults it fills in should already be correct:
   - **Framework Preset**: Astro
   - **Build Command**: `npm run build` (equivalent to `astro build`)
   - **Output Directory**: `dist`
4. No environment variables needed — skip that section.
5. Click **Deploy**. First deploy takes a minute or two; you'll get a working `https://your-project-name.vercel.app` URL immediately.

## 3. Add the custom domain in Vercel

1. In the Vercel project, go to **Settings → Domains**.
2. Add `leylinesofpower.com` (the bare/apex domain, not a subdomain this time).
3. Also add `www.leylinesofpower.com` — Vercel will offer to redirect it to the apex automatically, which is the usual setup (visitors to either address land on the same site).
4. Vercel will show you the exact DNS records it wants for each. Apex/root domains generally **can't use a CNAME** (that's a DNS-spec limitation, not a Vercel one), so expect:
   - `leylinesofpower.com` → an **A record** pointing at Vercel's IP (Vercel's dashboard shows the current value, commonly `76.76.21.21`)
   - `www.leylinesofpower.com` → a **CNAME** pointing at `cname.vercel-dns.com`
   - Use whatever Vercel's Domains page displays at the time, in case these values have changed.

## 4. Point the domain at Vercel from GoDaddy

1. Log into GoDaddy, go to **My Products → DNS** (or **Domain Settings → Manage DNS**) for `leylinesofpower.com`.
2. You likely already have an existing record at the root (`@`) and/or `www` from before — edit those rather than adding duplicates if so.
3. Set:
   - **Type**: A, **Name**: `@`, **Value**: the IP Vercel showed you, **TTL**: default
   - **Type**: CNAME, **Name**: `www`, **Value**: `cname.vercel-dns.com` (or whatever Vercel showed), **TTL**: default
4. Save. DNS propagation is usually fast (minutes) but can occasionally take up to a few hours.
5. Back in Vercel's Domains page, each domain shows "Invalid Configuration" until its DNS record is live, then flips to a green checkmark once detected — no action needed on your end beyond waiting.

## 5. Future deploys

Once this is wired up, every `git push` to the GitHub repo's main branch triggers a new Vercel deploy automatically. I can make code changes and push them (with your OK each time) and Vercel picks them up within a minute or two.

## Relationship to the simulator site

This site is the marketing/hub page at the bare domain; it links out to `play.leylinesofpower.com` (the actual game, a separate repo/Vercel project, already live) via the "Play Now" / "Begin your Ascension" buttons. The two are independent deploys — a push here never touches the simulator's deployment, and vice versa.
