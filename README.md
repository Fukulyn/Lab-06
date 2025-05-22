# Lab-06 SQL查詢優化練習
## 1. 查詢與效能運算練習
#### 1-1 列出「過去 6 個月未曾進場過的會員」的 member_id 與 name。
- 1.NOT EXISTS
```
EXPLAIN SELECT member_id, name
FROM Members m
WHERE NOT EXISTS (
    SELECT 1
    FROM Registrations r
    WHERE r.member_id = m.member_id
      AND r.entry_time IS NOT NULL
      AND r.entry_time >= DATE_SUB(NOW(), INTERVAL 6 MONTH)
)
```
- 2.NOT IN
```
EXPLAIN SELECT member_id, name
FROM Members m
WHERE NOT EXISTS (
    SELECT 1
    FROM Registrations r
    WHERE r.member_id = m.member_id
      AND r.entry_time IS NOT NULL
      AND r.entry_time >= DATE_SUB(NOW(), INTERVAL 6 MONTH)
)
```
- 3.LEFT JOIN ... IS NULL
```
EXPLAIN SELECT m.member_id, m.name
FROM Members m
LEFT JOIN (
    SELECT DISTINCT member_id
    FROM Registrations
    WHERE entry_time IS NOT NULL
      AND entry_time >= DATE_SUB(NOW(), INTERVAL 6 MONTH)
) r ON m.member_id = r.member_id
WHERE r.member_id IS NULL
```
