# 🕸️ Scrape & Classify – Web Data Extraction Pipeline

A modular Python pipeline for extracting, cleaning, and classifying web data using open-source tools. Designed for smart web prospecting, offline research, and scalable automation.

---

## 📌 Features

* 🔍 **Web Scraping**: Collects structured data from target websites using `requests` and `BeautifulSoup`.
* 🧹 **Data Cleaning**: Standardizes and prepares text for analysis using `pandas`, `re`, and `unicodedata`.
* 🧠 **ML Classification**: Predicts categories or relevance using a trained `scikit-learn` classifier.
* 🗃️ **Export Options**: Saves results to `.csv`, `.json`, and `.txt` formats for integration and sharing.
* 🔐 **Offline-First**: Fully operable without internet after initial setup. No external API dependencies.
* 📁 **Modular Design**: Easy to adapt, reuse, or extend in parts or as a full pipeline.

---

## 🚀 Use Cases

* Local business discovery and qualification
* Lead generation and client research
* Academic data collection and preprocessing
* Custom classifiers for specific domains or keywords

---

## 🧰 Stack

| Function          | Tools Used                         |
| ----------------- | ---------------------------------- |
| Scraping          | `requests`, `BeautifulSoup`        |
| Cleaning          | `pandas`, `re`, `unicodedata`      |
| ML Classification | `scikit-learn`, `joblib`           |
| Exporting         | `csv`, `json`, plain text handling |
| Logging & CLI     | `argparse`, `logging`, `.env`      |

---

## 🛠️ How It Works

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run the scraper (customize inside config or via CLI)
python scrape.py --target "https://example.com" --output data/raw.csv

# 3. Clean and preprocess the scraped data
python clean.py --input data/raw.csv --output data/clean.csv

# 4. Classify the cleaned data using the trained model
python classify.py --input data/clean.csv --model models/classifier.pkl --output results/predictions.csv
```

Each step is independent and can be used modularly.

---

## 📊 Example Output

```csv
name,website,description,category,score
"ACME Bakery","acme.com","...","food",0.92
"FutureTech Solutions","futuretech.ai","...","tech",0.88
...
```

---

## 📂 Folder Structure

```
scrape_classify_pipeline/
│
├── data/                 # Raw and processed data
├── models/               # Trained models (.pkl)
├── results/              # Classification outputs
├── scrape.py             # Web scraping logic
├── clean.py              # Data cleaning script
├── classify.py           # ML classification pipeline
├── utils/                # Reusable utility functions
├── config.env            # Environment variables
└── README.md             # Project overview
```

---

## 📦 Dependencies

* Python 3.8+
* pandas
* beautifulsoup4
* scikit-learn
* requests
* joblib

---

## 🔒 Privacy-First Design

This pipeline does **not** rely on cloud APIs or send data externally. Everything runs locally, making it ideal for private research or restricted environments.

---

## 📌 License

Licensed under the MIT License – feel free to modify and use for personal or commercial projects.

---

## 🙌 Author

**Jose Daniel Soto**
[📧 Email](mailto:jdsotomarin@gmail.com) | [🌐 GitHub](https://github.com/josedaniel-dev/portfolio) | [🔗 LinkedIn](https://linkedin.com/n/josedanielsoto)

