## Sử Dụng Câu Lệnh IF Trong Stored Procedures MySQL

Chúng ta sẽ tìm hiểu về cách sử dụng câu lệnh IF trong stored procedures của MySQL. Đây là một bước quan trọng khi chúng ta bắt đầu tiếp cận lập trình trong MySQL.

### Tạo Database và Bảng Mẫu

Đầu tiên, chúng ta cần một cơ sở dữ liệu và một bảng mẫu để thử nghiệm. Hãy sử dụng cơ sở dữ liệu `test` và bảng `accounts`.

```sql
CREATE DATABASE IF NOT EXISTS test;
USE test;

CREATE TABLE IF NOT EXISTS accounts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    balance DECIMAL(10, 2) NOT NULL
);

INSERT INTO accounts (balance) VALUES (1000.00), (500.00), (300.00);
```

### Tạo Stored Procedure Với Câu Lệnh IF

Bây giờ, chúng ta sẽ tạo một stored procedure đơn giản để minh họa cách sử dụng câu lệnh IF. Stored procedure này sẽ kiểm tra một cờ (flag) và thực hiện hành động tùy thuộc vào giá trị của cờ đó.

1. **Tạo Stored Procedure**:
   ```sql
   DELIMITER //

   CREATE PROCEDURE check_flag(IN flag BOOLEAN)
   BEGIN
       IF flag THEN
           SELECT 'Hello';
       ELSE
           SELECT 'Goodbye';
       END IF;
   END //

   DELIMITER ;
   ```

2. **Gọi Stored Procedure**:
   ```sql
   CALL check_flag(TRUE);   -- Kết quả: Hello
   CALL check_flag(FALSE);  -- Kết quả: Goodbye
   ```

### Ví Dụ Thực Tế: Rút Tiền Từ Tài Khoản

Bây giờ, chúng ta sẽ tạo một stored procedure thực tế hơn để rút tiền từ tài khoản. Stored procedure này sẽ kiểm tra số dư trong tài khoản trước khi rút tiền để đảm bảo rằng không rút quá số tiền hiện có.

1. **Tạo Stored Procedure Rút Tiền**:
   ```sql
   DELIMITER //

   CREATE PROCEDURE withdraw_money(IN account_id INT, IN amount DECIMAL(10, 2))
   BEGIN
       DECLARE current_balance DECIMAL(10, 2);

       -- Lấy số dư hiện tại của tài khoản
       SELECT balance INTO current_balance FROM accounts WHERE id = account_id;

       -- Kiểm tra số dư và thực hiện rút tiền nếu đủ tiền
       IF current_balance >= amount THEN
           UPDATE accounts SET balance = balance - amount WHERE id = account_id;
           SELECT 'Withdrawal successful' AS message;
       ELSE
           SELECT 'Insufficient funds' AS message;
       END IF;
   END //

   DELIMITER ;
   ```

2. **Gọi Stored Procedure Rút Tiền**:
   ```sql
   CALL withdraw_money(1, 200.00);  -- Kết quả: Withdrawal successful
   CALL withdraw_money(2, 600.00);  -- Kết quả: Insufficient funds
   ```

### Giải Thích

- **DECLARE**: Khai báo biến `current_balance` để lưu trữ số dư hiện tại của tài khoản.
- **SELECT INTO**: Lấy số dư hiện tại của tài khoản và lưu vào biến `current_balance`.
- **IF**: Kiểm tra nếu số dư hiện tại lớn hơn hoặc bằng số tiền cần rút (`amount`), thì thực hiện cập nhật số dư. Ngược lại, thông báo rằng không đủ tiền.

### Kết Luận

Chúng ta đã tìm hiểu cách sử dụng câu lệnh IF trong stored procedures của MySQL thông qua một ví dụ đơn giản và một ví dụ thực tế hơn về việc rút tiền từ tài khoản. Việc sử dụng câu lệnh IF giúp chúng ta thực hiện các kiểm tra và điều kiện logic trong stored procedures, làm cho chúng trở nên mạnh mẽ và linh hoạt hơn.
