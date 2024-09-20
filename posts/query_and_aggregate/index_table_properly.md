---
title: Index Table Properly
layout: home
parent: Query & Aggregate
nav_order: 1
---

# Index Table Properly

How to index table and check if the index is used when querying the table

To make queries faster, you can add an index to it. Index works like table of contents in a book. You mark where chapter 1 is, where chapter 2 is, etc.

## Index Types

There are some types of index, but generally we only use B-Tree and Hash.

> In simple terms, user Hash if the column is searched by "=" and use B-Tree if the column is searched using ">", "<", "=" and any other operator (B-Tree for general purposes).

### Example
Let's say we have table and query like this

![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FrfqFAj9quNXQkCENAcwo%252Fimage.png%3Falt%3Dmedia%26token%3D6fc2d0c7-ba20-400e-b5bd-fc1738ec2eaa&width=400&dpr=2&quality=100&sign=3fc561b4&sv=1)

```sql
SELECT * FROM posts WHERE created_at = '2024-06-01'
```

If the created_at field only queried using "=" operator, its better to use Hash index than B-Tree index. But if the created_at field queried using "<" or ">" like this query :

```sql
SELECT * FROM posts WHERE created_at > '2024-06-01' AND created_at <= '2024-06-09'
```
**or**
```sql
SELECT * FROM posts WHERE created_at BETWEEN '2024-06-01' AND '2024-06-09'
```
Its preferable to use B-Tree index because it can accomodate most of the operator used in `WHERE` or `JOIN`

