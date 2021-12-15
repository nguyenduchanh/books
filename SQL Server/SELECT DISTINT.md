# SELECT DISTINT trong SQL Server
- Đôi khi, bạn có thể chỉ muốn nhận các giá trị riêng biệt trong một cột được chỉ định của bảng
```sh
SELECT
    column_name
FROM
    table_name;
```
- Truy vấn chỉ trả về các giá trị riêng biệt trong cột được chỉ định. Nó loại bỏ các giá trị trùng lặp trong cột khỏi tập kết quả.
- Nếu áp dụng mệnh đề DISTINT cho một cột NULL, mệnh đề DISTINT sẽ chỉ giữ một giá trị NULL và loại bỏ những giá trị NULL khác
Nguồn [OFFET FETCH trong SQL Server](https://comdy.vn/sql-server/offset-fetch-trong-sql-server/)
