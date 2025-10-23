## The Three-Level Hierarchy

The schema represents the standard archival structure:

```
Fonds (top level collection)
  └─ Series (groupings within the fonds)
      └─ Sub-series (can nest multiple levels)
          └─ Items (individual records/documents)
```

## How Each Table Works

### 1. **Fonds Table** (Top Level)
```sql
CREATE TABLE fonds (
    id UUID PRIMARY KEY,           -- Unique identifier
    code VARCHAR(50) UNIQUE,       -- Human-readable code like "F1"
    title VARCHAR(500),            -- Name of the collection
    ...
);
```

This represents your entire collection. Example:
- Code: `F1`
- Title: "University Records, 1850-2020"

### 2. **Series Table** (The Hierarchical Part)
```sql
CREATE TABLE series (
    id UUID PRIMARY KEY,
    fonds_id UUID REFERENCES fonds(id),      -- Which fonds does this belong to?
    parent_series_id UUID REFERENCES series(id),  -- Which series is this under?
    code VARCHAR(50),                        -- Code like "S1" or "SS1"
    level_depth INTEGER,                     -- How deep in the hierarchy?
    ...
);
```

The **key insight** is `parent_series_id` - this creates the hierarchy:

**Example data:**

| id   | fonds_id | parent_series_id | code | title                    | level_depth |
|------|----------|------------------|------|--------------------------|-------------|
| s1   | f1       | NULL             | S1   | Administrative Records   | 0           |
| s2   | f1       | s1               | S1.1 | Board Minutes           | 1           |
| s3   | f1       | s1               | S1.2 | Financial Records       | 1           |
| s4   | f1       | s2               | S1.1.1 | Minutes 1900-1950    | 2           |


Notice how:
- `s1` has no parent (it's a top-level series)
- `s2` and `s3` point to `s1` as their parent
- `s4` points to `s2` as its parent (creating a sub-sub-series)

### 3. **Items Table** (Individual Records)
```sql
CREATE TABLE items (
    id UUID PRIMARY KEY,
    series_id UUID REFERENCES series(id),    -- Which series contains this?
    fonds_id UUID REFERENCES fonds(id),      -- Which fonds (for quick queries)?
    reference_code VARCHAR(100),             -- Full code like "F1/S1.1/001"
    ...
);
```

Each item represents an actual document or record.

## How to Query the Hierarchy

### Get all series under a fonds:
```sql
SELECT * FROM series WHERE fonds_id = 'some-uuid';
```

### Get direct children of a series:
```sql
SELECT * FROM series WHERE parent_series_id = 's1';
```

### Get the FULL hierarchy path (using recursive query):
```sql
WITH RECURSIVE hierarchy AS (
    -- Start with a specific series
    SELECT id, code, parent_series_id, code as full_path, 0 as depth
    FROM series
    WHERE id = 's4'
    
    UNION ALL
    
    -- Walk UP the tree to find parents
    SELECT s.id, s.code, s.parent_series_id, 
           s.code || '/' || h.full_path, 
           h.depth + 1
    FROM series s
    JOIN hierarchy h ON h.parent_series_id = s.id
)
SELECT * FROM hierarchy ORDER BY depth DESC;
```

This would return: `S1 → S1.1 → S1.1.1`

## Real-World Example

Let's say you're archiving a university:

```
Fonds: University of Example (F1)
  ├─ Series: Administrative (S1)
  │   ├─ Sub-series: Board Minutes (S1.1)
  │   │   └─ Item: "Minutes Jan 1950" (F1/S1.1/001)
  │   └─ Sub-series: Financial (S1.2)
  ├─ Series: Academic Departments (S2)
      └─ Sub-series: History Dept (S2.1)
          └─ Item: "Course catalog 1960" (F1/S2.1/042)
```

**In the database:**

**fonds:**

| id | code | title |
|----|------|-------|
| f1 | F1 | University of Example |

**series:**

| id | fonds_id | parent_series_id | code | title |
|----|----------|------------------|------|-------|
| s1 | f1 | NULL | S1 | Administrative |
| s1.1 | f1 | s1 | S1.1 | Board Minutes |
| s1.2 | f1 | s1 | S1.2 | Financial |
| s2 | f1 | NULL | S2 | Academic Departments |
| s2.1 | f1 | s2 | S2.1 | History Dept |

**items:**

| id | series_id | fonds_id | reference_code | title |
|----|-----------|----------|----------------|-------|
| i1 | s1.1 | f1 | F1/S1.1/001 | Minutes Jan 1950 |
| i2 | s2.1 | f1 | F1/S2.1/042 | Course catalog 1960 |

## The Key Benefits

1. **Flexible reorganization** - Change `parent_series_id` to move entire branches
2. **Efficient queries** - Indexes on foreign keys make lookups fast
3. **Referential integrity** - Can't orphan records (CASCADE deletes handle this)
4. **Unlimited depth** - Can nest as deep as needed

