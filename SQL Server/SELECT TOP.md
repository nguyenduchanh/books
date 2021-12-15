# SELECT TOP trong SQL Server
- Mệnh đề SELECT TOP cho phép hạn chế số lượng bản ghi hoặc tỷ lệ phần trăm của bản ghi được trả về trong một kết quả truy vấn.
- Vì thứ tự của các bản ghi được lưu trữ trong một bảng là không xác định, nên câu lệnh SELECT TOP luôn được sử dụng với mệnh để ORDER BY. Do đó tập kết quả được giới hạn N bản ghi đầu tiên đã được sắp xếp
```sh
SELECT TOP 10
    product_name,
    list_price
FROM
    production.products
ORDER BY
    list_price DESC;
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```
- Sử dụng TOP để trả về tỷ lệ phần trăm của các bản ghi trong SQL SERVER.
- Ví dụ sau PERCENT để chỉ định số lượng sản phẩm được trả về trong tập kết quả. Bảng production.products có 321 bản ghi, do đó, một phần trăm của 321 là một giá trị thập phân (3.21). SQL Server sẽ làm tròn lên thành 4
``` sh
SELECT TOP 1 PERCENT
    product_name,
    list_price
FROM
    production.products
ORDER BY
    
```
- Sử dụng TOP WITH TIES để bao gồm các bản ghi khớp các giá trị ở bản ghi cuối cùng
``` sh
SELECT TOP 3 WITH TIES
    product_name,
    list_name
FROM
    production.products
ORDER BY
    list_price DESC;
```
Nguồn [OFFET FETCH trong SQL Server](https://comdy.vn/sql-server/select-top-trong-sql-server/)
