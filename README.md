# 🏙️ Manhattan Vacation Rental Investment Analysis

A data-driven analysis of **2,000+ Manhattan Airbnb listings** to identify the most profitable short-term rental investment opportunities. This project uses actual booking data from **September–October 2022** to uncover top neighborhoods, ideal property sizes, and projected annual revenue.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Key Findings](#key-findings)
- [File Structure](#file-structure)
- [Methodology](#methodology)
- [Data Sources](#data-sources)
- [Tools & Formulas Used](#tools--formulas-used)
- [Assumptions & Limitations](#assumptions--limitations)
- [How to Use This File](#how-to-use-this-file)
- [Future Research](#future-research)

---

## 📌 Project Overview

This Excel-based analysis was conducted to provide **clear, actionable investment guidance** for stakeholders entering Manhattan's competitive short-term rental market. Rather than relying on assumptions, the project uses real Airbnb listing and calendar data to identify exactly which properties and locations generate superior returns.

---

## 🔑 Key Findings

### Top 10 Investment Neighborhoods
Ranked by number of reviews in the last 12 months (used as a proxy for demand and attractiveness):

| Rank | Neighborhood     | Reviews (LTM) |
|------|-----------------|---------------|
| 1    | Lower East Side | 6,242         |
| 2    | Hells Kitchen   | 5,506         |
| 3    | Harlem          | 5,157         |
| 4    | Midtown         | 4,128         |
| 5    | Upper West Side | 3,497         |
| 6    | Chelsea         | 2,913         |
| 7    | East Village    | 2,572         |
| 8    | East Harlem     | 2,175         |
| 9    | West Village    | 1,735         |
| 10   | Upper East Side | 1,696         |

![Most Attractive Neighborhoods in Manhattan](images/top_neighborhoods.png)

---

### Ideal Property Size
- **1-bedroom properties** dominate demand in **9 out of 10** top neighborhoods.
- **Midtown** is the exception — studios perform best there.

![Most Popular Property Sizes in Manhattan](images/property_sizes_manhattan.png)

![Top 10 Neighborhood Popular Property Sizes](images/property_sizes_top10.png)

---

### Top Revenue Performance (30-Day Period)
- The **highest-earning property** (ID: 49946551) in Midtown generated **$29,940** in 30 days.
- Projected annual revenue at similar occupancy: approximately **$359,280**.
- Top properties are concentrated in Midtown, Hells Kitchen, Lower East Side, Chelsea, East Village, and Upper West Side.

![Top Earning Short-Term Rental Properties by Neighborhood](images/top_earners_by_neighborhood.png)

![Projected Yearly Revenue for Top 10 Neighborhoods](images/projected_yearly_revenue.png)

---

## 📁 File Structure

```
manhattan-rental-analysis/
│
├── README.md                              ← You are here
├── images/
│   ├── top_neighborhoods.png              ← Most Attractive Neighborhoods chart
│   ├── property_sizes_manhattan.png       ← Most Popular Property Sizes (all Manhattan)
│   ├── property_sizes_top10.png           ← Most Popular Property Sizes (Top 10 neighborhoods)
│   ├── top_earners_by_neighborhood.png    ← Top Earning Properties by Neighborhood
│   └── projected_yearly_revenue.png       ← Projected Yearly Revenue chart
│
└── Manhattan_Rental_Analysis.xlsx         ← Main Excel workbook
    ├── Table of Contents
    ├── Executive Summary
    ├── AirBnB Listings Data               ← Raw property-level data
    ├── AirBnB 30-Day Pricing Range        ← Calendar: occupancy, availability, pricing
    ├── Initial Assumptions
    ├── Change Log
    ├── Pivot Tables & Charts
    │   ├── Top 10 Neighborhoods in Manhattan
    │   ├── Most Popular Property Sizes in Manhattan
    │   ├── Most Popular Property Sizes (by each Top 10 Neighborhood)
    │   ├── Top 10 Listings Revenue Summary and Projection
    │   └── Visualizations Compiled
    ├── Final Assumptions
    └── Data Dictionary
```

---

## 🔬 Methodology

### Data Cleaning
- **Froze** header rows on Listings and Calendar sheets for easier navigation.
- Created `neighborhood_clean` column using `=PROPER(TRIM(neighborhood))` to standardize neighborhood names.
- Created `bedrooms_clean` column using `=IF(bedrooms="",0,bedrooms)` to treat blank bedroom entries as studio apartments (0 bedrooms).

### Analysis Steps

1. **Neighborhood Attractiveness** — Built a Pivot Table ranking neighborhoods by sum of reviews in the last 12 months. Visualized as a bar chart.

2. **Property Size Popularity** — Built Pivot Tables showing the count of listings by bedroom count, both Manhattan-wide and for each of the Top 10 neighborhoods.

3. **Top Listing Identification** — Created a `top_listing` column using a nested `IF/OR/AND` formula to flag properties in the Top 10 neighborhoods that match the most popular bedroom size for that area.

4. **Revenue Calculation**
   - Added `revenue_earned` to the Calendar sheet: `=IF(C2="f",E2,0)` — counts revenue only for booked (non-available) nights.
   - Added `revenue_earned` to the Listings sheet: `=SUMIF(Calendar!A:A,A2,Calendar!H:H)` — aggregates total revenue per property.

5. **Top Earners** — Built a Pivot Table of the Top 10 revenue-earning properties. Used `=VLOOKUP(A2,Listings!A:AC,29,FALSE)` to retrieve their neighborhoods.

6. **Yearly Revenue Projection** — Extrapolated 30-day revenue figures to a full year at consistent occupancy rates.

---

## 📊 Data Sources

| Dataset               | Description                                                                                       |
|-----------------------|---------------------------------------------------------------------------------------------------|
| **AirBnB Listings**   | Property-level data: unique IDs, host details, property specs, review metrics, performance scores |
| **AirBnB Calendar**   | 30-day window (Sept–Oct 2022): unit occupancy, availability status, and daily pricing             |
| **AirBnB Data Dictionary** | Field definitions and data types for all columns                                           |

> ⚠️ The calendar data covers a **peak travel period** (Sept–Oct), which likely reflects higher-than-average occupancy. Annual projections should be interpreted with this in mind.

---

## 🛠️ Tools & Formulas Used

| Purpose                          | Formula / Tool                                                  |
|----------------------------------|-----------------------------------------------------------------|
| Clean neighborhood names         | `=PROPER(TRIM(neighborhood))`                                   |
| Assign 0 to studio apartments    | `=IF(bedrooms="",0,bedrooms)`                                   |
| Flag top listings                | `=IF(OR(AND(...)),1,0)` (nested neighborhood + bedroom checks)  |
| Calculate nightly revenue        | `=IF(C2="f",E2,0)`                                              |
| Aggregate revenue per listing    | `=SUMIF(Calendar!A:A,A2,Calendar!H:H)`                          |
| Look up neighborhood for top IDs | `=VLOOKUP(A2,Listings!A:AC,29,FALSE)`                           |
| Pivot Tables                     | Used throughout for neighborhood, bedroom, and revenue analysis |
| Bar Charts / Column Charts       | Visualizations for all major findings                           |

---

## ⚠️ Assumptions & Limitations

### Initial Assumptions
- The Sept–Oct 2022 window is assumed to be a **popular travel period**, which may inflate occupancy estimates.
- Daily price variation exists for the same unit across different dates.
- Review scale ranges from **1 to 5**; all reviews were counted (positive and negative alike) as a demand proxy.
- Empty bedroom values represent **studio apartments** (0 bedrooms), not missing data.
- All rentals are **full-unit rentals** — no shared spaces included.

### Final Assumptions
- The Top 10 neighborhoods will **maintain their relative attractiveness** going forward.
- 1-bedroom dominance and the Midtown studio exception will **persist in the near term**.
- This analysis does **not** account for property acquisition costs, renovation needs, or operational expenses.
- Properties within the same neighborhood/bedroom category are treated as **equivalent** regardless of individual amenities, reviews, or host status.
- The 30-day sample **does not capture full seasonal variation** (e.g., summer peaks or winter lows).
- External factors (economic downturns, policy changes, significant events) are **not modeled**.

---

## 💡 How to Use This File

1. Open `Manhattan_Rental_Analysis.xlsx` in Microsoft Excel (2016 or later recommended).
2. Start with the **Executive Summary** tab for a high-level overview.
3. Review the **Change Log** to understand all transformations applied to the raw data.
4. Explore individual **Pivot Table tabs** to drill into specific neighborhoods or property sizes.
5. Refer to the **Data Dictionary** tab for column definitions.

---

## 🔭 Future Research

- Analyze **property type diversity** beyond bedroom count (e.g., entire home vs. private room).
- Conduct **guest experience satisfaction surveys** within the top 10 neighborhoods.
- Incorporate **seasonal data** to build a more complete annual revenue model.
- Factor in **acquisition and operating costs** to calculate true ROI.

---

## 👤 Author

Richard Rivera Cartagena
[LinkedIn](https://www.linkedin.com/in/richard-rivera-cartagena/) · [GitHub](https://github.com/RichRC)


---

## 📄 License

This project is intended for educational and portfolio purposes. Raw data sourced from Airbnb public listings.
