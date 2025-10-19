# MCA-Insights-Engine

# MCA Insights Engine

## Project Overview
The **MCA Insights Engine** is an end-to-end data pipeline and interactive dashboard designed to process, analyze, and visualize corporate master data from the **Ministry of Corporate Affairs (MCA)**.

It consolidates data from multiple states, detects daily changes, enriches company records, and presents insights through a user-friendly **Streamlit dashboard** featuring advanced search, filters, and chatbot capabilities.

This project serves as a **representative implementation of a corporate data management and intelligence platform**.

---

## Architecture
The project follows a simple, scalable three-layer architecture:

### 1. Data Ingestion & Processing (Python/Pandas)
- Executed in **Google Colab**.
- Loads, merges, cleans, and normalizes raw MCA data.
- Simulates daily changes and produces enriched datasets.

### 2. Data Storage (Google Drive / CSV)
- All raw, intermediate, and final data are stored as **CSV** files in Google Drive.
- Ensures seamless access for both Colab scripts and Streamlit.

### 3. Presentation Layer (Streamlit)
- An interactive Streamlit dashboard that supports:
  - Multi-select filtering (State, Year, Company Status)
  - Text-based search
  - Change-history view by CIN
  - Rule-based chatbot answering dataset questions

---

## Workflow
A sequential pipeline where each task’s output becomes the next task’s input:

### **Task A – Data Integration & Normalization**
- Load five state-wise CSV files (`Maharashtra`, `Delhi`, `Gujarat`, `Karnataka`, `Tamil Nadu`).
- Concatenate and standardize column names, data types, and remove duplicates.
- Output → `mca_master_dataset.csv`

### **Task B – Change Detection**
- Treat the master dataset as “Day 1”.
- Simulate “Day 2” by adding/modifying records.
- Use **MD5 hashing** to detect additions, deletions, and updates.
- Output → `change_log.csv`

### **Task C – Data Enrichment**
- Randomly select CINs from `change_log.csv`.
- Apply simulated enrichment (see below).
- Output → `enriched_cin_data.csv`

### **Task D & E – Dashboard and Chatbot**
- Launch `app.py` via Streamlit.
- Load all three CSV files and provide interactive exploration and chatbot Q&A.

---

## Setup & Installation

### **Prerequisites**
- Google Account with **Drive access**
- Five raw MCA CSV files
- Google Colab environment
- **ngrok** account (for public Streamlit URL)

---

### **1 – Data Setup**
1. Create a folder `MCA_Project` in your Google Drive.
2. Inside it, create `MCA_Project_Data`.
3. Upload the five state-wise CSV files there.

---

### **2 – Running the Scripts (Google Colab)**

**Mount Google Drive**
```python
from google.colab import drive
drive.mount('/content/drive')
```

**Merge Datasets**
Run the merge cell to combine all five CSVs → creates `mca_master_dataset.csv`.

**Generate Change Log**
Run the change-detection script → creates `change_log.csv`.

**Generate Enriched Data**
Run the enrichment script → creates `enriched_cin_data.csv`.

---

### **3 – Launching the Streamlit Dashboard**

**Step 1 – Update File Paths**
```python
master_data = '/content/drive/MyDrive/MCA_Project_Data/mca_master_dataset.csv'
change_log = '/content/drive/MyDrive/MCA_Project_Data/change_log.csv'
enriched_data = '/content/drive/MyDrive/MCA_Project_Data/enriched_cin_data.csv'
```

**Step 2 – Save the App Code**
```python
%%writefile app.py
```

**Step 3 – Run ngrok to Expose Streamlit**
```bash
!streamlit run app.py & npx localtunnel --port 8501
```

**Step 4 – View Dashboard**
Click the generated **public URL** to open and interact with the dashboard.

---

##  Enrichment Logic
The enrichment task augments company records with additional attributes such as **director names**.

### 1. **Web Scraping (requests + BeautifulSoup)**
- Tried scraping public data (ZaubaCorp).
 Blocked with **403 Forbidden** (anti-bot protection).

### 2. **Headless Browsing (Selenium)**
- Simulated a browser using headless Chrome.
 Blocked by **Cloudflare JavaScript challenge**.

### 3. **Official API (API Setu)**
- Attempted to register for official government API access.
 Denied – restricted to organizational accounts.

### 4. **Final Implementation – Simulation (Faker Library)**
- Adopted a **representative simulation** with Python’s **Faker**.
- Generated realistic placeholders (e.g., director names, designations).
- Demonstrated enrichment workflow:
  ```
  Select CINs -> Generate synthetic data -> Save structured CSV
  ```


---

## Project Folder Structure
```
MCA_Project/
│
├── data/
│   ├── maharashtra.csv
│   ├── delhi.csv
│   ├── gujarat.csv
│   ├── karnataka.csv
│   ├── tamilnadu.csv
│   ├── mca_master_dataset.csv
│   ├── change_log.csv
│   └── enriched_cin_data.csv
│
├── MCA_Pipeline.ipynb        # Google Colab notebook
├── app.py                    # Streamlit dashboard app
├── requirements.txt          # Library dependencies
└── README.md                 # Project documentation
```

---

## requirements.txt
```
pandas
numpy
streamlit
faker
hashlib
datetime
pyngrok
```

---

## Key Features
- Automated data merging & normalization
- Row-level change detection via hashing
- Simulated data enrichment with Faker
- Interactive Streamlit dashboard with filters and search
- Integrated rule-based chatbot
- Fully reproducible in Google Colab + Drive

---

## Technologies Used
| Layer | Tools / Libraries |
|:------|:------------------|
| Data Processing | Python, Pandas, NumPy |
| Change Detection | hashlib (MD5), datetime |
| Enrichment | Faker |
| Dashboard | Streamlit, ngrok |
| Storage | Google Drive, CSV |
| Visualization | Streamlit Components |
| Environment | Google Colab |

---

## Future Enhancements
- Integrate **real MCA or API Setu** data sources.
- Migrate to a **database backend** (PostgreSQL/BigQuery).
- Add **authentication and role-based access**.
- Replace rule-based chatbot with an **LLM model**.

---

##  Author
**Project Name:** MCA Insights Engine  
**Developed By:** *Pranav SUnil Raja*  
**Environment:** Google Colab + Streamlit  

---


