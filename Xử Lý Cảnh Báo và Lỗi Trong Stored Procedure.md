# Xử Lý Cảnh Báo và Lỗi Trong Thủ Tục Lưu Trữ MySQL

Chúng ta sẽ tìm hiểu cách sửa đổi thủ tục lưu trữ của chúng ta để xử lý lỗi và cảnh báo. Khi thực thi thủ tục, có thể xảy ra các lỗi hoặc cảnh báo, và chúng ta cần biết cách xử lý chúng một cách khéo léo để đảm bảo rằng thủ tục của chúng ta hoạt động ổn định và an toàn.

### Hiểu về Lỗi và Cảnh Báo

- **Lỗi (Error)**: Một vấn đề nghiêm trọng ngăn chặn việc thực thi SQL, chẳng hạn như tham chiếu đến một bảng không tồn tại.
- **Cảnh báo (Warning)**: Một vấn đề ít nghiêm trọng hơn nhưng vẫn chỉ ra rằng có điều gì đó không hoạt động như mong muốn, nhưng không ngăn chặn hoàn toàn việc thực thi.

### Ví dụ Xử Lý Lỗi

1. **Ví dụ Lỗi**:
    ```sql
    USE test;
    SELECT * FROM nonexistent_table;
    ```
    Điều này sẽ tạo ra một lỗi vì bảng `nonexistent_table` không tồn tại.

    ```sql
    SHOW ERRORS;
    ```
    Điều này sẽ hiển thị các lỗi gần đây.

### Ví dụ Xử Lý Cảnh Báo

2. **Ví dụ Cảnh Báo**:
    ```sql
    SELECT id, title FROM books WHERE id = 7;
    ```
    Nếu không có hàng nào với `id = 7`, nó sẽ dẫn đến một cảnh báo.

    ```sql
    SHOW WARNINGS;
    ```
    Điều này sẽ hiển thị các cảnh báo gần đây.

### Triển Khai Xử Lý Lỗi và Cảnh Báo Trong Thủ Tục Lưu Trữ

Chúng ta sẽ chỉnh sửa thủ tục `withdraw_money` của mình để xử lý lỗi và cảnh báo.

### Quy Trình Từng Bước

1. **Khai Báo Biến**:
    ```sql
    DELIMITER //

    CREATE PROCEDURE withdraw_money(
        IN account_id INT, 
        IN amount DECIMAL(10, 2), 
        OUT success BOOL
    )
    BEGIN
        DECLARE current_balance DECIMAL(10, 2) DEFAULT 0.00;
        
        -- Xử lý lỗi
        DECLARE EXIT HANDLER FOR SQLEXCEPTION
        BEGIN
            SET success = FALSE;
            ROLLBACK;
            SELECT 'Error occurred' AS ErrorMessage;
        END;
        
        -- Xử lý cảnh báo
        DECLARE EXIT HANDLER FOR SQLWARNING
        BEGIN
            SET success = FALSE;
            ROLLBACK;
            SELECT 'Warning occurred' AS WarningMessage;
        END;

        -- Bắt đầu transaction
        START TRANSACTION;
        
        -- Chọn số dư hiện tại và khóa hàng
        SELECT balance INTO current_balance 
        FROM accounts 
        WHERE id = account_id 
        FOR UPDATE;

        -- Kiểm tra nếu số dư đủ để rút tiền
        IF current_balance >= amount THEN
            -- Cập nhật số dư
            UPDATE accounts 
            SET balance = balance - amount 
            WHERE id = account_id;
            SET success = TRUE;
        ELSE
            -- Không đủ tiền
            SET success = FALSE;
        END IF;

        -- Commit transaction
        COMMIT;
    END //

    DELIMITER ;
    ```

2. **Gọi Thủ Tục**:
    ```sql
    -- Đặt một biến session để giữ trạng thái thành công
    SET @success = FALSE;

    -- Gọi thủ tục lưu trữ với ID tài khoản, số tiền cần rút, và trạng thái thành công
    CALL withdraw_money(1, 50.00, @success);

    -- Kiểm tra trạng thái thành công
    SELECT @success;

    -- Kiểm tra số dư tài khoản
    SELECT * FROM accounts WHERE id = 1;
    ```

### Giải Thích

- **Trình Xử Lý Lỗi và Cảnh Báo**: Các lệnh `DECLARE EXIT HANDLER FOR SQLEXCEPTION` và `DECLARE EXIT HANDLER FOR SQLWARNING` định nghĩa các trình xử lý cho lỗi và cảnh báo SQL. Nếu có bất kỳ lỗi hoặc cảnh báo nào xảy ra, các trình xử lý này sẽ được kích hoạt, đặt biến `success` thành `FALSE` và rollback transaction.
- **Quản Lý Transaction**: Các lệnh `START TRANSACTION` và `COMMIT` đảm bảo rằng việc cập nhật số dư là nguyên tử. Nếu có bất kỳ lỗi hoặc cảnh báo nào xảy ra trong quá trình transaction, nó sẽ được rollback.

### Kiểm Tra

- **Rút Tiền Hợp Lệ**:
    ```sql
    SET @success = FALSE;
    CALL withdraw_money(1, 50.00, @success);
    SELECT @success;
    SELECT * FROM accounts WHERE id = 1;
    ```

- **Rút Tiền Không Hợp Lệ (Không Đủ Tiền)**:
    ```sql
    SET @success = FALSE;
    CALL withdraw_money(1, 1000.00, @success);
    SELECT @success;
    SELECT * FROM accounts WHERE id = 1;
    ```

### Kết Luận

Xử lý lỗi và cảnh báo trong thủ tục lưu trữ là rất quan trọng để xây dựng các ứng dụng cơ sở dữ liệu đáng tin cậy. Bằng cách sử dụng quản lý transaction và định nghĩa các trình xử lý lỗi và cảnh báo phù hợp, chúng ta có thể đảm bảo rằng các thủ tục của chúng ta xử lý các tình huống bất ngờ một cách khéo léo.
