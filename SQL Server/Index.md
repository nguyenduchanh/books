# Index trong SQL Server
- Index trong SQL Server là các cấu trúc dữ liệu đặc biệt được liên kết với các bảng hoặc view giúp tăng tốc truy vấn. SQL Server cung cấp hai loại index: clustered index và non-clustered index
1. Clustered Index trong SQL Server
- Bảng không có khóa chính, do đó SQL Server lưu trữ các bản ghi của nó trong một cấu trúc có thứ tự được gọi là heap (đống). Khi bạn truy cập dữ liệu từ bảng, trình tối ưu hóa truy vấn sẽ quét toàn bộ bảng để xác định vị trí chính xác.
- Khi một bảng có chứa một số lượng bản ghi lớn thì sẽ mất nhiều thời gian và tài nguyên để tìm kiếm dữ liệu.
- SQL Server cugn cấp một cấu trúc chuyên dụng để tăng tốc độ truy xuất các bản ghi từ một bảng được gọi index.
- SQL Server có hai loại index là clustered index và non-clustered index.
- Một clustered index lưu trữ các bản ghi dữ liệu trong một cấu trúc được sắp xếp dựa trên các giá trị khóa của nó. Mỗi bảng chỉ có một clustered index vì bản ghi dữ liệu chỉ có thể được săp xếp theo một thứ tự. Bảng có clustered index được gọi là clustered table.
- Một clustered index tổ chức dữ liệu bẳng cách sử dụng một cấu trúc dữ liệu đặc biệt được gọi là B-tree(balance tree - cây cân bằng) cho phép tìm kiếm. chèn, cập nhật và xóa bản ghi bất kỳ với thời gian như nhau.
- Trong cấu trúc này, nút trên cùng của B-tree được gọi là nút gốc (root node). Các nút ở cấp độ dưới cùng được gọi là các nút lá (leaf nodes). Bất kỳ nút nào ở giữa các nút gốc và nút là được gọi là nút trung gian.
- Trong B-tree, nút gốc và nút trung gian chứa các trang chỉ mục để lưu trữ các chỉ mục của các bản ghi. Các nút lá chứa các trang dữ liệu của bảng. Các tragn trong mỗi cấp của index được liên kết bẳng cấu trúc khác gọi là danh sách liên kết đôi. 
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
