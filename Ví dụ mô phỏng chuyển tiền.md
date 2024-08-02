### Giới thiệu
Chúng ta sẽ khám phá các tình huống mà bạn thực sự cần sử dụng transactions.

### Đặt vấn đề: Chuyển tiền
Chúng ta sẽ bắt đầu bằng cách xem xét một phiên bản đơn giản của vấn đề kinh điển là chuyển tiền từ tài khoản này sang tài khoản khác.

### Thiết lập cơ sở dữ liệu
Hãy tạo một bảng gọi là `accounts`, cung cấp cho các tài khoản một ID làm primary key tự động tăng và một balance. Số dư sẽ là một giá trị số với 10 chữ số sau dấu thập phân. Chúng ta sẽ chèn một vài tài khoản với số dư ban đầu để thiết lập ví dụ của chúng ta.

### Thiết lập ban đầu
```sql
CREATE TABLE accounts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    balance DECIMAL(15, 10)
);

INSERT INTO accounts (balance) VALUES (1000.00), (1500.00);
```

### Mô phỏng chuyển tiền
Để chuyển tiền từ tài khoản này sang tài khoản khác, chúng ta có thể sử dụng các câu lệnh SQL sau:
```sql
SET @transfer = 200;

UPDATE accounts SET balance = balance - @transfer WHERE id = 1;
UPDATE accounts SET balance = balance + @transfer WHERE id = 2;
```

### Vấn đề với concurrent transactions
Khi chạy nhiều câu lệnh SQL trong môi trường có các sessions đồng thời, có thể xảy ra nhiều vấn đề:
1. **Thực thi một phần**: Nếu một số câu lệnh được thực thi nhưng các câu lệnh khác thì không (ví dụ: ứng dụng bị lỗi), cơ sở dữ liệu có thể bị để lại ở trạng thái không nhất quán.
2. **Chỉnh sửa đồng thời**: Nhiều sessions có thể thực thi các câu lệnh này cùng lúc, gây ra các điều kiện cạnh tranh và dữ liệu không nhất quán.

### Giải pháp: Sử dụng Transactions
Chúng ta gói các câu lệnh SQL trong một transaction để đảm bảo tính nguyên tử:
```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - @transfer WHERE id = 1;
UPDATE accounts SET balance = balance + @transfer WHERE id = 2;

COMMIT;
```

### Giải thích
1. **Tính nguyên tử**: Cả hai câu lệnh sẽ được commit cùng nhau hoặc không câu lệnh nào được commit. Điều này đảm bảo rằng nếu ứng dụng bị lỗi sau câu lệnh đầu tiên, câu lệnh thứ hai sẽ không để cơ sở dữ liệu ở trạng thái không nhất quán.
2. **Concurrency Control**: Transactions giúp quản lý truy cập đồng thời, đảm bảo rằng nhiều sessions thực thi các câu lệnh này đồng thời không can thiệp vào nhau.

### Kết luận
Sử dụng transactions đảm bảo tính toàn vẹn và nhất quán của cơ sở dữ liệu, đặc biệt khi thực hiện các hoạt động như chuyển tiền giữa các tài khoản. Luôn xem xét:
1. Điều gì xảy ra nếu chỉ một số câu lệnh được thực thi?
2. Điều gì xảy ra nếu nhiều sessions thực thi các câu lệnh này cùng lúc?

Bằng cách gói các hoạt động này trong một transaction, chúng ta có thể giải quyết cả hai vấn đề một cách hiệu quả.

### Lưu ý cuối cùng
Hãy thử tự triển khai điều này để hiểu tầm quan trọng của transactions trong các hoạt động cơ sở dữ liệu.
