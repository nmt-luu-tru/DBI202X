### Giới thiệu
Chúng ta sẽ xem xét về `select for update`.

### Vấn đề với Transactions
Chúng ta đã thấy rằng việc gói các câu lệnh trong một transaction có thể ngăn chặn nhiều vấn đề. Điều này đặc biệt hữu ích khi bạn cần đảm bảo rằng tất cả các câu lệnh đều thành công hoặc không có câu lệnh nào thành công. Ví dụ, trong việc chuyển tiền từ tài khoản này sang tài khoản khác, việc gói các câu lệnh trong một transaction sẽ đảm bảo rằng commit chỉ xảy ra khi mọi thứ đã thành công.

### Ví dụ về Rút Tiền
Hãy tưởng tượng rằng chúng ta đang vận hành một máy ATM và muốn thực hiện một giao dịch rút tiền.

```sql
SET @withdrawal = 500; -- Số tiền rút

SELECT balance FROM accounts WHERE id = @account_id; -- Kiểm tra số dư

UPDATE accounts SET balance = balance - @withdrawal WHERE id = @account_id; -- Thực hiện rút tiền
```

### Vấn đề với Concurrent Transactions
Nếu bạn chạy các câu lệnh này từ nhiều sessions cùng một lúc, có thể xảy ra vấn đề. Ví dụ, cả hai sessions đều kiểm tra số dư và đều thấy đủ tiền để rút, nhưng sau đó cả hai đều thực hiện việc rút tiền, dẫn đến số dư âm.

### Giải pháp: Transactions và Locking
Việc chỉ gói các câu lệnh trong một transaction có thể không đủ để giải quyết vấn đề này. Chúng ta cần một cách để đảm bảo rằng khi một session đang đọc số dư, không session nào khác có thể can thiệp cho đến khi hoàn thành.

### Sử dụng `select for update`
`select for update` là câu lệnh mà chúng ta cần. Nó sẽ đảm bảo rằng các dòng dữ liệu được chọn sẽ bị khóa cho đến khi transaction hoàn thành, ngăn chặn các session khác đọc hoặc thay đổi dữ liệu này.

### Ví dụ với `select for update`
Hãy xem ví dụ dưới đây:

```sql
START TRANSACTION;

SET @withdrawal = 500;
SET @account_id = 1;

SELECT balance FROM accounts WHERE id = @account_id FOR UPDATE; -- Khóa dòng dữ liệu này

-- Kiểm tra số dư và thực hiện rút tiền nếu đủ
IF @balance >= @withdrawal THEN
    UPDATE accounts SET balance = balance - @withdrawal WHERE id = @account_id;
END IF;

COMMIT;
```

### Hoạt động của `select for update`
1. Khi một session thực hiện `select for update`, dòng dữ liệu được chọn sẽ bị khóa.
2. Các session khác sẽ không thể đọc hoặc ghi vào dòng dữ liệu này cho đến khi transaction hoàn thành (commit hoặc rollback).
3. Điều này đảm bảo rằng các session khác không thể can thiệp vào dữ liệu đang được sử dụng bởi transaction hiện tại.

### Kết luận
Sử dụng `select for update` đảm bảo rằng các dòng dữ liệu được bảo vệ khỏi sự can thiệp của các session khác trong suốt quá trình transaction. Điều này rất quan trọng trong việc duy trì tính toàn vẹn và nhất quán của dữ liệu, đặc biệt là trong các giao dịch tài chính.

Hãy thử tự triển khai điều này để hiểu rõ hơn về cách hoạt động của `select for update`.
