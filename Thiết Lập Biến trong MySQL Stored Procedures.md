# Thiết Lập Biến trong MySQL Stored Procedures

Chúng ta sẽ tìm hiểu cú pháp mới để thiết lập giá trị cho các biến trong câu lệnh SQL, đặc biệt là trong stored procedures. 

## Sử dụng Cơ sở Dữ liệu và Bảng Mẫu

Trước tiên, chúng ta cần làm việc với một cơ sở dữ liệu và bảng mẫu. Hãy sử dụng cơ sở dữ liệu `online_shop` và bảng `books` làm ví dụ.

1. **Tạo Database và Bảng Mẫu**:
   ```sql
   CREATE DATABASE IF NOT EXISTS online_shop;
   USE online_shop;

   CREATE TABLE IF NOT EXISTS books (
       id INT AUTO_INCREMENT PRIMARY KEY,
       title VARCHAR(255) NOT NULL
   );

   INSERT INTO books (title) VALUES ('Book 1'), ('Book 2'), ('Book 3'), ('Book 4'), ('Book 5');
   ```

## Thiết lập biến local trong Stored Procedures

Khi bạn muốn lấy giá trị từ một truy vấn trả về một hàng và lưu các giá trị này vào các biến, bạn có thể sử dụng cú pháp `SELECT ... INTO`. Điều này đặc biệt hữu ích trong stored procedures.

1. **Ví dụ về SELECT ... INTO**:
   ```sql
   DELIMITER $$

   CREATE PROCEDURE get_book_details(IN book_id INT)
   BEGIN
       DECLARE book_title VARCHAR(255);

       SELECT title INTO book_title FROM books WHERE id = book_id;

       SELECT book_title;
   END$$

   DELIMITER ;
   ```
Trong ví dụ này, biến `book_title` được khai báo là biến cục bộ bên trong thủ tục và chỉ có thể được truy cập trong phạm vi của thủ tục.

### Gọi Stored Procedure và Kiểm Tra Kết Quả

Bây giờ, chúng ta có thể gọi stored procedure và truyền vào tham số để lấy tiêu đề của sách có `id` nhất định.

1. **Gọi Stored Procedure**:
   ```sql
   CALL get_book_details(1);
   ```

   Kết quả sẽ là tiêu đề của sách có `id` là 1.

### Ví dụ Thực Hành

Hãy thử thực hành bằng cách thay đổi giá trị `book_id` khi gọi stored procedure để hiểu rõ hơn về cách hoạt động của nó.

1. **Gọi Stored Procedure với giá trị khác**:
   ```sql
   CALL get_book_details(2);
   ```

   Kết quả sẽ là tiêu đề của sách có `id` là 2.

### Thiết Lập Nhiều Biến

Bạn cũng có thể thiết lập nhiều biến trong cùng một câu lệnh `SELECT ... INTO`.

1. **Ví dụ về Thiết Lập Nhiều Biến**:
   ```sql
   DELIMITER $$

   CREATE PROCEDURE get_book_info(IN book_id INT)
   BEGIN
       DECLARE book_title VARCHAR(255);
       DECLARE book_id_returned INT;

       SELECT id, title INTO book_id_returned, book_title FROM books WHERE id = book_id;

       SELECT book_id_returned, book_title;
   END$$

   DELIMITER ;
   ```

2. **Gọi Stored Procedure**:
   ```sql
   CALL get_book_info(3);
   ```

   Kết quả sẽ là `id` và tiêu đề của sách có `id` là 3.

## Thiết lập biến session trong Stored Procedures  

```sql
DELIMITER $$

CREATE PROCEDURE get_book_details_session(IN book_id INT)
BEGIN
    SELECT id, title INTO @book_id, @book_title FROM books WHERE id = book_id LIMIT 1;

    SELECT @book_id, @book_title;
END$$

DELIMITER ;
```
Trong ví dụ này, biến @book_id, @book_title là biến session (biến người dùng) và có thể được truy cập bên ngoài thủ tục lưu trữ. Biến session tồn tại trong toàn bộ phiên làm việc của người dùng và có thể được sử dụng bởi các truy vấn khác trong cùng phiên.  

## Sự khác nhau giữa biến local và biến session  

### 1. Sử dụng biến local
```sql
DELIMITER $$

CREATE PROCEDURE get_book_details_local(IN book_id INT)
BEGIN
    DECLARE book_title VARCHAR(255);

    SELECT title INTO book_title FROM books WHERE id = book_id;

    SELECT book_title;
END$$

DELIMITER ;
```

**Thực thi thủ tục và kiểm tra giá trị biến:**
```sql
CALL get_book_details_local(1);
```

Kết quả:
- Bạn sẽ thấy tiêu đề của cuốn sách có `id` là `1`.
- Sau khi thực thi thủ tục, bạn sẽ không thể truy cập giá trị của biến `book_title` bên ngoài thủ tục vì nó là biến cục bộ.

### 2. Sử dụng biến session
```sql
DELIMITER $$

CREATE PROCEDURE get_book_details_session(IN book_id INT)
BEGIN
    SELECT title INTO @book_title FROM books WHERE id = book_id;

    SELECT @book_title;
END$$

DELIMITER ;
```

**Thực thi thủ tục và kiểm tra giá trị biến:**
```sql
CALL get_book_details_session(1);
```

Kết quả:
- Bạn sẽ thấy tiêu đề của cuốn sách có `id` là `1`.
- Sau khi thực thi thủ tục, bạn có thể truy cập giá trị của biến `@book_title` bên ngoài thủ tục vì nó là biến session.

**Kiểm tra giá trị biến session sau khi thủ tục kết thúc:**
```sql
SELECT @book_title;
```

Kết quả:
- Bạn sẽ thấy giá trị của `@book_title` vẫn được lưu trữ và có thể truy cập sau khi thủ tục kết thúc.

### Kết luận
- **Biến cục bộ** (`DECLARE book_title`) chỉ tồn tại trong phạm vi của thủ tục lưu trữ và không thể truy cập bên ngoài.
- **Biến session** (`@book_title`) tồn tại trong toàn bộ phiên làm việc của người dùng và có thể truy cập bởi các truy vấn khác sau khi thủ tục kết thúc.

Điều này cho thấy sự linh hoạt của biến session khi bạn cần giữ lại giá trị sau khi thủ tục hoàn thành, so với biến cục bộ chỉ hữu ích trong phạm vi thủ tục.
