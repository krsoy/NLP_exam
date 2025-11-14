Project Link:https://github.com/krsoy/NLP_exam
Video Link:https://youtu.be/W1t9-KhkqEw

# 📘 Semantic Knowledge Graph of S&P 500 Earnings Call Transcripts (2024)
### Network Analysis & Topic Extraction using LLM + UMAP + Bipartite Graphs

---

## 📑 1. Dataset and Subset Choice
We use the https://huggingface.co/datasets/kurry/sp500_earnings_transcripts dataset from HuggingFace, which contains machine-transcribed earnings call dialogues for S&P 500 companies.

### Why this dataset?
- Broad coverage of US public companies  
- Covers CEO/CFO statements + analyst Q&A  
- Suitable for topic extraction, semantic similarity, and network analysis  

### Subset used (2024 only)
We filter to **2024 transcripts**, optionally to single companies (e.g., PayPal) for demonstration.

#### Rationale
- Ensures temporal consistency  
- Reflects current economic climate  
- Avoids outdated noise  
- Keeps computational workload manageable  

---

## 🔍 2. Extraction Pipeline: From Text → Topics → Knowledge Graph

### 2.1 Cleaning
- Remove greetings: “good morning”, “afternoon”, “thank you”  
- Remove structural words: “slide”, “operator”, “please proceed”  
- Remove transcription noise  
- Merge all utterances per company into one **company document**  

### 2.2 Topic Extraction (TF–IDF + Semantic Filtering)
Use:

```
TfidfVectorizer(ngram_range=(1,2), max_features=5000)
```

Custom filtering removes noise topics and keeps true business concepts:

✔ revenue, margin, cash flow  
✔ supply chain, inventory, demand  
✔ cloud, AI, semiconductors  
✔ power, LNG, megawatts  
✔ occupancy, NOI, REIT metrics  

❌ “good morning”, “slide”, “operator”, “adjusted”, etc.

---

## 🧱 3. Knowledge Graph Construction

### Bipartite Graph Structure  
**Companies ↔ Topics**

- Company nodes include ticker, name, first/last year  
- Topic nodes are meaningful n-grams  
- Edge weight = TF–IDF score  
- Edge attributes include time range  

This forms a structured semantic knowledge graph.

---

## 🔁 4. Network Projections & Analysis

### 4.1 Company–Company Similarity Network
Project bipartite graph → one-mode company network:

- Edge weight = number of shared topics or TF–IDF similarity  
- Reveals firms with similar thematic emphasis  

### 4.2 Topic–Topic Co-occurrence Network
Project topics → one-mode topic network:

- Edge weight = number of shared companies  
- Helps detect topic bundles (AI + cloud + data center)

---

## 🌐 5. Embedding & Clustering

### UMAP Embedding
```
UMAP(n_components=2, metric="cosine")
```

Used to:
- Visualize semantic distances  
- Identify natural clusters  
- Provide coordinates for network visualization  

### KMeans / HDBSCAN Clustering
Clusters correspond to:
- AI/cloud infra companies  
- Retail/consumer companies  
- Financial institutions  
- Healthcare/biotech  
- Energy/utilities  

---

## 📈 6. Visualization

Using **Plotly (interactive)**:
- Bipartite graph (companies ↔ topics)  
- Company–company similarity graph  
- Topic–topic co-occurrence graph  
- UMAP cluster plot  

Features:
- Hover tooltips  
- Zoom & pan  
- Color-coded clusters  

---

## 🧠 7. Main Findings

### Major insights:
1. **Clear sector-based clusters**  
   - AI/compute: NVDA, AMD, MSFT  
   - Retail: COST, WMT, TGT  
   - Banking: JPM, BAC, C  
   - Energy/utilities: grid, power, LNG topics  

2. **AI-related topics dominate tech calls**  
3. **Inventory and traffic signal consumer weakness**  
4. **Interest rates drive financial sector themes**  
5. **Co-occurrence networks show topic bundles**  
   - (“AI”, “cloud”, “data center”)  
   - (“inventory”, “traffic”, “consumer demand”)  
   - (“margin”, “productivity”, “cost savings”)  

---

## ⚠️ 8. Limitations

- ASR transcription noise  
- TF–IDF cannot capture deep semantics  
- Business-topic filtering is heuristic  
- Projections can be dense; thresholding required  
- Only 2024 included; no long-term trend analysis  
- No sentiment attribution per topic  
- No speaker-role distinction (CEO vs Analyst)  

---

## 🏁 9. Conclusion
This project shows how to transform unstructured earnings call transcripts into a **semantic knowledge graph**, enriched by topic extraction, embeddings, network projections, and interactive visualization.  
The approach reveals sector patterns, company similarity, and thematic structures across the S&P 500.

