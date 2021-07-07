You are given a table `numbers` with just one column, `number`. It holds some numbers that are already ordered.

You need to write a query that makes them un-ordered, as in, every possible ordering should appear equally often.


題意是將 numbers 表中的 number 以非排序方式（希望每次回傳都是隨機位置）回傳。想法就是往 `random` 去想。

```sql
--
SELECT * FROM numbers ORDER BY RANDOM()
```

