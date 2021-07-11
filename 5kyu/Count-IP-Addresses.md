Given a database of first and last IPv4 addresses, calculate the number of addresses between them (including the first one, excluding the last one).

### Input
```
---------------------------------
|     Table    | Column | Type  |
|--------------+--------+-------|
| ip_addresses | id     | int   |
|              | first  | text  |
|              | last   | text  |
---------------------------------
```
### Output
```
----------------------
|   Column    | Type |
|-------------+------|
| id          | int  |
| ips_between | int  |
----------------------
```
All inputs will be valid IPv4 addresses in the form of strings. The last address will always be greater than the first one.

### Examples
```
   first    |    last     | ips_between
------------+-------------+-------------
 '10.0.0.0' | '10.0.0.50' |      50 
 '10.0.0.0' |  '10.0.1.0' |     256 
'20.0.0.10' |  '20.0.1.0' |     246
```

題意是計算兩個給定 IP 範圍中的 IP 個數。

```sql
SELECT id, (inet(last) - inet(first)) AS ips_between FROM ip_addresses;
```