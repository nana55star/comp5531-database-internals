# DBMS Page and Record Storage

## 1. Why Storage Internals Matter

A DBMS does not store tables as simple lists of rows.  
Data is stored in fixed-size **pages (blocks)** on disk (e.g., 4KB, 8KB, 16KB).

Understanding how rows are packed into pages helps explain:
- I/O cost
- Cache efficiency
- Index behavior
- Fragmentation

---

## 2. Pages (Blocks)

A page is the smallest unit of disk I/O.

Example:
- Page size = 1000 bytes
- If each row = 100 bytes → exactly 10 rows per page

Since disk reads happen at the page level, more rows per page means:
- Fewer disk reads
- Better performance

---

## 3. Fixed-Length Records

If all rows have fixed length:

- Row size is predictable
- The DBMS can compute row position directly
- Example:
  - Row size = 100 bytes
  - Page size = 1000 bytes
  - Rows per page = 10

Addressing formula:
RowOffset = PageStart + (RowNumber × RowSize)


Advantages:
- Simple addressing
- No offset directory needed

Disadvantages:
- Possible wasted space due to padding

---

## 4. Variable-Length Records

If rows contain variable-length attributes (e.g., VARCHAR):

- Row sizes differ
- DBMS cannot compute offsets directly
- A **slot directory** is used at the end of the page

The slot directory stores:
- Pointer (offset) to each row
- Row length

Advantages:
- Better space utilization
- More rows per page (on average)

Disadvantages:
- Slightly more management overhead

---

## 5. Impact on Performance

More efficient page utilization leads to:
- Higher row density per page
- Fewer disk I/O operations
- Better buffer cache usage

Storage design decisions affect:
- Query performance
- Index structure
- Page splits
- Fragmentation

---

## Key Insight

Physical storage design (fixed vs variable-length records) directly impacts how efficiently a DBMS can retrieve and manage data at scale.
