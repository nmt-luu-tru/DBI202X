# Sử Dụng Biến OUT Trong Stored Procedure MySQL

Chúng ta sẽ tìm hiểu cách sử dụng biến OUT trong stored procedure để lấy dữ liệu từ stored procedure và lưu trữ vào các biến.

## Tạo Database và Bảng Mẫu

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

## Tạo Stored Procedure với Biến OUT

Chúng ta sẽ tạo một stored procedure có thể nhận một ID sách và trả về tiêu đề của sách đó thông qua biến OUT.

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER $$

   CREATE PROCEDURE get_book_info(IN book_id INT, OUT book_title VARCHAR(255))
   BEGIN
       SELECT title INTO book_title FROM books WHERE id = book_id;
   END$$

   DELIMITER ;
   ```

### Gọi Stored Procedure và Sử Dụng Biến OUT

Bây giờ, chúng ta sẽ gọi stored procedure và sử dụng biến OUT để lấy tiêu đề của sách.

1. **Gọi Stored Procedure**:
   ```sql
   -- Khai báo biến để lưu trữ kết quả
   SET @title = '';

   -- Gọi stored procedure và truyền tham số
   CALL get_book_info(1, @title);

   -- Kiểm tra kết quả
   SELECT @title;
   ```

   Kết quả sẽ là tiêu đề của sách có `id` là 1.
   
khi bạn khai báo một biến như SET @title = '';, bạn không cần phải chỉ định kiểu dữ liệu cụ thể như CHAR hoặc VARCHAR hay xác định số lượng ký tự tối đa.
   MySQL sẽ tự động xác định kiểu dữ liệu và kích thước cần thiết dựa trên giá trị mà bạn gán cho biến đó.

Có thể không cần khởi tạo SET @title = '' . MySQL sẽ tạo ra các biến này khi stored procedure được gọi nếu chúng chưa tồn tại.
   Tuy nhiên khuyến nghị nên khởi tạo để đảm bảo rằng các biến tồn tại và có giá trị trước khi gọi stored procedure.
   Điều này giúp tránh các lỗi tiềm ẩn nếu các biến chưa được khởi tạo.

### Ví Dụ Thực Hành

Hãy thử thay đổi giá trị `book_id` khi gọi stored procedure để hiểu rõ hơn về cách hoạt động của nó.

1. **Gọi Stored Procedure với Giá Trị Khác**:
   ```sql
   -- Khai báo biến để lưu trữ kết quả
   SET @title = '';

   -- Gọi stored procedure và truyền tham số
   CALL get_book_info(2, @title);

   -- Kiểm tra kết quả
   SELECT @title;
   ```

   Kết quả sẽ là tiêu đề của sách có `id` là 2.

## Kiểm Tra Các Trường Hợp Khác Nhau

1. **Gọi Stored Procedure với `book_id` không tồn tại**:
   ```sql
   -- Khai báo biến để lưu trữ kết quả
   SET @title = '';

   -- Gọi stored procedure và truyền tham số
   CALL get_book_info(10, @title);

   -- Kiểm tra kết quả
   SELECT @title;
   ```

   **Kết quả** sẽ là NULL vì không có sách nào có `id` là 10.

## Kết Luận

Việc sử dụng biến OUT trong stored procedure cho phép bạn lấy dữ liệu từ stored procedure và lưu trữ vào các biến để sử dụng sau này. Đây là một kỹ năng quan trọng để tạo ra các stored procedures phức tạp và linh hoạt hơn trong MySQL. Hãy thử thực hành các bước trên để hiểu rõ hơn về cách sử dụng biến OUT trong stored procedures.
