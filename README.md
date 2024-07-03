# Pharmaceutical Product Performance and Trend Analysis

## Project Overview

This project aims to analyze the performance and trends of pharmaceutical products using SQL Server for data management and Power BI for visualization. The analysis covers various aspects, including product popularity, pack performance, trend analysis, relationship mapping, and market status distribution.

## Business Questions

1. **Product Popularity:** Which products are the most prescribed?
2. **Pack Performance:** Which packs have the highest total links?
3. **Trend Analysis:** How have product prescriptions changed over the last five years?
4. **Relationship Analysis:** What are the common relationships between prescribable products?

## Solution Approach

We solved these business questions by executing SQL queries on the `DBATest` database in SQL Server and visualizing the results in Power BI.

## Queries

### 1. Product Popularity

**Query:**

```sql

SELECT 
    pp.sProductDisplayName,
    COUNT(lp.iPrescribableProductID) AS TotalPrescriptions
FROM 
    [DBATest].[dbo].[SX_PrescribableProduct] pp
JOIN 
    [DBATest].[dbo].[SX_linkPack] lp ON pp.iPrescribableProductID = lp.iPrescribableProductID
GROUP BY 
    pp.sProductDisplayName;
```
### 2. Pack Performance

**Query:**

```sql
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
```

### 3. Trend Analysis

**Query:**

```sql
SELECT 
    YEAR(lp.dDate) AS Year,
    COUNT(lp.iPrescribableProductID) AS TotalPrescriptions
FROM 
    [DBATest].[dbo].[SX_linkPack] lp
JOIN 
    [DBATest].[dbo].[SX_PrescribableProduct] pp ON lp.iPrescribableProductID = pp.iPrescribableProductID
WHERE 
    lp.dDate IS NOT NULL AND lp.dDate >= DATEADD(YEAR, -20, GETDATE())
GROUP BY 
    YEAR(lp.dDate)
ORDER BY 
    Year;
```

### 4. Relationship Analysis

**Query:**

```sql
SELECT 
    pp1.sProductDisplayName AS ProductFrom,
    pp2.sProductDisplayName AS ProductTo,
    COUNT(r.iRelationshipTypeID) AS TotalRelationships
FROM 
    [DBATest].[dbo].[SX_PrescribableProduct] pp1
JOIN 
    [DBATest].[dbo].[SX_PrescribableProductRelationship] r ON pp1.iPrescribableProductID = r.iPrescribableProductFromID
JOIN 
    [DBATest].[dbo].[SX_PrescribableProduct] pp2 ON r.iPrescribableProductToID = pp2.iPrescribableProductID
GROUP BY 
    pp1.sProductDisplayName, pp2.sProductDisplayName
ORDER BY 
    TotalRelationships DESC;

```
