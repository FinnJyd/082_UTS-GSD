# UTS Graf Sains Data - Graph-Based E-Commerce Recommender System (Neo4j)

This repository contains the complete implementation, analysis, and documentation for the **Mid-Term Examination (UTS) - Graph Data Science (Gasal 2026/2027)**, Undergraduate Data Science Program, Faculty of Mathematics and Natural Sciences, Universitas Negeri Surabaya.

---

## 👤 Student Profile
* **Name** : [Your Full Name]
* **Student ID (NIM)** : [Your NIM]
* **Class** : [Your Class / Parallel]
* **Course** : Graph Data Science (Graf Sains Data - 3 SKS)

---

## 📌 Project Overview & Dataset
This project models and analyzes implicit e-commerce user-item interactions (`view`, `addtocart`, `transaction`) from the **RetailRocket E-commerce Dataset** (Roman Zykov) using **Neo4j** graph database and Python.

### ⏱️ Selected Time Periods (Section 2.1 #1 & #2)
* **Training Fold (P1):** `2015-08-05` to `2015-08-13` (8 days, 142,768 events)
  * *Purpose:* Constructs the training interaction graph, Vector Space Model (VSM), and monopartite Item-Item similarity projection (`:SIMILAR_TO`).
* **Testing Fold / Ground Truth (P2):** `2015-08-13` to `2015-08-20` (7 days, 125,747 events)
  * *Purpose:* Serves as the ground truth hold-out fold to evaluate offline recommendation performance.
* **Overlapping Visitors:** 3,186 users shared between P1 and P2.

---

## 📊 Summary of Results

### 1. Graph Projection & Similarity (Section 2.1 #6, #7, #8)
* **Projection Analysis:** Proved that Item-Item projection is computationally superior to User-User projection due to catalog stability ($52,957$ items vs. $84,635$ users), lower dimensional sparsity ($99.9974\%$), and immunity to user cold-start.
* **Anchor Item 187946:** Calculated top 5 most similar items via topological Jaccard similarity in Cypher SQL and persisted back into Neo4j via `[:SIMILAR_TO {similarity: ...}]`.
* **Before vs. After Graph Visualization:** NetworkX side-by-side visualization contrasting the 2-hop bipartite interaction graph with the projected direct monopartite graph.

### 2. Analytics & Evaluation (Section 2.2 #1 & #2)
* **Target User:** Visitor ID `113137` (reproducibly sampled with `seed=42` from active overlapping users).
* **KNN Recommender ($k = 5$):** Generated Top-5 candidate items and identified the Top-3 advertised items (`288517`, `437746`, `422494`).
* **Offline Evaluation Metrics on Testing Fold (P2):**
  * **Hit@5:** **$1.0$ ($100\%$)** — Items `437746` (Rank 2) and `367490` (Rank 5) were verified in the user's test fold interactions.
  * **NDCG@5:** **$0.6241$ ($62.41\%$)** — Demonstrates strong ranking quality placing relevant items at the top of the recommendation list.

---

## 📁 Repository Structure

```text
├── data/
│   ├── events.csv                  # Raw RetailRocket events dataset (gitignored)
│   ├── events_p1_train.csv         # Filtered training interactions (P1)
│   └── events_p2_test.csv          # Filtered testing interactions (P2)
├── Prologue.ipynb                  # Section 1: Neo4j setup, driver verification & checkpoint answers
├── UTS.ipynb                       # Section 2: Complete exam solutions (EDA, VSM, Similarity, Analytics)
├── requirements.txt                # Python dependencies (neo4j, pandas, scipy, scikit-learn, networkx, matplotlib)
├── .env.example                    # Environment variable template for Neo4j credentials
├── .gitignore                      # Git exclusion rules
└── README.md                       # Main documentation
```

---

## 🚀 Setup & Execution

1. **Environment Setup:**
   ```bash
   uv venv --python 3.10
   .venv\Scripts\activate
   pip install -r requirements.txt
   ```

2. **Configure Neo4j Connection:**
   Copy `.env.example` to `.env` and configure your credentials:
   ```env
   NEO4J_URI=neo4j://localhost:7687
   NEO4J_USER=neo4j
   NEO4J_PASSWORD=your_password
   ```

3. **Run Notebooks:**
   * Open `Prologue.ipynb` to verify preliminary setup and database connection.
   * Open `UTS.ipynb` and select kernel `Python (GSD UTS)` to run the entire exam pipeline.
