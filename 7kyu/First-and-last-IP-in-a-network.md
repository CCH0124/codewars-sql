### Task
Given a table where users' connections are logged, find the first and the last address of the networks they connected from.

### Notes
Order the result by the id column
There's no need to validate anything - it's okay if the user connects from a private network
(You don't need the `connection_time` field for this task but without it the input data looks too dull)
You can read more about IPv4 on [Wikipedia](https://en.wikipedia.org/wiki/IPv4) (check the First and last subnet addresses section if you need an example/explanation related to this task only)

### Input table
```
---------------------------------------------
|    Table    |     Column      |   Type    |
|-------------+-----------------+-----------|
| connections | id              | int       |
|             | connection_time | timestamp |
|             | ip_address      | inet      |
---------------------------------------------
```

### Output table
```
------------------------
|    Column     | Type |
|---------------+------|
| id            | int  |
| first_address | text |
| last_address  | text |
------------------------
```

### Example
For the IP address `182.240.42.115/24` the first address in the network is `182.240.42.0/24`, and the last one is `182.240.42.255/24`.


題意是計算 IP 的網段，和最後廣播位置，官方有提供[函數](http://www.postgresql.cn/docs/13/functions-net.html)進行操作。

```sql
SELECT id, network(ip_address) as first_address, broadcast(ip_address) as last_address 
FROM connections;
```
