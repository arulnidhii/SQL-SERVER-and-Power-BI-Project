# Pharmaceutical Product Performance and Trend Analysis

## Project Overview

This project aims to analyze the performance and trends of pharmaceutical products using SQL Server for data management and Power BI for visualization. The analysis covers various aspects, including product popularity, pack performance, trend analysis, relationship mapping, and market status distribution.

## Business Questions

1. **Product Popularity:** Which products are the most prescribed?
2. **Pack Performance:** Which packs have the highest total links?
3. **Trend Analysis:** How have product prescriptions changed over the last five years?
4. **Relationship Analysis:** What are the common relationships between prescribable products?
5. **Market Status:** What is the market status distribution of all packs?

## Solution Approach

We solved these business questions by executing SQL queries on the `DBATest` database in SQL Server and visualizing the results in Power BI.

## Queries and Explanations

### 1. Product Popularity

**Query:**
'''sql

SELECT 
    pp.sProductDisplayName,
    COUNT(lp.iPrescribableProductID) AS TotalPrescriptions
FROM 
    [DBATest].[dbo].[SX_PrescribableProduct] pp
JOIN 
    [DBATest].[dbo].[SX_linkPack] lp ON pp.iPrescribableProductID = lp.iPrescribableProductID
GROUP BY 
    pp.sProductDisplayName;

### 2. Pack Performance

**Query:**

'''sql
SELECT 
    p.sPackDisplayName,
    COUNT(lp.iDispensiblePackID) AS TotalLinks
FROM 
    [DBATest].[dbo].[SX_Pack] p
JOIN 
    [DBATest].[dbo].[SX_linkPack] lp ON p.iDispensiblePackID = lp.iDispensiblePackID
WHERE 
    p.sPackDisplayName IS NOT NULL
GROUP BY 
    p.sPackDisplayName
ORDER BY 
    TotalLinks DESC;
