# 🧹 Client Analysis for Floor Cleaner Sales
### Data Analysis · Pandas · NumPy · Matplotlib · Seaborn · Geospatial Filtering · Excel Export

> A business-focused data analysis project to identify the **best potential clients** (hotels, restaurants & guest houses) for a floor cleaner product — using location-based filtering, ratings analysis, and visualisation.

---

## 📌 Objective

Analyse a dataset of hotels and restaurants to:
- Find which clients are best suited for floor cleaner sales
- Filter nearby clients using real GPS coordinates and the Haversine formula
- Understand trends using location, ratings, reviews, types, and subtypes
- Generate charts to visualise potential leads and hotspots
- Export a final top-10 lead list to Excel for the sales team

---

## 📂 Project Structure

```
ClientAnalysis/
├── FloorCleanerSales.ipynb         # Main Jupyter Notebook (all parts)
├── Leads.xlsx                      # Input dataset (hotels & restaurants)
├── Top10_Potential_Customer.xlsx   # Output: top 10 high-potential clients
└── README.md
```

---

## 🔧 Tech Stack

| Tool | Usage |
|------|-------|
| Python | Core language |
| Pandas | Data loading, cleaning, filtering, grouping |
| NumPy | Haversine formula, radians conversion |
| Matplotlib | Pie chart, scatter plot, histogram |
| Seaborn | Bar chart, histogram with annotations |
| OpenPyXL | Exporting results to Excel |
| Jupyter Notebook | Development environment |

---

## 📋 Part 1 — Data Loading & Cleaning

- Loaded `Leads.xlsx` using `pd.read_excel()` and inspected shape, column types, and null counts
- Explored unique values in `category`, `subtypes`, and `type` columns
- Dropped 40+ irrelevant columns (URLs, hours, social links etc.) to keep only useful fields
- Removed rows where `business_status != OPERATIONAL` and `verified == False` — filtered out closed/unverified businesses
- Removed duplicate business names using `drop_duplicates(subset="name")`
- Dropped nulls in `name` and `full_address`; filled `phone` nulls with `"N/A"`
- Filled missing `rating` with column mean; filled `reviews` and `photos_count` with `0`
- Extracted **locality** from the `borough` column using string splitting for area-level analysis

---

## 📍 Part 2 — Distance-Based Client Filter (Geospatial)

- Reference point: **School of Excellence** — Lat: `28.606740`, Lon: `77.307630`
- Converted all lat/lon values to radians using `np.radians()`
- Implemented full **Haversine formula with NumPy** (vectorised — no loops):

```python
dlat = target_df["latitude_rad"] - lat_main_rad
dlon = target_df["longitude_rad"] - lon_main_rad
a = np.sin(dlat/2)**2 + np.cos(lat_main_rad) * np.cos(target_df["latitude_rad"]) * np.sin(dlon/2)**2
c = 2 * np.arctan2(np.sqrt(a), np.sqrt(1 - a))
target_df["distance_from_office (in km.)"] = round(6371.0 * c, 2)
```

- Added `distance_from_office (in km.)` column to the dataset
- Filtered clients within **20 km** of the office for lead shortlisting

---

## 📊 Part 3 — Visualisations

| Chart | What it shows |
|-------|--------------|
| Bar Plot (Seaborn) | Top 10 most common client types with annotated count labels |
| Pie Chart (Matplotlib) | Distribution of top 5 categories with exploded slices |
| Scatter Plot | Reviews vs Rating — red dots highlight rating ≥ 4.5 and reviews > 200 |
| Histogram (Seaborn) | Overall ratings distribution across all clients |
| Bar Plot by Locality | Number of potential customers per area/locality |

---

## 💼 Part 4 — Business Insights & Lead Generation

**High-potential client criteria applied:**
- Within 20 km of office
- Reviews > 200
- Rating ≥ 4.2

**Key finding:** `Rohini` area has the highest concentration of potential customers

**Final output:** Top 10 clients from Rohini sorted by rating, exported to `Top10_Potential_Customer.xlsx` with:
- Business name
- Phone number
- Full address

---

## 🎁 Bonus Analysis

- **Grouped by locality** to find average ratings per area — sorted descending
- **Flagged low-engagement clients** with zero photos OR zero reviews using a boolean column `zero_photos_reviews`
- **Grouped by subtype** to find the highest-rated category in each subtype using `idxmax()`

---

## 📈 Summary of Insights

- Hotels, restaurants, and guest houses are the top potential customers for floor cleaner sales
- Businesses with `CLOSED_TEMPORARILY` status and `verified = False` were excluded to avoid irrelevant leads
- High ratings + high review count + geographic proximity = strongest indicator of a valuable client
- **Rohini** emerged as the top target area with the highest number of qualified leads

---

## 🚀 How to Run

```bash
# 1. Clone the repo
git clone https://github.com/inboxofpiyush01/ClientAnalysis

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn openpyxl jupyter

# 3. Add your dataset
# Place Leads.xlsx in the project root folder

# 4. Open notebook
jupyter notebook FloorCleanerSales.ipynb

# 5. Run all cells
# Top10_Potential_Customer.xlsx is auto-generated on completion
```

---
*Built as part of Data Science & Python upskilling — Edureka Master Program in GenAI*
