# Hyperlocal Influencer DB

An internal tool for discovering and managing hyperlocal creators across US cities — built to replace a fragmented Google Sheet workflow with a shared, searchable, real-time database.

**Live app → [hyperlocal-stars-db.lovable.app](https://hyperlocal-stars-db.lovable.app)**

---

## System Architecture

![Hyperlocal Influencer DB — System Architecture](system-architecture.png)
---

## Problem

The team was tracking 200+ hyperlocal influencers across 9 cities in a Google Sheet. It had no search, no shared access control, no way to add entries collaboratively, and went stale immediately. Anyone wanting to find a food creator in Boston had to scroll manually through 200 rows.

## Solution

A password-gated internal web app backed by a real database. Any team member with the link can search, filter, star, and submit new creators — and everyone sees updates in real time.

## Features

- **Grid view** — creator cards color-coded by city with name, handle, and notes
- **Stats view** — bar chart of creator count per city with summary metrics
- **Real-time search** — filters across name, handle, city, and tags instantly
- **City filter pills** — one-click filter per market
- **Add Creator form** — submissions persist to shared database immediately
- **Star/save** — flag priority creators for outreach
- **Password gate** — simple shared-secret auth for internal use

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend | React (via Lovable) | Rapid iteration, no local dev environment needed |
| Database | Supabase (PostgreSQL) | Realtime websockets, free tier, Row Level Security |
| Hosting | Lovable / Vercel | Auto-deploy, free tier, shareable URL |
| Data seed | Python / pandas | Parsed original Google Sheet → JSON → Supabase |

## Data

- **210 creators** seeded from original internal research
- **9 cities** — New York, Miami, Los Angeles, NOLA, Seattle, Austin, SF/Bay Area, Boston, Chicago
- Schema: `id · name · handle · city · description · tags · created_at`

## Known Limitations & Roadmap

| Status | Item |
|--------|------|
| ✅ Live | Shared real-time database, city filtering, submission form |
| 🔜 Next | Per-user auth via Supabase magic link — adds audit trail (who added what) |
| 🔜 Next | Automated scraping via Supabase Edge Function + Apify/ScraperAPI — Collabstr top 100 per city |
| 📋 Later | Follower tier auto-tagging (micro / macro / mega) via bio parsing |
| 📋 Later | CSV export + Notion sync for GTM team handoff |
| 📋 Later | Campaign tracking — outreach status per creator |

## Product Decisions

**Why Supabase over Airtable** — Airtable hits rate limits on the free tier, costs $20/mo for team sharing, and makes UI customization difficult. Supabase gives a real PostgreSQL database with Realtime built in at no cost.

**Why a shared password over full auth** — For a small trusted team, magic link auth adds friction without meaningful security gain at this stage. The shared password is a known tradeoff; the upgrade path to Supabase Auth is one prompt away when the team scales.

**Why a review queue for scraped entries (roadmap)** — Automated scrapers produce noisy data. A `pending_influencers` table lets a human approve before entries surface in the main DB, keeping data quality high.

---

*Built with Lovable · Supabase · Claude · Python*
