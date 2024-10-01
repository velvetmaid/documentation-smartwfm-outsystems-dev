---
title: Advanced SQL Cheatsheet
layout: home
parent: Common
nav_order: 1
---

# Advanced SQL Cheatsheet for OutSystems
___

Some syntax may not be found in standard SQL, OutSystems has specific adjustments. This guide aims to help you understand and effectively use SQL in the context of OutSystems.

## Table of Contents
1. [Filtering with LIKE](#filtering-with-like)
2. [Range]
___


### Filtering with LIKE
```sql
SELECT * FROM MyEntity WHERE user_name LIKE '%' || @Parameter || '%'

-- Search for a substring anywhere (case-insensitive) using UPPER or LOWER
SELECT * FROM MyEntity WHERE UPPER(user_name) LIKE '%' || UPPER(@Parameter) || '%'
```