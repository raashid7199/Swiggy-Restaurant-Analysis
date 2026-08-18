# Swiggy Restaurant Analysis (SQL)

Analysis of **148,541 restaurants** listed on Swiggy, scraped across Indian cities/localities, using SQLite.

## Dataset
- File: `swiggy.csv`
- Columns (as they actually appear in the file): `id, name, city, rating, rating_count, cost, cuisine, lic_no, link, address, menu`
- `rating` — decimal string or `--` for unrated restaurants (87,014 rows, ~58% of the data, are unrated)
- `rating_count` — bucketed text: `'50+ ratings'`, `'1K+ ratings'`, `'Too Few Ratings'`, `'NA'`
- `cost` — text like `'₹ 200'` (average cost for two, rupee symbol included)
- `cuisine` — comma-separated combo string, e.g. `'North Indian,Chinese'`
- No nulls found in `name`, `city`, or `cuisine`

## Tools
SQLite via DB Browser for SQLite. Full script: [`swiggy_analysis.sql`](./swiggy_analysis.sql)

## Approach
1. Created a `swiggy` table matching the real CSV schema.
2. Added `rating_clean` (REAL), `cost_clean` (INTEGER), `rating_count_clean` (INTEGER) helper columns instead of deleting rows outright — this keeps unrated restaurants in the table for city/cuisine counts, while cleanly excluding them from rating-based averages.
3. Ran GROUP BY / aggregation queries to answer 8 business questions.

## Key Findings

**1. Most restaurants listed (by city/locality)**
Bikaner tops the list with 1,666 listings, ahead of Noida-1 (1,428) and Indirapuram, Delhi (1,279). Several Delhi, Bangalore, Pune and Mumbai localities round out the top 10 — the dataset is scraped at a locality level (e.g. "BTM,Bangalore"), not just city level.

**2. Most popular cuisine nationally**
As combo tags, `North Indian,Chinese` is the single most listed combination (6,471 restaurants), followed by `Indian` alone (6,414) and `Chinese` alone (5,051). When cuisine tags are split individually (see Q2b in the SQL file), Indian, Chinese, and North Indian dominate as standalone tags — reflecting how broadly these cuisines are offered across India.

**3. Restaurant chain with the most branches**
Domino's Pizza leads with 442 branches on Swiggy, followed by Pizza Hut (319) and KFC (309). Quick-service and dessert chains (Kwality Wall's, Baskin Robbins, Subway) round out the top 10.

**4. Best-rated city/locality (50+ restaurants)**
Mylapore, Chennai has the highest average rating (4.23) among localities with more than 50 rated restaurants, closely followed by South Kolkata, Frazer Town (Bangalore), and Adyar (Chennai) — all around 4.2.

**5. Most expensive vs. cheapest places to eat**
Khan Market, Delhi is the most expensive locality by average cost-for-two (₹601), followed by Fort Colaba, Mumbai (₹493) and North Goa (₹468) — premium/tourist areas dominate the top of this list. On the other end, smaller towns like Bathinda (₹167) and Ichalkaranji (₹168) are the cheapest.

**6. Best value for money**
Combining high average rating with low average cost, Mylapore and Adyar (Chennai) and Burrabazar (Kolkata) stand out as strong value picks — high ratings (4.1–4.2) at moderate cost (₹291–₹350).

**7. Highly rated & well-reviewed restaurants**
123 restaurants have a rating of 4.5+ with 1,000 or more ratings. Ice cream/sweets specialists (NIC Natural Ice Creams, The Grand Sweets and Snacks) and biryani chains (Behrouz Biryani) feature prominently.

## Files
- `swiggy_analysis.sql` — full CREATE TABLE, cleaning, and all 8+ analysis queries (ready to run in DB Browser for SQLite)
