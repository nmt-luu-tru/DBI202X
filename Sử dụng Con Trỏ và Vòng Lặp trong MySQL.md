# Sử dụng Con Trỏ và Vòng Lặp trong MySQL

Chúng ta sẽ xem xét cách sử dụng vòng lặp để lấy kết quả từ một con trỏ (cursor) trong MySQL.

### Một Số Lưu Ý về Con Trỏ trong MySQL

Con trỏ trong MySQL không sử dụng bản sao tạm thời của dữ liệu mà trực tiếp làm việc với dữ liệu gốc. Điều này giúp tăng tốc độ truy xuất dữ liệu, nhưng nếu dữ liệu bị thay đổi trong quá trình con trỏ đang duyệt qua, bạn có thể gặp phải vấn đề. Con trỏ trong MySQL cũng chỉ hỗ trợ duyệt một chiều (uni-directional), nghĩa là bạn chỉ có thể duyệt qua các kết quả theo thứ tự từ đầu đến cuối, không thể quay lại.

### Bước 1: Chuẩn Bị Bảng và Dữ Liệu

Trước hết, chúng ta cần một bảng và dữ liệu để truy vấn. Giả sử bạn đã tạo bảng `users` với các cột `email`, `registered`, và `active`.

### Bước 2: Tạo Stored Procedure với Cursor và Vòng Lặp

Dưới đây là cách tạo một stored procedure sử dụng cursor để truy vấn dữ liệu và vòng lặp để duyệt qua các kết quả:

1. **Đặt DELIMITER:**
    ```sql
    DELIMITER $$
    ```

2. **Tạo Stored Procedure:**
    ```sql
    CREATE PROCEDURE fetch_emails()
    BEGIN
        DECLARE finished BOOLEAN DEFAULT FALSE;
        DECLARE user_email VARCHAR(255);
        DECLARE cur CURSOR FOR 
            SELECT email 
            FROM users 
            WHERE active = TRUE 
            AND registered > NOW() - INTERVAL 1 YEAR;

        DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = TRUE;

        OPEN cur;

        fetch_loop: LOOP
            FETCH cur INTO user_email;
            IF finished THEN 
                LEAVE fetch_loop;
            END IF;
            -- Thực hiện hành động với user_email, ví dụ: chèn vào bảng khác
            INSERT INTO leads (email) VALUES (user_email);
        END LOOP;

        CLOSE cur;
    END $$
    ```

3. **Đặt lại DELIMITER:**
    ```sql
    DELIMITER ;
    ```

### Bước 3: Gọi Stored Procedure

Gọi stored procedure `fetch_emails` để chạy và duyệt qua các kết quả:

```sql
CALL fetch_emails();
```

### Giải Thích Mã Lệnh

- **Khai báo biến và con trỏ:** `DECLARE finished BOOLEAN DEFAULT FALSE;` và `DECLARE cur CURSOR FOR ...` khai báo con trỏ cho câu truy vấn.
- **Khai báo trình xử lý:** `DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = TRUE;` khai báo trình xử lý để xử lý trường hợp không tìm thấy kết quả (khi con trỏ đã duyệt hết các hàng).

Handler trong MySQL hoạt động như một "người lắng nghe" hoặc "người giám sát" các điều kiện nhất định trong suốt quá trình thực thi của thủ tục
lưu trữ (stored procedure). Khi một điều kiện đã được định nghĩa xảy ra, handler sẽ tự động kích hoạt và thực hiện các hành động đã được khai báo.  

Handler `DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;` được đăng ký để thực hiện một hành động cụ thể khi điều kiện `NOT FOUND` xảy ra. 
Handler này không thực thi ngay lập tức khi được khai báo mà chỉ kích hoạt khi lệnh `FETCH` trong vòng lặp gặp điều kiện `NOT FOUND`, 
tức là không còn hàng nào để lấy từ con trỏ. Khi điều đó xảy ra, biến done sẽ được thiết lập thành 1, cho phép chương trình kiểm tra và thoát khỏi vòng lặp một cách hợp lý.  

Từ khóa `CONTINUE` xác định loại handler sẽ tiếp tục thực thi các câu lệnh tiếp theo sau khi bắt gặp điều kiện được khai báo. Có hai loại handler khác nhau:  
  \- **CONTINUE**: Tiếp tục thực thi các câu lệnh tiếp theo.  
  \- **EXIT**: Thoát ra khỏi khối `BEGIN...END` ngay lập tức.  

- **Mở và đóng con trỏ:** `OPEN cur;` và `CLOSE cur;`.
- **Vòng lặp:** `LOOP ... END LOOP;` để duyệt qua các kết quả của con trỏ.
- **Lấy kết quả từ con trỏ:** `FETCH cur INTO user_email;` lấy giá trị từ con trỏ và gán vào biến `user_email`. Sau mỗi lần thực hiện lệnh này, con trỏ sẽ tự động dịch chuyển xuống dòng tiếp theo.
- **Kiểm tra điều kiện kết thúc:** `IF finished THEN LEAVE fetch_loop;` kiểm tra nếu con trỏ đã duyệt hết các hàng thì thoát khỏi vòng lặp.
- **Chèn dữ liệu:** `INSERT INTO leads (email) VALUES (user_email);` chèn email vào bảng `leads`.

### Kết Luận

Sử dụng con trỏ và vòng lặp trong MySQL giúp bạn có thể duyệt qua các kết quả của một truy vấn một cách hiệu quả. Hãy thử thực hiện các bước trên và kiểm tra kết quả để hiểu rõ hơn về cách hoạt động của con trỏ trong MySQL.
