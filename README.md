# Hyperlocal Influencer DB

An internal-style tool for discovering and managing hyperlocal creators across US cities — built to replace fragmented spreadsheet-based workflows with a shared, searchable, real-time database.

**Live app → [hyperlocal-stars-db.lovable.app](https://hyperlocal-stars-db.lovable.app)**

---

## System Architecture

![Hyperlocal Influencer DB — System Architecture](system-architecture.png)

---

## Problem

Many GTM and marketing teams track creators in large shared spreadsheets. While easy to start, this approach quickly becomes difficult to manage as the creator list grows.

Typical limitations include:

- No structured search across multiple attributes (city, niche, engagement)
- Manual tagging that becomes inconsistent across rows
- Limited collaboration when multiple team members edit simultaneously
- Difficulty adding new creators without breaking formatting
- Static records that go stale quickly

In this example workflow, a creator list had grown to **200+ hyperlocal influencers across 9 cities**, making discovery slow and forcing users to manually scroll through hundreds of rows to find relevant creators.

---

## Solution

A password-gated internal web app backed by a real database. Any team member with the link can search, filter, star, and submit new creators — and everyone sees updates in real time.

Instead of navigating rows in a spreadsheet, creators are stored as structured entities and surfaced through a searchable interface.

---

## Features

- **Grid view** — creator cards color-coded by city with name, handle, and notes
- **Stats view** — bar chart of creator count per city with summary metrics
- **Real-time search** — filters across name, handle, city, and tags instantly
- **City filter pills** — one-click filter per market
- **Add Creator form** — submissions persist to shared database immediately
- **Star/save** — flag priority creators for outreach
- **Password gate** — simple shared-secret auth for internal use

---

## Tech Stack

| Layer | Technology | Why |
|------|-----------|-----|
| Frontend | React (via Lovable) | Rapid iteration, no local dev environment needed |
| Database | Supabase (PostgreSQL) | Realtime websockets, free tier, Row Level Security |
| Hosting | Lovable / Vercel | Auto-deploy, free tier, shareable URL |
| Data seed | Python / pandas | Parsed original Google Sheet → JSON → Supabase |

---

## Data

- **210 creators** seeded from initial research
- **9 cities** — New York, Miami, Los Angeles, NOLA, Seattle, Austin, SF/Bay Area, Boston, Chicago
- Schema: `id · name · handle · city · description · tags · created_at`

---

## Data Model

Primary entity:

**Creator**

- id  
- name  
- handle  
- city  
- description  
- tags  
- created_at  

Potential future relationships:

Creator → Campaign  
Creator → Brand  
Creator → Agency  

---

## Known Limitations & Roadmap

| Status | Item |
|------|------|
| ✅ Live | Shared real-time database, city filtering, submission form |
| 🔜 Next | Per-user auth via Supabase magic link — adds audit trail (who added what) |
| 🔜 Next | Automated scraping via Supabase Edge Function + Apify/ScraperAPI — Collabstr top 100 per city |
| 📋 Later | Follower tier auto-tagging (micro / macro / mega) via bio parsing |
| 📋 Later | CSV export + Notion sync for GTM team handoff |
| 📋 Later | Campaign tracking — outreach status per creator |

---

## Product Decisions

**Why Supabase over Airtable** — Airtable hits rate limits on the free tier, costs $20/mo for team sharing, and makes UI customization difficult. Supabase gives a real PostgreSQL database with Realtime built in at no cost.

**Why a shared password over full auth** — For a small trusted team, magic link auth adds friction without meaningful security gain at this stage. The shared password is a known tradeoff; the upgrade path to Supabase Auth is straightforward when the team scales.

**Why a review queue for scraped entries (roadmap)** — Automated scrapers produce noisy data. A `pending_influencers` table lets a human approve before entries surface in the main DB, keeping data quality high.

---

## Disclaimer

This repository demonstrates a conceptual creator discovery system designed to illustrate how structured databases and tagging workflows can replace spreadsheet-based influencer tracking.

The implementation and example data are illustrative and do not represent any proprietary internal tools.

---

*Built with Lovable · Supabase · Claude · Python*
