# JOIN trong SQL Server
1. LEFT JOIN
- Mệnh đề LEFT JOIN cho phép truy cập dữ liệu từ nhiều bảng. Nó trả về tất cả các bản ghi từ bảng bên trái và bản ghi phù hợp từ bảng bên phải.
- Nếu không có bản ghi phù hợp được tìm thấy trong bảng bên phải, các cột của bảng bên phải sẽ có giá trị NULL
2. RIGHT JOIN
- Mệnh đề RIGHT JOIN kết hợp dữ liệu từ hai hoặc nhiều bảng. Mệnh đề RIGHT JOIN bắt đầu chọn dữ liệu từ bảng bên phải và khớp với các bảng bên trái.
- Mệnh đề RIGHT JOIN trả về một tập kết quả bảo gồm tất cả các bản ghi trong bảng bên phải, cho dù chúng có hay không các bản ghi khớp từ bảng bên trái.
- Nếu một bản ghi trong bảng bên phải không có bất kỳ bản ghi khớp nào từ bảng bên trái.
3. CROSS JOIN
- Server để truy bấn dữ liệu từ hai hoặc nhiều bảng không liên quan.
- Mệnh đề CROSS JOIN sẽ nối mỗi bản ghi từ bảng đầu tiên với mỗi bản ghi từ bảng thứ hai. Nói cách khách CROSS JOIN trả về tích ĐỀ CÁC các bản ghi từ hai bảng. CROSS JOIN không thiếp lập mối quan hệ giữu hai bảng được join.
- ví dụ cross join giữa hai bảng A :{1,2,3} vs B: {A,B,C} sẽ được kết quả C:[{1,A},{1,B},{1,C},{2,A},{2,B},{2,C}, {3,A},{3,B},{3,C}]
4. Seft Join
- Self join cho phép join một bảng vào chính nó. Rất hữu ích để truy vấn dữ liệu phân cấp hoặc so sánh các bản ghi trong cùng một bảng.
- Self join sử dụng mệnh đề Inner join hoặc left join. Vì truy vấn sử dụng tham chiếu đến cùng một bảng, nên bí danh bảng 
Nguồn [OFFET FETCH trong SQL Server](https://comdy.vn/sql-server/like-trong-sql-server/)
