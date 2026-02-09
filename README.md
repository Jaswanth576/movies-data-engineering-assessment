# Movie Data Engineering Assessment

## Overview
This project builds a **Gold analytics layer** for movies using Databricks.

Netflix titles are treated as the base dataset and are enriched with ratings, votes, and director information from IMDB using a best-effort matching strategy.

The focus of the solution is clarity, reproducibility, and preserving all Netflix records while applying deterministic enrichment rules.

---

## Architecture
The solution follows a medallion-style approach:

**Bronze → Silver → Gold**

- **Bronze**: raw ingested data  
- **Silver**: cleaned, typed, standardized datasets  
- **Gold**: analytics-ready outputs

---

## Gold Layer Design
The Gold view provides one row per Netflix movie.

Key principles:
- Netflix is the source of truth  
- IMDB is enrichment  
- no Netflix rows are dropped  

If enrichment cannot be found, values remain **NULL**.

---

## Join Strategy
Direct joins were not reliable due to differences in:
- punctuation  
- casing  
- extra spaces  
- occasional numbering prefixes in IMDB  

To improve matching, titles are normalized before joining.

---

## Director Logic
Only one director is required, so the first element from the IMDB list is selected.

---

## Ratings & Votes
IMDB values are preferred.  
If missing, Netflix values are used as fallback.

---

## Handling Multiple Matches
If multiple IMDB rows match a Netflix movie, the highest rated record is selected using ranking logic.

---

## Why a View Instead of Tables?
For the purpose of this assessment:
- keeps the implementation simple  
- avoids extra storage  
- easier to review  
- always reflects latest Silver data  

In a production system, this logic could be materialized for performance.

---

## Repository Structure
