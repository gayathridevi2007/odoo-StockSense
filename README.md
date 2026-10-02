# 🚚 StockSense — Outbound Fulfillment & Customer Delivery Operations Engine

[![Production](https://img.shields.io/badge/Production-Live%20on%20Vercel-success?style=for-the-badge&logo=vercel)](https://odoo-omega.vercel.app/operations/deliveries)
[![Demo Video](https://img.shields.io/badge/Demo_Video-Watch_Walkthrough-E50914?style=for-the-badge&logo=googledrive)](https://drive.google.com/file/d/1LTevcDiSRAGjvqE-0zIf-cpEfyf7eDIm/view?usp=sharing)
[![Next.js](https://img.shields.io/badge/Next.js-16.3-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon%20Serverless-336791?style=for-the-badge&logo=postgresql)](https://neon.tech)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748?style=for-the-badge&logo=prisma)](https://www.prisma.io/)

> **Odoo x GCET Hyderabad Hackathon 2026 | Team 4**  
> **Lead Developer & Module Owner:** **Gayathri Devi M S** ([@gayathridevi2007](https://github.com/gayathridevi2007))  
> **Specialization:** Outbound Fulfillment, 3-Step Pick/Pack/Ship Pipeline & Negative-Stock Prevention Guards  

---

## 🌐 Live Module Deployment & Demo Video

🚀 **Live Module Direct URL:** **[https://odoo-omega.vercel.app/operations/deliveries](https://odoo-omega.vercel.app/operations/deliveries)**  
🏢 **Enterprise Platform URL:** **[https://odoo-omega.vercel.app](https://odoo-omega.vercel.app)**  
🎥 **Demo Video Walkthrough:** **[Watch on Google Drive](https://drive.google.com/file/d/1LTevcDiSRAGjvqE-0zIf-cpEfyf7eDIm/view?usp=sharing)**  

### 🔑 Demo Login Credentials
| Role | Email | Password | What Gayathri Built Here |
|---|---|---|---|
| **Warehouse Staff** | `staff@stocksense.dev` | `StockSense!1` | **Deliveries Console**, Pick/Pack/Validate pipeline, Printable Delivery Slips |
| **Inventory Manager** | `manager@stocksense.dev` | `StockSense!1` | Full administrative control & delivery dispatch approvals |

---

## 🎯 Executive Overview

Outbound logistics is the **final and most critical frontier** of any enterprise supply chain. A customer delivery failure—whether caused by an oversold product, picking errors, or shipping discrepancies—directly damages company reputation and bottom-line revenue.

As part of **Team 4**, this repository represents the **Outbound Fulfillment & Customer Delivery Engine** for StockSense. It provides a fault-tolerant, Odoo-compliant delivery workflow that guarantees stock availability, enforces multi-step verification (`Pick ➔ Pack ➔ Validate`), executes atomic double-entry stock reductions, and generates print-ready physical Delivery Slips for logistics carriers.

---

## 🏗️ Architecture & 3-Step Outbound Fulfillment Pipeline

```
  [ Sales Order / Customer Demand ]
                 │
                 ▼
  ┌────────────────────────────────────────────────────────┐
  │ 1. DRAFT & AVAILABILITY CHECK                          │
  │    - Auto-assigned Outbound Sequence: WH1/OUT/0000X    │
  │    - Real-time stock reserve check                     │
  │    - Shortage protection prevents overselling          │
  └──────────────────────────┬─────────────────────────────┘
                             │
                             ▼
  ┌────────────────────────────────────────────────────────┐
  │ 2. PICK & PACK WORKFLOW (Station Progression)          │
  │    - Pick: Warehouse runner retrieves items from bin   │
  │    - Pack: Items boxed & verified at dispatch table    │
  │    - State transition: Draft ➔ Waiting ➔ Ready         │
  └──────────────────────────┬─────────────────────────────┘
                             │
                             ▼
  ┌────────────────────────────────────────────────────────┐
  │ 3. VALIDATION (Atomic Stock Debit)                     │
  │    - Source: "WH1/Stock" (Physical Main Storage)       │
  │    - Destination: "Partner Locations/Customers"        │
  │    - Quant balance decremented with row-level locks    │
  │    - Immutable StockMove record appended to Ledger     │
  └──────────────────────────┬─────────────────────────────┘
                             │
                             ▼
  ┌────────────────────────────────────────────────────────┐
  │ 4. DISPATCH DOCUMENTATION                              │
  │    - Carrier Delivery Slip with @media print layout    │
  │    - Customer address, line item breakdown & signature │
  └────────────────────────────────────────────────────────┘
```

---

## 🚀 Key Features Built by Gayathri Devi M S

### 1. Delivery Operations Console (`/operations/deliveries`)
- **Real-Time Operational Visibility:** Filter delivery orders by state (`Draft`, `Waiting`, `Ready`, `Done`, `Cancelled`) and target customer partner.
- **Shortage Warning Badges:** Instant visual alert if requested quantities exceed currently available stock on hand.

### 2. Multi-Step Dispatch Station Progression (`/operations/deliveries/[id]`)
- **Step 1 — Pick:** Confirms products have been collected from storage bins.
- **Step 2 — Pack:** Confirms packaging at the shipping station.
- **Step 3 — Validate:** Executes atomic stock reduction into customer hands.
- **Cancel Safeguard:** Allows clean cancellation before goods leave the warehouse dock.

### 3. Strict Concurrency & Oversell Protection Guard
- Every delivery validation verifies `stock_quants.quantity >= requested_quantity`.
- Uses PostgreSQL atomic row-locking to ensure two simultaneous orders cannot oversell the same unit.

### 4. Enterprise Printable Delivery Slip (`/operations/deliveries/[id]/print`)
- Dedicated printable route tailored for warehouse thermal and laser printers.
- Fully styled with CSS `@media print` rules: hides navigation, headers, and UI chrome, leaving only the clean invoice/slip.
- Includes Carrier Reference, Source Document, Delivery Address, Itemized Table, and Dispatch Sign-off Box.

---

## 📂 Core Files Authored

| File | Purpose |
| :--- | :--- |
| `app/(app)/operations/deliveries/page.tsx` | Main Outbound Deliveries list, status tabs, and customer filters |
| `app/(app)/operations/deliveries/new/page.tsx` | Delivery Order creation interface with customer selector & stock availability check |
| `app/(app)/operations/deliveries/[id]/page.tsx` | Outbound dispatch inspector, multi-step Pick/Pack/Validate action console |
| `app/(app)/operations/deliveries/[id]/print/page.tsx` | High-fidelity printable Delivery Slip with `@media print` CSS layout |
| `app/api/pickings/[id]/pick/route.ts` | Endpoint for marking items as picked from storage |
| `app/api/pickings/[id]/pack/route.ts` | Endpoint for recording packaging completion |
| `app/api/pickings/[id]/validate/route.ts` | Core double-entry delivery execution: `WH1/Stock` ➔ `Customers` |
| `lib/stock/availability.ts` | Real-time stock reservation and shortage verification engine |

---

## 🧠 Evaluator Q&A Cheatsheet (For Harshil Patel `hapt`)

When defending this module before Odoo evaluators, here are the exact architectural decisions implemented:

### Q1: How does your module support Odoo's 1-step vs 2-step vs 3-step delivery routes?
> **Answer:**  
> *"In our architecture, we designed modular state endpoints: `/pick`, `/pack`, and `/validate`. For high-volume warehouses, operators can advance orders sequentially through Pick and Pack stages. For expedited shipments, a warehouse supervisor can execute direct 1-step validation. In all cases, stock ledger reduction is deferred until the final Validate step."*

### Q2: What prevents two customers from purchasing and validating the last unit of stock at the same time?
> **Answer:**  
> *"We implement atomic database isolation during picking validation. The query locks the product's quant record in the source location (`WH1/Stock`) before decrementing. If the quantity drops below zero, the transaction rolls back with a strict `ShortageError`, preventing overselling under high concurrency."*

### Q3: What is the destination location for an Outbound delivery in Odoo's double-entry model?
> **Answer:**  
> *"Under Odoo's double-entry rules, stock cannot vanish. Outbound deliveries move stock from `WH1/Stock` to the virtual location `Partner Locations/Customers`. This reduces physical warehouse assets while recording the balance on customer hands."*

### Q4: How is the physical Delivery Slip generated without heavy third-party PDF dependencies?
> **Answer:**  
> *"We implemented a native Next.js printable view at `/operations/deliveries/[id]/print` using CSS `@media print` directives. When `window.print()` triggers, it strips away UI navigation bars, optimizes contrast for thermal/laser printers, and prints a crisp, carrier-compliant packing slip instantly."*

---

## 🛠️ Tech Stack

- **Framework:** Next.js 15 (App Router), React 19, TypeScript
- **Styling:** Tailwind CSS & Custom Print Media Query Engine
- **Database & ORM:** PostgreSQL (Neon Serverless) & Prisma ORM
- **Authentication & RBAC:** NextAuth.js
- **Architecture:** Odoo Double-Entry Stock Principles

---

## ⚡ Live Verification

Click to test the live deployed Outbound Deliveries engine directly:
👉 **[https://odoo-omega.vercel.app/operations/deliveries](https://odoo-omega.vercel.app/operations/deliveries)**

---

*Engineered with precision for the Odoo x GCET Hackathon 2026.*
