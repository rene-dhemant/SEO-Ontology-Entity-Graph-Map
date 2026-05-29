# 🕸️ SEO Ontology & Entity Graph Map 

## 🚀 Overview
The **Ontology & Entity Graph Map** is an advanced Python/Streamlit application that shifts SEO analysis from implicit semantic vectors to explicit Knowledge Graph architecture. By parsing web content through Large Language Models (LLMs), it reverse-engineers Google's "Things, not Strings" methodology.

This tool extracts the "Anchor Entity" of a page and its structured "Related Entities" based on the Schema.org ontology. It visualizes these relationships using interactive NetworkX/PyVis graphs and outputs prescriptive content briefs, competitor gap analyses, and deployment-ready JSON-LD Schema markup.

## 🛠️ Technical Architecture & What It Does
1. **Data Ingestion & Cleaning:** Ingests Screaming Frog crawl data containing URLs, Titles, and extracted body text.
2. **Knowledge Graph Extraction (`entity_extractor.py`):** Uses the `gemini-2.5-flash` model to analyze text and output strict JSON. It maps nodes (Entities), properties (Schema.org Types), edges (Relationships like `isPartOf`, `author`), and assigns a `recommended_depth` score for content structuring.
3. **Local Caching:** Responses are saved to `entities_cache.json` to prevent redundant API calls and reduce bandwidth costs.
4. **Interactive Visualization (`graph_builder.py`):** Uses `networkx` and `pyvis` to generate an interactive, node-based web showing how the core topic connects to its supporting entities.
5. **Schema Generation (`schema_generator.py`):** Automatically translates the extracted entity graph into perfectly nested, deployment-ready `application/ld+json` code.
6. **Site-Wide Bulk Processing:** Merges individual page graphs into a massive, domain-wide Network Graph to visualize entity clusters and orphaned content.

## 🎯 Primary Use Cases
* **Deterministic Content Briefs:** Gives copywriters a rigid blueprint of exactly which Schema entities must exist on a page and at what structural depth (Mention vs. Dedicated Section).
* **Competitor Entity Gap Analysis:** Overlays a competitor's URL to flag missing critical entities that are satisfying the Knowledge Graph intent better than your page.
* **Automated JSON-LD Generation:** Instantly creates valid, nested Schema.org markup based on the page's actual extracted nodes.
* **Domain Architecture Visualization:** Maps the entire site to find "Entity Orphans" (pages disconnected from your core domain clusters) or strong semantic hubs.

---

## 📂 Data Requirements
The application dynamically adapts based on the files present in its directory.

### Core Required Files
* `crawl_data.csv`: Screaming Frog "Internal HTML" export.
  * Required mappings: `Adresse` (URL), `Titel 1` (Title), `Extracted_Text 1` (Main body text via Custom Extraction).

### Optional Upgrade Files (Auto-Detected)
* `competitor_crawl.csv`: Screaming Frog crawl of a competitor (using the exact same extraction setup). Enables the **Competitor Entity Gap Finder**.

---

## 💻 System Requirements & Installation

**Prerequisites:**
* Python 3.10+
* Valid API Key for Google GenAI (Gemini).

**1. Clone/Setup the Repository**
Ensure all Python scripts (`app.py`, `data_processor.py`, `entity_extractor.py`, `graph_builder.py`, `schema_generator.py`) and your `.csv` data files are in the same working directory.

**2. Install Dependencies**
Create a `requirements.txt` file containing:
```text
streamlit
pandas
networkx
pyvis
google-genai
```
Run the installation:
```bash
pip install -r requirements.txt
```

**3. Run the Application**
```bash
streamlit run app.py
```
*(Note: You can input your Gemini API Key directly into the secure sidebar UI within the app, or set it system-wide via the `GEMINI_API_KEY` environment variable).*

---

## 🔄 Running on a New Domain or Updating Content (Cache Reset)
To ensure your graphs and schema are accurate, you **must clear the application cache** when changing domains or analyzing newly updated content.

Follow this checklist:
1. **Delete the Cache:** Delete the `entities_cache.json` file in your directory. If you don't, the tool might merge two different domains together or load outdated content analysis.
2. **Swap the Core Data:** Replace `crawl_data.csv` (and `competitor_crawl.csv` if applicable) with the new data.
3. **Restart the App:** Refresh Streamlit and run your extractions again to build a fresh, accurate Knowledge Graph.

---

## 👔 The Strategic Consultant Viewpoint

As a strategic SEO consultant, this tool moves beyond high-level strategy and directly into tactical content execution. Here is how to action the data:

### 1. Translating "Depth" into Copywriting Briefs
The Content Brief table assigns a `recommended_depth` to every entity. This is a direct blueprint for your writers:
* **Depth 3 (Dedicated Sections):** These are critical sub-topics. The writer must create dedicated H2 or H3 sections for these entities. 
* **Depth 2 (Paragraph Level):** These entities require supporting context. A strong paragraph tying the concept to the Anchor Entity is sufficient.
* **Depth 1 (Mentions/E-E-A-T):** These are factual nodes (e.g., `Person`, `Organization`, `Price`). They just need to exist explicitly on the page to satisfy Trust and Identity vectors. 

### 2. Identifying Contextual Isolation
When reviewing a page's graph, look for an imbalance. If a "Service" page is heavily linked to software tools or competitor brand names, but lacks connections to informational entities, it suffers from **Contextual Isolation**. Google may miscategorize the page intent. *Action: Inject the missing top-level informational entities to re-anchor the page's meaning.*

### 3. Executing the "Entity Gap" Strategy
When the competitor gap table flashes red, it reveals the exact nodes the competitor used to satisfy Google's NLP algorithms. 
* *Action:* Do not just sprinkle those words onto the page. Look at the Schema Type and Depth. If the competitor has `Search Intent (Thing)` at Depth 3, you must write a highly authoritative, dedicated section about Search Intent to close the gap.

### 4. Resolving Entity Orphans (Site-Wide Graph)
After running the Bulk Extract, view the Site-Wide Graph tab. Look for your revenue-driving pages. Are they pulled tightly into the center of a dense semantic cluster, or are they floating on the periphery with only one line connecting them to the rest of the site?
* *Action:* If a core page is an **Entity Orphan**, build new supporting content that explicitly bridges the topic of the orphaned page to the dominant topics of your main cluster, connecting them via internal links.
