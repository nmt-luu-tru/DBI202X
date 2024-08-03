# Truyền tham số vào Stored Procedure trong MySQL

Chúng ta sẽ tìm hiểu cách truyền tham số vào stored procedure. Đây là một chủ đề rất hữu ích và quan trọng trong MySQL.

## Tạo Database và Bảng Mẫu

Trước tiên, chúng ta cần làm việc với một cơ sở dữ liệu và bảng mẫu. Hãy sử dụng cơ sở dữ liệu `online_shop` và bảng `books` làm ví dụ.

1. **Tạo Database**:
   ```sql
   CREATE DATABASE IF NOT EXISTS online_shop;
   USE online_shop;
   ```

2. **Tạo Bảng `books`**:
   ```sql
   CREATE TABLE IF NOT EXISTS books (
       id INT AUTO_INCREMENT PRIMARY KEY,
       title VARCHAR(255) NOT NULL
   );

   INSERT INTO books (title) VALUES ('Book 1'), ('Book 2'), ('Book 3'), ('Book 4'), ('Book 5');
   ```

## Tạo Stored Procedure với Tham Số

Giả sử chúng ta muốn tạo một stored procedure để lấy ra các sách có `id` nhỏ hơn một giá trị nào đó mà chúng ta sẽ truyền vào khi gọi stored procedure.

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER $$

   CREATE PROCEDURE show_books(max_id INT)
   BEGIN
       SELECT * FROM books WHERE id < max_id;
   END$$

   DELIMITER ;
   ```

### Gọi Stored Procedure với Tham Số

Bây giờ chúng ta có thể gọi stored procedure và truyền vào một tham số để lấy các sách có `id` nhỏ hơn giá trị này.

1. **Gọi Stored Procedure**:
   ```sql
   CALL show_books(4);
   ```

   Kết quả sẽ là các sách có `id` nhỏ hơn 4, tức là `Book 1`, `Book 2`, và `Book 3`.

### Ví dụ Thực hành

Hãy thử thay đổi giá trị tham số khi gọi stored procedure để hiểu rõ hơn về cách hoạt động của nó.

1. **Gọi Stored Procedure với giá trị khác**:
   ```sql
   CALL show_books(3);
   ```

   Kết quả sẽ là các sách có `id` nhỏ hơn 3, tức là `Book 1` và `Book 2`.


## Tạo Stored Procedure với Nhiều Tham Số

Giả sử chúng ta muốn tạo một stored procedure để lấy ra các sách có `id` nhỏ hơn một giá trị nào đó và có tiêu đề (title) khớp với giá trị mà chúng ta sẽ truyền vào khi gọi stored procedure.

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER $$

   CREATE PROCEDURE show_books(max_id INT, book_title VARCHAR(50))
   BEGIN
       SELECT * FROM books WHERE id < max_id AND title = book_title;
   END$$

   DELIMITER ;
   ```

### Gọi Stored Procedure với Nhiều Tham Số

Bây giờ chúng ta có thể gọi stored procedure và truyền vào các tham số để lấy các sách có `id` nhỏ hơn một giá trị và có tiêu đề khớp với giá trị này.

1. **Gọi Stored Procedure**:
   ```sql
   CALL show_books(4, 'Book 1');
   ```

   Kết quả sẽ là các sách có `id` nhỏ hơn 4 và tiêu đề là 'Book 1'.

### Ví dụ Thực hành

Hãy thử thay đổi giá trị tham số khi gọi stored procedure để hiểu rõ hơn về cách hoạt động của nó.

1. **Gọi Stored Procedure với giá trị khác**:
   ```sql
   CALL show_books(5, 'Book 2');
   ```

   Kết quả sẽ là các sách có `id` nhỏ hơn 5 và tiêu đề là 'Book 2'.

### Kiểm tra Các Trường Hợp Khác Nhau

1. **Gọi Stored Procedure với `max_id` không đủ lớn để lấy kết quả**:
   ```sql
   CALL show_books(2, 'Book 3');
   ```

   Kết quả sẽ là không có sách nào vì không có sách nào có `id` nhỏ hơn 2 và tiêu đề là 'Book 3'.

2. **Gọi Stored Procedure với `book_title` không khớp**:
   ```sql
   CALL show_books(5, 'Non-existing Book');
   ```

   Kết quả sẽ là không có sách nào vì không có sách nào có tiêu đề là 'Non-existing Book'.

## Parameter Mode

Trong SQL, đặc biệt là trong MySQL khi làm việc với thủ tục lưu trữ (stored procedures), `IN` là một từ khóa được sử dụng để chỉ định rằng một tham số là tham số đầu vào (input parameter). Điều này có nghĩa là giá trị của tham số được truyền vào thủ tục khi nó được gọi.

Ngoài `IN`, còn có hai từ khóa khác tương tự thuộc cùng loại để chỉ định kiểu tham số trong thủ tục lưu trữ:

1. **IN**:
   - **Mô tả**: Tham số đầu vào, giá trị được truyền từ bên ngoài vào thủ tục lưu trữ.
   - **Ví dụ**:
     ```sql
     CREATE PROCEDURE show_books(IN max_id INT)
     BEGIN
         SELECT * FROM books WHERE id < max_id;
     END;
     ```

2. **OUT**:
   - **Mô tả**: Tham số đầu ra, giá trị được trả ra ngoài từ thủ tục lưu trữ.
   - **Ví dụ**:
     ```sql
     CREATE PROCEDURE get_book_count(OUT total_books INT)
     BEGIN
         SELECT COUNT(*) INTO total_books FROM books;
     END;
     ```
   - Trong ví dụ này, `total_books` là tham số đầu ra và sẽ chứa kết quả của câu lệnh SELECT.

3. **INOUT**:
   - **Mô tả**: Tham số vừa đầu vào vừa đầu ra, có thể nhận giá trị từ bên ngoài và cũng có thể trả giá trị ra ngoài.
   - **Ví dụ**:
     ```sql
     CREATE PROCEDURE adjust_price(INOUT price DECIMAL(10,2), IN adjustment DECIMAL(10,2))
     BEGIN
         SET price = price + adjustment;
     END;
     ```
   - Trong ví dụ này, `price` là tham số vừa đầu vào vừa đầu ra, nó nhận giá trị ban đầu từ bên ngoài và sau đó giá trị này được điều chỉnh trong thủ tục.

### Tóm tắt

- **IN**: Tham số đầu vào.
- **OUT**: Tham số đầu ra.
- **INOUT**: Tham số vừa đầu vào vừa đầu ra.

Việc sử dụng đúng các từ khóa này giúp làm rõ vai trò của từng tham số trong thủ tục lưu trữ, giúp mã nguồn rõ ràng và dễ bảo trì hơn.
### So sánh và lựa chọn
- **Tính rõ ràng**: Sử dụng các từ khóa `IN, OUT, INOUT` giúp rõ ràng hơn về mục đích của tham số.
- **Chuẩn hóa**: Trong một số ngữ cảnh và phiên bản của MySQL, sử dụng các từ khóa trên được coi là tốt hơn và chuẩn hóa hơn, mặc dù không bắt buộc.



