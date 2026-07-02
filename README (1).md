# 🚂 India Rail Navigation System

A real-time railway navigation system for India built with C++, Node.js, and Gradio.

## Architecture

```
C++ Core Engine          Node.js API Layer         Gradio Frontend
─────────────────        ─────────────────         ───────────────
Graph (Dijkstra)    →    REST endpoints       →    User search UI
BFS (nearby/paths)  →    SQLite database      →    Booking panel
Seat management     →    Child process bridge →    Admin dashboard
CSV data loading    →    CORS middleware      →    PNR checker
Route history log   →    JSON responses      →    Fare calculator
```

## Project Structure

```
rail-nav/
├── cpp/
│   ├── graph.h          # Graph data structures & class declaration
│   ├── graph.cpp        # Dijkstra, BFS, fare calculation, CSV loader
│   ├── main.cpp         # CLI interface (called by Node.js)
│   ├── stations.csv     # 25 major Indian stations
│   └── edges.csv        # Rail connections with distance/fare/train data
├── api/
│   ├── server.js        # Express server
│   ├── db.js            # SQLite setup & seeding
│   ├── package.json
│   └── routes/
│       ├── search.js    # Route search endpoints (calls C++ binary)
│       ├── bookings.js  # Ticket booking & PNR management
│       └── trains.js    # Train CRUD (admin)
├── frontend/
│   └── app.py           # Gradio UI (3 tabs: Search, Booking, Admin)
├── build.sh             # One-command build script
└── README.md
```

## Quick Start

```bash
chmod +x build.sh
./build.sh

# Terminal 1
cd api && node server.js

# Terminal 2
cd frontend && python app.py
```

## API Endpoints

### Search
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/search/route?from=NDLS&to=MMCT&class=SL` | Shortest route (Dijkstra) |
| GET | `/api/search/nearby?station=NDLS&maxDistance=300` | Nearby stations (BFS) |
| GET | `/api/search/alternatives?from=NDLS&to=MMCT&k=3` | K alternate paths (BFS) |
| GET | `/api/search/fare?distance=500&class=3A` | Fare calculation |
| GET | `/api/search/stations` | List all stations |
| GET | `/api/search/history` | Search history log |

### Bookings
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/bookings` | Book a ticket |
| GET | `/api/bookings/pnr/:pnr` | Check PNR status |
| GET | `/api/bookings` | List all bookings |
| DELETE | `/api/bookings/:pnr` | Cancel booking |

### Trains (Admin)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/trains` | List all trains |
| POST | `/api/trains` | Add a train |
| PUT | `/api/trains/:id/seats` | Update seat count |
| DELETE | `/api/trains/:id` | Remove a train |

## C++ CLI Usage

```bash
cd cpp
./railnav route "New Delhi" "Mumbai Central" SL
./railnav nearby "New Delhi" 400
./railnav alternatives "New Delhi" "Chennai Central" 3
./railnav fare 1000 3A
./railnav stations
```

## Travel Classes & Fares

| Class | Description | Rate |
|-------|-------------|------|
| GEN | General | ₹0.4/km + ₹20 |
| SL | Sleeper | ₹0.5/km + ₹30 |
| CC | Chair Car | ₹0.9/km + ₹50 |
| 3A | AC 3-Tier | ₹1.2/km + ₹100 |
| 2A | AC 2-Tier | ₹1.8/km + ₹150 |
| 1A | AC First | ₹3.0/km + ₹250 |

## Stations Covered (25)

Delhi, Mumbai, Chennai, Kolkata, Bangalore, Hyderabad, Ahmedabad, Pune,
Jaipur, Lucknow, Bhopal, Nagpur, Patna, Guwahati, Bhubaneswar,
Amritsar, Chandigarh, Visakhapatnam, Varanasi, Jammu, Udaipur,
Indore, Kanpur, Prayagraj, Jodhpur

## Tech Stack

- **C++17** — Graph engine with Dijkstra & BFS, STL containers, file I/O
- **Node.js + Express** — REST API, child_process bridge to C++
- **SQLite (better-sqlite3)** — Bookings, trains, route history
- **Gradio** — Web UI with 3 tabs: Route Search, Booking, Admin Panel
- **Python requests** — Gradio → Node.js API communication
