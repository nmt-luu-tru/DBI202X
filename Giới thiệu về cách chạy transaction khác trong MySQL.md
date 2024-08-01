### Giới thiệu về cách chạy transaction khác trong MySQL

Trong hướng dẫn này, chúng ta sẽ xem xét một cách khác để chạy các transaction.

### Mở các kết nối và thiết lập chế độ auto commit

Chúng ta sẽ mở hai kết nối mới với các thiết lập mặc định. Theo mặc định, auto commit sẽ được đặt thành `true`, tức là chế độ auto commit sẽ được bật. Điều này có nghĩa là nếu bạn cập nhật một bảng, ví dụ như bảng `test`, và thực hiện lệnh:
```sql
SELECT * FROM books;
UPDATE books SET name = 'Farly' WHERE id = 4;
```
Khi bạn thực hiện lệnh này với engine InnoDB (mặc định trong MySQL hiện tại), lệnh này sẽ chạy trong một transaction và sẽ được commit ngay lập tức.

### Cách thay đổi hành vi của transaction

Chúng ta có thể thay đổi hành vi này bằng cách sử dụng lệnh `START TRANSACTION`. Bạn có thể để chế độ auto commit như cũ và sử dụng lệnh `START TRANSACTION` để bắt đầu một transaction mới:
```sql
START TRANSACTION;
```
Sau đó, bạn có thể thực hiện nhiều câu lệnh SQL tùy thích. Cuối cùng, khi bạn muốn áp dụng các thay đổi vào database, bạn sẽ sử dụng lệnh `COMMIT` hoặc nếu bạn không muốn áp dụng các thay đổi, bạn sẽ sử dụng lệnh `ROLLBACK` để huỷ bỏ chúng.

### Ví dụ cụ thể

1. **Bắt đầu transaction và thực hiện các thao tác:**
   ```sql
   START TRANSACTION;
   -- Thực hiện các câu lệnh SQL khác nhau
   SELECT * FROM books;
   UPDATE books SET name = 'The Valley' WHERE id = 4;
   -- Áp dụng thay đổi vào database
   COMMIT;
   ```

2. **Thử nghiệm với hai kết nối:**
   - Kết nối thứ nhất:
     ```sql
     START TRANSACTION;
     SELECT * FROM books;
     UPDATE books SET name = 'The Valley' WHERE id = 4;
     COMMIT;
     ```
   - Kết nối thứ hai:
     ```sql
     START TRANSACTION;
     SELECT * FROM books;
     -- Kiểm tra xem có thấy thay đổi từ kết nối thứ nhất không
     ```

Theo mặc định, bạn sẽ không thấy các thay đổi từ kết nối khác cho đến khi transaction được commit. 

### Thử nghiệm và khám phá

Bạn nên thử nghiệm các thao tác này để hiểu rõ hơn về cách transaction hoạt động. Mở hai kết nối riêng biệt trong MySQL Workbench, bắt đầu transaction ở cả hai kết nối, thực hiện các thay đổi ở một kết nối và kiểm tra bảng ở kết nối còn lại.
