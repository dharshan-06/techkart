# ⚡ SALESTORM — High-Concurrency Location-Aware Electronics Marketplace

> **Hackathon MVP & Production Architecture Specification**  
> *Discover. Reserve. Buy. Electronics directly from physical retail stores near you with a 0% overselling guarantee.*

---

## 🌟 Executive Summary

**SALESTORM** is a location-aware electronics marketplace connecting multi-branch physical electronics retailers with customers. Unlike standard classifieds or second-hand portals, SALESTORM provides **real-time branch-level inventory management**, **express local pickup**, **Haversine distance-based delivery calculations**, and an **Optimistic Concurrency Control (OCC) engine** capable of handling **10,000 concurrent user purchase requests against 100 inventory units with ZERO overselling**.

---

## 🚀 Key Features & Highlights

- 🏪 **Store-First & Product-First Shopping**: Browse products near your GPS location or explore physical store branches directly.
- ⚡ **10,000 User High-Concurrency OCC Engine**: PostgreSQL atomic conditional updates using monotonic `version` locking (`0% Overselling Guarantee`).
- 🛒 **Multi-Store Cart Splitting**: Group cart items by physical store; automatically split orders per branch with parallel atomic reservations.
- 📍 **Haversine Proximity Sorting**: Geolocation distance calculation sorting stores and assigning express local delivery fee tiers (< 5km, 5-15km, > 15km).
- ⌛ **Automated Cancellation Scheduler**: 15-minute unpaid reservation expiry & 24-hour COD pickup timeout engine.
- 💬 **Real-Time Customer ↔ Business Chat**: Direct store communication linked to specific products and orders.
- 🛡️ **Idempotent Transactions**: Unique idempotency keys for reservation, payment, and checkout requests.
- 📊 **Interactive Benchmark Panel**: Test 10,000 concurrent requests live at `/demo/concurrency`.

---

## 🛠️ Tech Stack & Architecture

- **Framework**: Next.js 14 (App Router, Server Actions, API Routes)
- **Language**: TypeScript (Strict type safety across domain logic)
- **Styling**: Tailwind CSS, Vanilla CSS Design System (`app/globals.css`)
- **Database**: Supabase (PostgreSQL with RLS policies, indexes, procedures)
- **Icons**: Lucide React
- **Architecture**: Modular Monolith with clean service boundaries for Microservices extraction.

---

## 📁 Project Directory Structure

```
SD-Hahathon/
├── app/
│   ├── api/                   # API Handlers
│   │   ├── products/          # Listing, search, & store stock breakdown
│   │   ├── stores/            # Store locator & proximity sorting
│   │   ├── search/            # Location-aware product search
│   │   ├── cart/              # Shopping cart management
│   │   ├── checkout/          # Multi-store checkout pipeline
│   │   ├── inventory/reserve/ # Atomic OCC inventory reservation
│   │   ├── payments/mock/     # Mock payment gateway with idempotency
│   │   ├── orders/            # Order status & cancellation
│   │   ├── conversations/     # Real-time customer-business messaging
│   │   ├── notifications/     # Notification service
│   │   └── demo/concurrency/  # 10,000 user benchmark simulation
│   ├── buy/                   # Electronics Marketplace Page
│   ├── stores/                # Nearby Stores & Store-First Shopping Page
│   ├── products/[id]/         # Product Detail with Multi-Store Inventory
│   ├── search/                # Search Results Page
│   ├── cart/                  # Multi-Store Cart Page
│   ├── checkout/              # Step-by-Step Checkout Page
│   ├── orders/                # Customer Order History & Tracking
│   ├── chat/                  # Customer ↔ Business Chat Page
│   ├── profile/               # Account & Role Switcher
│   ├── business/dashboard/    # Business Retailer Management Suite
│   ├── admin/                 # Platform Administration Dashboard
│   ├── demo/concurrency/      # Live Concurrency Benchmark Page
│   ├── layout.tsx             # Root App Layout with Navigation & Footer
│   ├── page.tsx               # Landing Page with Dual CTAs
│   └── globals.css            # Styling System & Color Tokens
├── components/                # Shared UI Components
│   ├── navbar.tsx             # Navigation header with Role Switcher & Cart Count
│   ├── footer.tsx             # Platform Footer & Policy Disclosures
│   ├── location-modal.tsx     # City & Pincode Geolocation Modal
│   └── stock-badge.tsx        # Live Stock Status Badges (🟢 🟡 🔴)
├── lib/
│   ├── db/
│   │   └── store.ts           # In-Memory Database Store with OCC Engine
│   ├── services/              # Domain Logic Services
│   │   ├── inventory.ts       # OCC reservation & stock locking
│   │   ├── checkout.ts        # Cart splitter & multi-store order orchestration
│   │   ├── cancellation.ts    # Auto-cancellation scheduler
│   │   ├── location.ts        # Haversine formula distance computation
│   │   ├── payments.ts        # Mock payment gateway with idempotency
│   │   ├── chat.ts            # Messaging service
│   │   └── notifications.ts   # Notification service
│   ├── types.ts               # TypeScript Domain Interfaces
│   └── seed-data.ts           # Seed Companies, Stores, Products, & Stock
├── docs/                      # Architectural Documentation & Diagrams
│   ├── architecture.md        # System Architecture & Microservices Plan
│   ├── ADR.md                 # Architectural Decision Records (ADR-001..006)
│   └── diagrams/              # 12 Renderable Mermaid Diagrams
└── supabase/
    └── schema.sql             # Full PostgreSQL Schema, Indexes, & OCC Procedures
```

---

## ⚡ Quick Start & Local Execution

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Environment Variables
Copy `.env.example` to `.env.local`:
```bash
cp .env.example .env.local
```

### 3. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🧪 High-Concurrency Benchmark Simulation

To run the 10,000 concurrent user purchase simulation against 100 inventory units:

1. Navigate to **[http://localhost:3000/demo/concurrency](http://localhost:3000/demo/concurrency)** in the web UI.
2. Set **Total Concurrent Requests**: `10000` and **Available Stock**: `100`.
3. Click **[ LAUNCH 10,000 USER BENCHMARK SIMULATION ]**.
4. Observe the results:
   - Total Requests: `10,000`
   - Successful Reservations: `100` (🟢)
   - Failed Requests: `9,900` (🔴)
   - Overselling Count: `0` (🛡️ **0% Overselling Guaranteed**)

---

## 📐 Documentation & System Design Architecture

Comprehensive design records are available in `/docs`:
- [`/docs/architecture.md`](file:///d:/SD-Hahathon/docs/architecture.md): Microservices strategy, Resilience, and Circuit Breaking.
- [`/docs/ADR.md`](file:///d:/SD-Hahathon/docs/ADR.md): Architectural Decision Records (ADR-001 through ADR-006).
- [`/docs/diagrams/`](file:///d:/SD-Hahathon/docs/diagrams/): 12 renderable Mermaid diagrams covering ERD, Checkout Sequence, OCC Lock Flow, and Cancellation Engine.

---

## 📜 License & Acknowledgements

Built for Hackathon Demonstration. Powered by Next.js, Supabase PostgreSQL, and Tailwind CSS.
