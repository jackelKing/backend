# RIFT 2026 – Money Muling Detection Engine

RIFT (Real-time Illicit Fund Tracking) is a graph-based fraud detection engine designed to identify complex money laundering behaviors in transactional datasets.

This system detects:

1. Circular Fund Routing (Cycle-based laundering)
2. Smurfing Patterns (Fan-in / Fan-out aggregation)
3. Layered Shell Networks (Pass-through laundering chains)
4. ML-based behavioral anomalies

Built for scalability, accuracy, and competition-grade performance.

---

## 🔎 Core Detection Capabilities

### 1️⃣ Circular Fund Routing (Cycles)
Money moves in loops to obscure origin.

Example:
A → B → C → A

• Detects cycles of length 3 to 5  
• All members flagged as part of the same fraud ring  
• Structural risk scoring applied  

---

### 2️⃣ Smurfing Patterns (Fan-in / Fan-out)

Fan-in:
Many accounts → single aggregator within 72 hours

Fan-out:
Single account → many receivers within 72 hours

• Threshold: 10+ unique nodes  
• Temporal validation (≤ 72 hours)  
• Ring-level flagging  

---

### 3️⃣ Layered Shell Networks

Funds pass through intermediate low-activity accounts.

Detection rules:
• Chains of 3 hops  
• Intermediate accounts have ≤ 3 total transactions  
• Pass-through behavior (in-degree + out-degree)

---

### 4️⃣ ML Anomaly Detection

Node-level feature engineering includes:

• Degree metrics  
• Transaction counts  
• Temporal velocity  
• Structural signals  

Isolation-based anomaly scoring highlights behavioral outliers.

---

## 🏗 Architecture Overview

backend/
│
├── app/
│   ├── main.py
│   ├── core/
│   │   ├── graph_builder.py
│   │   ├── cycle_detector.py
│   │   ├── smurf_detector.py
│   │   ├── shell_detector.py
│   │   ├── feature_engineering.py
│   │   ├── scoring_model.py
│   │   └── orchestrator.py
│   │
│   ├── repositories/
│   │   └── analysis_repo.py
│   │
│   ├── models/
│   │   └── db_models.py
│   │
│   └── core/
│       └── database.py
│
├── requirements.txt
└── README.md

System Flow:

1. CSV Upload
2. Graph Construction (Directed Graph)
3. Structural Pattern Detection
4. ML Scoring
5. Ring Consolidation
6. Risk Scoring
7. Database Logging
8. API Response

---

## ⚙️ Installation

Create Environment:

conda create -n rift python=3.11
conda activate rift

Install Dependencies:

pip install -r requirements.txt

---

## 🚀 Run Server

uvicorn app.main:app --reload --port 8037

Access API docs:
http://127.0.0.1:8037/docs

---

## 📤 API Usage

POST /analyze

Upload a CSV file with columns:

sender_id  
receiver_id  
amount  
timestamp  

Example:

curl -X POST "http://127.0.0.1:8037/analyze" \
  -H "accept: application/json" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@dataset.csv"

---

## 📊 Response Structure

{
  "suspicious_accounts": [...],
  "fraud_rings": [...],
  "summary": {
    "total_accounts_analyzed": 0,
    "suspicious_accounts_flagged": 0,
    "fraud_rings_detected": 0,
    "processing_time_seconds": 0
  }
}

---

## 🗄 Database Logging

Each analysis run stores:

• Graph ID  
• Risk score (number of flagged accounts)  
• Cycle count  
• Smurf count  
• Shell count  
• Timestamp  

SQLite database: rift.db

---

## 🧠 Design Philosophy

RIFT combines:

Graph theory  
Temporal analysis  
Behavioral anomaly detection  
Ring-level aggregation  
Weighted risk scoring  

The system prioritizes:

• Precision over noise  
• Structural fraud detection  
• Scalable architecture  
• Modular detector design  

---

## 🏆 Competitive Strengths

• Multi-pattern detection  
• Ring-level intelligence  
• ML + Structural hybrid scoring  
• Temporal validation  
• Fast graph-based architecture  
• Easily extensible  

---

## 📌 Future Enhancements

• Dynamic threshold calibration  
• Streaming transaction support  
• Ring evolution tracking  
• Frequent fraudster profiling  
• Risk dashboard  

---

## 📜 License

Competition / Educational Use

Built with precision for high-stakes financial anomaly detection.
