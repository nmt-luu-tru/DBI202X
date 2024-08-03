# Sử Dụng Tham Số INOUT Trong Stored Procedures MySQL

Chúng ta sẽ tìm hiểu về cách sử dụng tham số INOUT trong stored procedures của MySQL. Tham số này giúp chúng ta có thể truyền và nhận giá trị từ stored procedure.

## Tạo Database và Bảng Mẫu

Trước tiên, chúng ta cần một cơ sở dữ liệu và một bảng mẫu để thử nghiệm. Hãy sử dụng cơ sở dữ liệu `test` và bảng `books`.

```sql
CREATE DATABASE IF NOT EXISTS test;
USE test;

CREATE TABLE IF NOT EXISTS books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL
);

INSERT INTO books (title) VALUES ('Book 1'), ('Book 2'), ('Book 3'), ('Book 4'), ('Book 5'), ('Book 6');
```

## Nhắc lại sử dụng Tham Số IN, OUT


#### Stored Procedure với Tham Số IN

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER //

   CREATE PROCEDURE count_books(IN max_id INT)
   BEGIN
       SELECT COUNT(*) FROM books WHERE id <= max_id;
   END //

   DELIMITER ;
   ```

2. **Gọi Stored Procedure**:
   ```sql
   CALL count_books(3);
   ```

   Kết quả sẽ trả về 3 (số lượng sách có `id` nhỏ hơn hoặc bằng 3).

#### Stored Procedure với Tham Số OUT

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER //

   CREATE PROCEDURE get_book_count(OUT book_count INT)
   BEGIN
       SELECT COUNT(*) INTO book_count FROM books;
   END //

   DELIMITER ;
   ```

2. **Gọi Stored Procedure**:
   ```sql
   SET @count = 0;
   CALL get_book_count(@count);
   SELECT @count;
   ```

   Kết quả sẽ là 6 - tổng số sách trong bảng `books`.

## Stored Procedure với Tham Số INOUT

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER //

   CREATE PROCEDURE increment_count(INOUT count INT)
   BEGIN
       SET count = count + 1;
   END //

   DELIMITER ;
   ```

2. **Gọi Stored Procedure**:
   ```sql
   SET @count = 5;
   CALL increment_count(@count);
   SELECT @count;
   ```

   Kết quả sẽ là 6, vì giá trị ban đầu của `count` là 5 và được tăng lên 1 bởi stored procedure.

### Ví dụ khác

Giờ chúng ta sẽ tạo một stored procedure sử dụng tham số INOUT để kết hợp chức năng của cả IN và OUT.

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER //

   CREATE PROCEDURE update_count(INOUT count INT)
   BEGIN
       SET count = count + 1;
       SELECT COUNT(*) INTO count FROM books WHERE id <= count;
   END //

   DELIMITER ;
   ```

2. **Gọi Stored Procedure**:
   ```sql
   SET @count = 3;
   CALL update_count(@count);
   SELECT @count;
   ```

   Kết quả sẽ là 4, vì giá trị ban đầu của `count` là 3, tăng lên 4 và sau đó số lượng sách có `id` nhỏ hơn hoặc bằng 4.

### Kết Luận

**Tham Số INOUT**: Kết hợp cả hai chức năng của IN và OUT. Giá trị này được truyền vào, có thể thay đổi bên trong stored procedure và trả về giá trị mới sau khi thực hiện.

Hãy thử thực hành các bước trên để hiểu rõ hơn về cách hoạt động của các loại tham số này trong stored procedures.
