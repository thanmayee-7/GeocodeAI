 is a sub-500ms location intelligence agent designed for e-commerce and last-mile delivery in India. It takes unstructured, free-text Indian addresses—full of landmark directions (*"opp Ganesh temple"*), informal colony names, regional spellings, and wrong pincodes—and resolves them into precise GPS coordinates.

# The Problem It Solves

Indian delivery partners handling 60–100 packages a day waste massive time making calls or circling blocks because existing map apps drop pins hundreds of meters off at generic pincode centroids.
# How It Works (The 4-Agent Pipeline)

1. **Multilingual Address Parser:** Instantly strips out door numbers, relative landmark phrases, sub-localities, and pincodes from raw text.
2. **DuckDB Ground-Truth Engine:** Validates or self-corrects pincodes against 150k+ All India Pincode Directory records in under 10ms.
3. **OSM Spatial Anchor Finder:** Queries live OpenStreetMap POIs (temples, banks, shops) in a local bounding box to anchor relative directions (*"opposite X"*) to exact coordinates.
4. **Self-Checking Confidence Engine:** Calculates a 0.0–1.0 confidence score and returns a transparent audit trail showing *why* the pin was placed there, or flags low-confidence results for driver verification
5. 
# Why It Wins
 Fast:** Runs end-to-end in **< 500ms** (fast enough for order placement, no overnight batch jobs).
Cheap:** Costs **< ₹0.02 per request** by relying on open-source spatial data (OSM) and lightweight local search rather than expensive commercial LLM calls.
DPDP Compliant:** Processing happens locally on India-based servers, dropping raw address strings from memory immediately after geocoding.
Transparent:** Never silently guesses—always returns a full audit log with evidence for every address correction.
┌────────────────────────────────────────────────────────────────────────┐
│                        RAW MESSY INDIAN ADDRESS                        │
│   "Flat 302, opp Ganesh temple, near SBI ATM, Mg road, 500038 hyd"     │
└─────────────────────────────────┬──────────────────────────────────────┘
│
▼
┌────────────────────────────────────────────────────────────────────────┐
│  AGENT 1: Multilingual Address Parser (< 120ms)                        │
│  • Extracts: Door No, Landmarks, Sub-locality, City, Pincode           │
└─────────────────────────────────┬──────────────────────────────────────┘
│
▼
┌────────────────────────────────────────────────────────────────────────┐
│  AGENT 2: DuckDB Pincode & Locality Ground-Truthing (< 80ms)           │
│  • Cross-checks All India Pincode Directory CSV in-memory             │
│  • Auto-corrects invalid or mismatched pincodes                        │
└─────────────────────────────────┬──────────────────────────────────────┘
│
▼
┌────────────────────────────────────────────────────────────────────────┐
│  AGENT 3: Overpass OSM Spatial Anchor Finder (< 200ms)                 │
│  • Bounding-box search around centroid for real POIs (temples, ATMs)   │
│  • Computes relative spatial offset coordinates                        │
└─────────────────────────────────┬──────────────────────────────────────┘
│
▼
┌────────────────────────────────────────────────────────────────────────┐
│  AGENT 4: Confidence Scoring & Self-Check (< 50ms)                      │
│  • Calculates Geocode Confidence Score (0.0 – 1.0)                     │
│  • Returns High-Confidence Geocode OR Flags for Field Verification     │
└────────────────────────────────────────────────────────────────────────┘
