
# Sử dụng Hàm RAND trong MySQL

Hàm `RAND()` trong MySQL sẽ tạo ra một giá trị ngẫu nhiên từ 0 đến 1. Bạn có thể sử dụng hàm này để tạo ra các giá trị ngẫu nhiên cho bảng dữ liệu thử nghiệm của bạn.

### Ví dụ về Stored Procedure Tạo Dữ Liệu Thử Nghiệm

Chúng ta sẽ tạo một stored procedure để tạo bảng và chèn dữ liệu thử nghiệm vào bảng đó. Bảng này sẽ có các cột:
- `id` - Khóa chính tự tăng.
- `email` - Email giả lập theo định dạng `user1@kaferprogramming.com`.
- `created_date` - Ngày ngẫu nhiên trong khoảng từ hôm nay đến 10000 ngày trước.
- `enabled` - Giá trị boolean ngẫu nhiên (true/false).

#### Tạo Bảng Dữ Liệu

```sql
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255),
    created_date DATE,
    enabled BOOLEAN
);
```

#### Tạo Stored Procedure

Dưới đây là ví dụ về stored procedure để tạo bảng và chèn dữ liệu thử nghiệm vào bảng:

```sql
DELIMITER //

CREATE PROCEDURE generate_test_data()
BEGIN
    DECLARE i INT DEFAULT 0;
    DECLARE random_email VARCHAR(255);
    DECLARE random_date DATE;
    DECLARE random_boolean BOOLEAN;

    -- Xóa bảng nếu đã tồn tại
    DROP TABLE IF EXISTS users;

    -- Tạo bảng mới
    CREATE TABLE users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        email VARCHAR(255),
        created_date DATE,
        enabled BOOLEAN
    );

    -- Vòng lặp để chèn 10000 dòng dữ liệu
    WHILE i < 10000 DO
        SET random_email = CONCAT('user', i + 1, '@kaferprogramming.com');
        SET random_date = DATE_SUB(CURDATE(), INTERVAL FLOOR(RAND() * 10000) DAY);
        SET random_boolean = ROUND(RAND());

        INSERT INTO users (email, created_date, enabled) VALUES (random_email, random_date, random_boolean);

        SET i = i + 1;
    END WHILE;
END //

DELIMITER ;
```

### Gọi Stored Procedure

Để gọi stored procedure này và tạo dữ liệu thử nghiệm, bạn sử dụng:

```sql
CALL generate_test_data();
```

### Kết Quả

Stored procedure này sẽ tạo ra bảng `users` và chèn 10000 dòng dữ liệu với các giá trị ngẫu nhiên vào bảng đó.

### Bài Tập

Hãy thử tạo stored procedure theo ví dụ trên và kiểm tra kết quả. Bạn có thể thử thay đổi số lượng dòng dữ liệu hoặc các giá trị ngẫu nhiên để phù hợp với yêu cầu của mình.
