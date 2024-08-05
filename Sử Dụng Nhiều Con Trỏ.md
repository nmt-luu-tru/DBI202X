# Sử dụng Nhiều Cursors

Tôi sẽ giới thiệu về cách sử dụng nhiều cursors và phạm vi biến trong MySQL. Đây là một kỹ thuật quan trọng khi bạn muốn làm việc với nhiều bảng trong một stored procedure.

### Tạo Bảng và Dữ Liệu

Trước tiên, chúng ta cần tạo các bảng và thêm dữ liệu vào bảng `books` và `fruits`. Sau đó, chúng ta sẽ tạo stored procedure để thực hiện thao tác với các bảng này.

```sql
CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(40)
);

CREATE TABLE fruits (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(40)
);
```

### Tạo Stored Procedure

Dưới đây là cách tạo stored procedure `multiple_cursors` để thực hiện thao tác với nhiều cursors:

1. **Đặt DELIMITER:**
    ```sql
    DELIMITER $$
    ```

2. **Tạo Stored Procedure:**
    ```sql
    CREATE PROCEDURE multiple_cursors()
    BEGIN
        DECLARE finished BOOLEAN DEFAULT FALSE;
        DECLARE book_title VARCHAR(40);
        DECLARE fruit_name VARCHAR(40);
        DECLARE books_cursor CURSOR FOR SELECT title FROM books;
        DECLARE fruits_cursor CURSOR FOR SELECT name FROM fruits;
        DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = TRUE;

        -- Begin block for books
        BEGIN
            DECLARE books_titles LONGTEXT DEFAULT '';
            SET finished = FALSE;

            OPEN books_cursor;
            books_loop: LOOP
                FETCH books_cursor INTO book_title;
                IF finished THEN
                    LEAVE books_loop;
                END IF;
                SET books_titles = CONCAT(books_titles, book_title, ', ');
            END LOOP books_loop;
            CLOSE books_cursor;

            SELECT books_titles;
        END;

        -- Begin block for fruits
        BEGIN
            DECLARE fruits_names LONGTEXT DEFAULT '';
            SET finished = FALSE;

            OPEN fruits_cursor;
            fruits_loop: LOOP
                FETCH fruits_cursor INTO fruit_name;
                IF finished THEN
                    LEAVE fruits_loop;
                END IF;
                SET fruits_names = CONCAT(fruits_names, fruit_name, ', ');
            END LOOP fruits_loop;
            CLOSE fruits_cursor;

            SELECT fruits_names;
        END;
    END $$
    ```

3. **Đặt lại DELIMITER:**
    ```sql
    DELIMITER ;
    ```

### Gọi Stored Procedure

Gọi stored procedure `multiple_cursors` để chạy và duyệt qua các kết quả:

```sql
CALL multiple_cursors();
```

### Giải Thích Mã Lệnh

- **Khai báo biến và con trỏ:** `DECLARE books_cursor CURSOR FOR SELECT title FROM books;` và `DECLARE fruits_cursor CURSOR FOR SELECT name FROM fruits;` khai báo các con trỏ cho các câu truy vấn.
- **Khai báo trình xử lý:** `DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = TRUE;` khai báo trình xử lý để xử lý trường hợp không tìm thấy kết quả (khi con trỏ đã duyệt hết các hàng).
- **Mở và đóng con trỏ:** `OPEN books_cursor;` và `CLOSE books_cursor;`.
- **Vòng lặp:** `LOOP ... END LOOP;` để duyệt qua các kết quả của con trỏ.
- **Lấy kết quả từ con trỏ:** `FETCH books_cursor INTO book_title;` lấy giá trị từ con trỏ và gán vào biến tương ứng.
- **Kiểm tra điều kiện kết thúc:** `IF finished THEN LEAVE books_loop;` kiểm tra nếu con trỏ đã duyệt hết các hàng thì thoát khỏi vòng lặp.
- **Chèn dữ liệu:** `SET books_titles = CONCAT(books_titles, book_title, ', ');` nối chuỗi tiêu đề sách vào biến `books_titles`.

### Kết Luận

Sử dụng nhiều con trỏ và phạm vi biến trong MySQL giúp bạn có thể duyệt qua các kết quả của nhiều truy vấn một cách hiệu quả. Hãy thử thực hiện các bước trên và kiểm tra kết quả để hiểu rõ hơn về cách hoạt động của con trỏ và phạm vi biến trong MySQL.
