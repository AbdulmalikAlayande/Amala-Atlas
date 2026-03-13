# Amala Atlas

[![Amala Atlas](https://img.shields.io/badge/Amala-Atlas-orange.svg)](#)
[![Live](https://img.shields.io/badge/Live-amalatlas.vercel.app-brightgreen.svg)](https://amalatlas.vercel.app)

A community-driven location intelligence platform for discovering, mapping, and verifying **Amala food spots** across Nigeria. We specialize in finding obscure, undocumented, and informal locations that fly under the radar of mainstream directories like Google Maps.

## The Problem

Nigeria's best Amala spots are invisible online. They don't have websites, Google listings, or social media pages. They're known by word of mouth: "Iya Basira under the bridge in Mokola" or "that buka behind the market in Surulere." This knowledge lives in people's heads and WhatsApp groups — not on the internet.

## The Solution

Instead of scraping the web for data that doesn't exist, we go where the knowledge is: **the people who eat Amala every day**. Amala Atlas turns community knowledge into verified map data through multiple discovery channels, all feeding into a single verification pipeline.

## How It Works

```
User eats Amala  ->  Their spot is missing from the map  ->  They submit via WhatsApp/web
     -> Spot gets verified  ->  Appears on the map  ->  User shares "I added my spot"
     -> Friends see it  ->  Their spots are missing  ->  They submit  ->  Cycle repeats
```

## System Architecture

This project is organized as a monorepo hub with three components:

```
amala-atlas/
├── amala-atlas-backend-api/     # Django REST API — the system of record
├── amala-atlas-auto-discovery/  # Scrapy-based web crawler (archived)
└── amala-atlas-explorer/        # React frontend (separate repo)
```

| Component | Role | Tech | Repo |
|-----------|------|------|------|
| **[Backend API](./amala-atlas-backend-api)** | System of record — ingestion, verification, spot lifecycle | Django 5.x, DRF, PostgreSQL | [amala-atlas-backend-api](https://github.com/AbdulmalikAlayande/amala-atlas-backend-api) |
| **[Auto-Discovery](./amala-atlas-auto-discovery)** | Web crawler for bootstrapping data (archived — replaced by lightweight agents in the backend) | Scrapy, spaCy, SQLAlchemy | [amala-atlas-auto-discovery](https://github.com/AbdulmalikAlayande/amala-atlas-auto-discovery) |
| **Frontend** | Map UI for browsing, searching, and submitting spots | React, Vercel | [amala-atlas-explorer](https://github.com/AbdulmalikAlayande/amala-atlas-explorer) |

## Discovery Channels

Data enters the platform through multiple channels, each with a different trust level:

| Channel | How It Works | Trust Level | Verification Needed |
|---------|-------------|-------------|-------------------|
| **WhatsApp Bot** | Users text a spot name + location pin to a WhatsApp number | High (GPS proof) | 1 approval |
| **Web Form** | Users submit via the frontend at amalatlas.vercel.app | Medium | 1-2 approvals |
| **Google Maps Agent** | Automated scan of Google Places API for amala-related searches | High (Google-reviewed) | 1 approval |
| **Twitter Agent** | Monitors tweets mentioning amala spots with city context | Low | 2 approvals |
| **Seed Script** | One-time bootstrap from Nigerian food blogs | Low | 2 approvals |

## Candidate Lifecycle

Every discovered spot follows the same pipeline regardless of source:

```
Submission  ->  Candidate (scored + deduplicated)  ->  Verification Queue  ->  Spot (on the map)
```

1. **Submission**: Raw data enters the system (name, address, coordinates, photos)
2. **Candidate**: Scored based on evidence quality and source channel trust (0.0–1.0)
3. **Verification**: Human reviewers approve/reject with dynamic thresholds per channel
4. **Spot**: Promoted to the canonical map database

## Getting Started

Each component has its own setup instructions:

- **Backend API**: See the [Backend README](./amala-atlas-backend-api/README.md)
- **Auto-Discovery** (archived): See the [Auto-Discovery README](./amala-atlas-auto-discovery/README.md)
- **Frontend**: See the [Explorer repo](https://github.com/AbdulmalikAlayande/amala-atlas-explorer)

### Quick Start (Backend Only)

```bash
cd amala-atlas-backend-api
pip install -r requirements.txt   # or: pipenv install
cp .env.example .env              # configure DATABASE_URL, SECRET_KEY
python manage.py migrate
python manage.py runserver
```

## Deployment

- **Backend**: [Render](https://amala-atlas.onrender.com) (Django + PostgreSQL)
- **Frontend**: [Vercel](https://amalatlas.vercel.app) (React)

## Contributing

Contributions are welcome. If you know Amala spots that aren't on the map, the fastest way to contribute is to submit them through the platform.

For code contributions, pick a component repo, read its README, and open a PR.

---

*Amala Atlas — Mapping the flavor of Nigeria, one spot at a time.*
