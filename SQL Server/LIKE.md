# LIKE trong SQL Server
- Pattern: là một chuỗi các ký tự để tìm kiếm trong cột hoặc biểu thức. Nó có thể bao gồm các ký tự đại diện hợp lệ sau:
1. Ký tự đại diện phần trăm (%): bất kỳ chuỗi nào có 0 hoặc nhiều ký tự.
2. Ký tự đại diện gạch dưới (_): bất kỳ ký tự đơn nào.
3. Ký tự đại diện [danh sách các ký tự]: bất kỳ ký tự đơn nào trong tập đã chỉ định.
4. [ký tự - ký tự]: bất kỳ ký tự đơn nào trong phạm vi chỉ định
5. [^]: Bất kỳ ký tự đơn nào không nằm trong danh sách phạm vi.
```sh
SELECT
    customer_id,
    first_name,
    last_name
FROM
    sales.customers
WHERE
    last_name LIKE '_u%'
ORDER BY
    first_name;
```
Nguồn [OFFET FETCH trong SQL Server](https://comdy.vn/sql-server/like-trong-sql-server/)
