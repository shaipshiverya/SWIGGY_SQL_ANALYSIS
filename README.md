# SWIGGY_SQL_ANALYSIS

---

## 📂 Files in This Repo
| File | Description |
|------|-------------|
| `swiggy.sql` | All SQL queries with detailed comments |
| `restaurants.csv` | Raw Swiggy restaurant dataset |

---

## 🛠️ Tech Stack
| Tool | Purpose |
|------|---------|
| MySQL | Data Cleaning, Analysis, Strategy |
| CTEs | Complex multi-step queries |
| Window Functions | Ranking & Segmentation |
| GitHub | Version Control |

---

## 🔄 Project Phases

### 🧹 Phase 1 — Data Cleaning
```sql
-- Standardized inconsistent cuisine names
UPDATE restaurants
SET cuisine = 'North Indian'
WHERE cuisine = 'North-Indian';

-- Handled NULL ratings
UPDATE restaurants
SET rating = 0
WHERE rating IS NULL;

-- Removed cost outliers
DELETE FROM restaurants
WHERE cost < 50 OR cost > 50000;
```

---

### 📊 Phase 2 — Market Overview (EDA)

#### 🏙️ Market Saturation — Top 5 Cities
```sql
SELECT city,
COUNT(id) AS restaurants_number
FROM restaurants
GROUP BY city
ORDER BY restaurants_number DESC
LIMIT 5;
```

#### 💸 Cost of Living Analysis
```sql
SELECT city,
ROUND(AVG(cost), 0) AS avg_cost
FROM restaurants
WHERE city IN ('Bangalore','Delhi','Mumbai',
               'Hyderabad','Pune')
GROUP BY city
ORDER BY avg_cost DESC;
```

---

### 🍛 Phase 3 — Cuisine & Popularity

#### 🔥 Most Popular Cuisines by Vote Volume
```sql
SELECT cuisine,
SUM(rating_count) AS total_votes
FROM restaurants
GROUP BY cuisine
ORDER BY total_votes DESC
LIMIT 5;
```

#### ⭐ Quality vs Quantity Matrix
```sql
SELECT cuisine,
COUNT(id) AS restaurant_count,
ROUND(AVG(rating), 2) AS avg_rating,
SUM(rating_count) AS total_votes
FROM restaurants
GROUP BY cuisine
HAVING SUM(rating_count) > 1000
ORDER BY avg_rating DESC
LIMIT 5;
```

---

### 🚀 Phase 4 — Advanced Analytics

#### 🏆 Top Rated Restaurant Per City (Window Function)
```sql
WITH RankedRestaurants AS (
    SELECT name, city, cuisine, rating,
        RANK() OVER (
            PARTITION BY city 
            ORDER BY rating DESC, 
            rating_count DESC
        ) AS rank_in_city
    FROM restaurants
    WHERE rating_count > 50
)
SELECT name, city, cuisine, rating
FROM RankedRestaurants
WHERE rank_in_city = 1;
```

#### 💰 Market Segmentation (Bucketing)
```sql
SELECT 
    CASE 
        WHEN cost < 300 THEN 'Budget'
        WHEN cost BETWEEN 300 AND 800 
             THEN 'Mid-range'
        ELSE 'Luxury' 
    END AS price_segment,
    COUNT(id) AS total_restaurants,
    ROUND(AVG(rating), 2) AS avg_rating
FROM restaurants
GROUP BY 1
ORDER BY total_restaurants DESC;
```

---

### 🌊 Phase 5 — Blue Ocean Strategy
> *Find the perfect City + Cuisine to launch next*

```sql
-- High Demand (rating_count > 20000)
-- Low Competition (restaurants < 50)
SELECT city, cuisine,
COUNT(id) AS supply_count,
SUM(rating_count) AS demand_volume
FROM restaurants
GROUP BY city, cuisine
HAVING COUNT(id) < 50
   AND SUM(rating_count) > 20000
ORDER BY demand_volume DESC
LIMIT 5;
```

---

## 💡 Key Business Insights

> 🏙️ **Bangalore** = Most saturated market — tough to enter
>
> 💸 **Mumbai** = Highest average dining cost in India
>
> 🍛 **North Indian** = Most popular cuisine by volume
>
> ⭐ **Biryani** = Highest customer satisfaction score
>
> 🚀 **Recommendation: Launch a Mid-range Biryani
>    brand in Pune** to capitalize on the
>    Supply-Demand gap

---

## 🧠 SQL Concepts Used
| Concept | Used For |
|---------|---------|
| `UPDATE / DELETE` | Data Cleaning |
| `GROUP BY + HAVING` | Aggregations |
| `CASE WHEN` | Market Segmentation |
| `WITH (CTE)` | Ranked Restaurant Query |
| `RANK()` | Window Function Ranking |
| `AVG / COUNT / SUM` | Statistical Analysis |

---

## 🚀 How to Run
```sql
-- Step 1: Create Database
CREATE DATABASE SWIGGYY;
USE SWIGGYY;

-- Step 2: Import restaurants.csv

-- Step 3: Run swiggy.sql
```

---

## 🔮 Future Scope
- 📊 Power BI Dashboard for visual insights
- 🐍 Python (Pandas) for deeper EDA
- 📍 Geo-mapping of restaurant density
- 🤖 ML model for demand forecasting

---

## 👩‍💻 About Me
**Shaipshi** — Aspiring Data Analyst
`SQL` `Python` `Power BI` `Excel`

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/shaipshi-verya-1b918a162/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/shaipshiverya)
