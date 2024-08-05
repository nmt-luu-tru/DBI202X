### Giới thiệu về Con Trỏ (Cursors) trong MySQL

Chúng ta sẽ bắt đầu tìm hiểu về con trỏ (cursors) trong MySQL và tôi sẽ giới thiệu một ví dụ cơ bản nhất về con trỏ.
Bạn có thể hình dung rằng nếu bạn muốn duyệt qua các hàng của một bảng, bạn có thể tạo một truy vấn và đưa nó vào một vòng lặp và vòng lặp đó có thể có một bộ đếm để đếm các giá trị như một, hai, ba, bốn, v.v.

Bạn có thể sử dụng giá trị của bộ đếm trong một số cách để chọn, ví dụ, các hàng bằng ID từ một bảng, nhưng như vậy bạn sẽ phải chạy cùng một truy vấn lặp đi lặp lại. Điều này không hiệu quả lắm.

Sẽ giống như nếu bạn có một danh sách mua sắm với các mục được đánh số và bạn muốn duyệt qua danh sách từng mục một, bạn sẽ phải thực hiện cùng một truy vấn lặp đi lặp lại trong vòng lặp với các giá trị khác nhau của khóa chính hoặc bất kỳ giá trị nào khác.

Điều này sẽ giống như việc bạn lấy danh sách, nhìn vào số một, rồi đặt danh sách xuống, sau đó lại nhấc danh sách lên, tìm số hai, rồi lại đặt xuống, và cứ thế tiếp tục.

Khi bạn duyệt qua một danh sách, bạn sẽ sử dụng vị trí của mục trong danh sách đó. Bạn chỉ cần quét qua nó một cách trực quan, từng dòng một, và con trỏ trong SQL cũng tương tự như vậy.

Hãy cùng xem xét điều này.

### Bước 1: Tạo Bảng và Dữ Liệu Mẫu

Trước hết, chúng ta cần một bảng và dữ liệu để truy vấn. Ví dụ, tôi đã tạo bảng `users` với dữ liệu mẫu. Bạn có thể sử dụng lệnh sau để xem dữ liệu trong bảng `users`:

```sql
SELECT * FROM users;
```

### Bước 2: Tạo Stored Procedure với Cursor

Dưới đây là ví dụ về cách tạo một stored procedure để sử dụng con trỏ (cursor) để truy vấn dữ liệu:

1. **Đặt DELIMITER:**
    ```sql
    DELIMITER $$
    ```

2. **Tạo Stored Procedure:**
    ```sql
    CREATE PROCEDURE cursor_test()
    BEGIN
        DECLARE user_email VARCHAR(255); -- Khai báo biến để lưu trữ email từ cursor

        DECLARE cur CURSOR FOR SELECT email FROM users; -- Khai báo cursor cho câu truy vấn

        OPEN cur; -- Mở cursor

        FETCH cur INTO user_email; -- Lấy giá trị từ cursor vào biến user_email

        CLOSE cur; -- Đóng cursor

        SELECT user_email; -- Hiển thị giá trị của user_email
    END $$
    ```

3. **Đặt lại DELIMITER:**
    ```sql
    DELIMITER ;
    ```

### Bước 3: Gọi Stored Procedure

Gọi stored procedure `cursor_test` để chạy và xem kết quả:

```sql
CALL cursor_test();
```

### Giải Thích Mã Lệnh

- **Khai báo biến:** `DECLARE user_email VARCHAR(255);` Khai báo biến `user_email` để lưu trữ giá trị email từ cursor.
- **Khai báo cursor:** `DECLARE cur CURSOR FOR SELECT email FROM users;` Khai báo cursor `cur` với câu truy vấn để chọn email từ bảng `users`.
- **Mở cursor:** `OPEN cur;` Mở cursor để bắt đầu truy vấn dữ liệu.
- **Lấy giá trị từ cursor:** `FETCH cur INTO user_email;` Lấy giá trị từ cursor và gán vào biến `user_email`.
- **Đóng cursor:** `CLOSE cur;` Đóng cursor sau khi hoàn thành truy vấn.
- **Hiển thị giá trị:** `SELECT user_email;` Hiển thị giá trị của biến `user_email`.

### Kiểm Tra Kết Quả

Sau khi gọi stored procedure `cursor_test`, bạn sẽ thấy giá trị email **đầu tiên** từ bảng `users`.
