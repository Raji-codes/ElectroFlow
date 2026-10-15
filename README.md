## 🏗️ System Architecture

ElectroFlow is built on an event-driven, full-stack architecture combining a reactive React 19 frontend, an interactive 3D WebGL Digital Twin, real-time WebSocket state synchronization, and Google Gemini AI multimodal intelligence.

```mermaid
graph TB
    subgraph ClientLayer["🖥️ Frontend & Presentation Layer (React 19 + TypeScript)"]
        UI["Modern Dashboard & Analytics<br/><i>(Tailwind CSS v4 + Framer Motion)</i>"]
        InvUI["Categorized Expandable Inventory<br/><i>(Stock Level Indicators, SKU Management)</i>"]
        OrderUI["Order Lifecycle Tracker<br/><i>(Pending / Processing / Shipped / Delivered)</i>"]
        Twin3D["3D Warehouse Digital Twin<br/><i>(Three.js + React Three Fiber + Drei)</i>"]
        AIChat["AI Co-Pilot & Logistics Advisor<br/><i>(React-Markdown + Remark-GFM)</i>"]
    end

    subgraph CommLayer["⚡ Communication & Event Layer"]
        HTTP["REST API Client<br/><i>(Fetch / Express Routes)</i>"]
        WSClient["WebSocket Client<br/><i>(Real-Time State Listener)</i>"]
    end

    subgraph ServerLayer["⚙️ Backend Server (Node.js + Express + TSX)"]
        Router["Express API Router<br/><i>(/api/inventory, /api/orders, /api/stats, /api/db)</i>"]
        WSServer["WebSocket Server <i>(ws)</i><br/><i>(Live Inventory & Order Broadcasts)</i>"]
        AuthMid["Session & RBAC Middleware<br/><i>(Admin / Manager / Staff Roles)</i>"]
    end

    subgraph AIEngine["🧠 AI & Intelligence Layer (Google Gemini)"]
        GeminiFlash["Gemini 2.5/3 Flash<br/><i>(Real-Time Assistant & Strategic Chat)</i>"]
        GeminiMulti["Gemini Multimodal Vision<br/><i>(Warehouse Blueprint & CAD Layout Analyzer)</i>"]
        GoogleSearch["Google Search Grounding<br/><i>(Live Market Trends & Hardware Pricing)</i>"]
    end

    subgraph DataLayer["💾 Persistence & Storage Layer"]
        DB[(SQLite Database<br/><i>better-sqlite3 / WAL Mode</i>)]
        subgraph Tables["Schema Tables"]
            T1["products"]
            T2["orders & order_items"]
            T3["order_tracking"]
            T4["transactions & inventory_logs"]
            T5["users & roles"]
        end
    end

    %% Client Interactions
    UI --> HTTP
    InvUI --> HTTP
    OrderUI --> HTTP
    AIChat --> HTTP
    WSClient <--> WSServer
    Twin3D -.-> |Renders Zones & Racks| InvUI

    %% Client <-> Backend
    HTTP --> Router
    Router --> AuthMid

    %% Backend <-> Database
    AuthMid --> DB
    DB --- Tables
    Router --> |Trigger Broadcasts| WSServer
    WSServer -.-> |Live Updates| WSClient

    %% AI Integration
    AIChat --> GeminiFlash
    GeminiFlash --> GoogleSearch
    Router --> |Blueprint Uploads| GeminiMulti
    GeminiMulti --> |Structured JSON Zones| Twin3D
