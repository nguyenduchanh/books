# MINUS, UNION, UNION ALL và INTERSECT trong SQL Server
1. MINUS
- Sử dụng để kết hợp 2 câu lệnh select, nó trả về các bản ghi chỉ thuộc vào bảng của câu truy vấn đầu tiên, những bản ghi giao nhau và những bản ghi của cầu select thứ 2 thì không dc lấy vào kết quả.
2. UNION
- Bạn viết 2 hay nhiều câu truy vấn SELECT khác nhau nhưng muốn nó trả về một danh sách kết quả duy nhất thì bản phải sử dụng toán tử UNION.
3. UNION ALL
- Trả về tất cả các hàng được chọn bởi một trong hai truy vấn, giữu lại các kết quả trùng.
4. INTERECT
- Lấy ra những bản ghi nào mà nó hiện diện ở xuất hiện trong cả 2 bảng
Nguồn [OFFET FETCH trong SQL Server](https://comdy.vn/sql-server/like-trong-sql-server/)
