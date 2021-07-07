For this challenge you need to create a simple SELECT statement. Your task is to create a query and do a FULL TEXT SEARCH. You must search the `product` table on the field `name` for the word `Awesome` and return each row with the given word. Your query MUST contain `to_tsvector` and `to_tsquery` PostgreSQL functions.

### Tables and relationship below:

![](http://i.imgur.com/kBkwsbi.png)

題意是從 `product` 表中的 `name` 搜尋有關 `Awesome` 的句子。

```sql
-- Create your SELECT statement here
SELECT * FROM product WHERE to_tsvector(name)  @@ to_tsquery('Awesome');
```

不過這題的函數沒有用過所以有特別查過，`to_tsvector` 很像 Elasticsearch 的倒排索引。

- [參考資源](https://www.compose.com/articles/mastering-postgresql-tools-full-text-search-and-phrase-search/)