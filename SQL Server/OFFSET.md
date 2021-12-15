# OFFET FETCH trong SQL Server
- Các mênh đề OFFSET và FETCH là các tùy chọn của mệnh đề ORDER BY. Chúng cho phép giới hạn số lượng bản ghi được trả về bởi 1 truy vấn
```sh
SELECT
    product_name,
    list_price
FROM
    production.products
ORDER BY
    list_price,
    product_name
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```
Nguồn [OFFET FETCH trong SQL Server](https://comdy.vn/sql-server/offset-fetch-trong-sql-server/)
