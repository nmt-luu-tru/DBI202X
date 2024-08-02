### Giới thiệu về Safe Points trong MySQL

Chúng ta sẽ tìm hiểu về một tính năng hữu ích trong các giao dịch MySQL: **Safe Points**.

### Cài đặt môi trường

Chúng ta sẽ sử dụng một cơ sở dữ liệu thử nghiệm với bảng `books` và thiết lập mức độ cô lập mặc định.

### Bắt đầu với Safe Points

1. **Kiểm tra mức độ cô lập hiện tại**:
   ```sql
   SELECT @@session.tx_isolation;
   ```

2. **Bắt đầu một giao dịch**:
   ```sql
   START TRANSACTION;
   ```

3. **Thêm dữ liệu vào bảng `books`**:
   ```sql
   INSERT INTO books (name) VALUES ('The Horror');
   SELECT * FROM books;
   -- Sẽ thấy sách mới được thêm vào trong giao dịch này
   ```

4. **Tạo Safe Point**:
   ```sql
   SAVEPOINT test1;
   ```

### Sử dụng Safe Points

1. **Thực hiện các thay đổi thêm**:
   ```sql
   UPDATE books SET name = 'The Forest' WHERE id = 1;
   SELECT * FROM books;
   -- Thay đổi tên sách có id = 1 thành 'The Forest'
   ```

2. **Rollback về Safe Point**:
   ```sql
   ROLLBACK TO SAVEPOINT test1;
   SELECT * FROM books;
   -- Quay trở lại trạng thái trước khi cập nhật tên sách
   ```

3. **Kết thúc giao dịch**:
   ```sql
   COMMIT;
   ```

### Tác động của Safe Points tới lock

1. **User root: Bắt đầu một giao dịch và tạo Safe Point**:
   ```sql
   START TRANSACTION;
   SELECT * FROM books;
   SAVEPOINT test1;
   -- Lúc này chưa dòng nào bị khóa
   UPDATE books SET name = 'The Amazing Universe' WHERE id = 1;
   SELECT * FROM books;
   -- Thay đổi tên sách có id = 1 thành 'The Amazing Universe'
   -- Lúc này dòng có id = 1 bị khóa
   ROLLBACK TO SAVEPOINT test1;
   SELECT * FROM books;
   -- Quay trở lại trạng thái trước khi cập nhật tên sách
   -- Chưa commit
   -- Lúc này dòng có id = 1 vẫn bị khóa
   ```

2. **User 1: Kiểm tra khóa từ một phiên khác**:
     ```sql
     START TRANSACTION;
     UPDATE books SET name = 'The Amazing Galaxy' WHERE id = 1;
     -- Giao dịch bị treo vì dòng này đang bị khóa
     ```

3. **User root:**:
     ```sql
     COMMIT;
     -- Giao dịch kết thúc và giải phóng khóa
     ```
4. **User 1:**
   ```sql
   -- Giao dịch lúc này mới được thực thi
   ```

### Kết luận

Safe Points trong MySQL là một công cụ hữu ích cho phép bạn tạo các điểm phục hồi trong quá trình thực hiện giao dịch. Điều này giúp bạn quay lại trạng thái trước đó mà không cần kết thúc toàn bộ giao dịch. Tuy nhiên, cần lưu ý rằng việc rollback về Safe Point sẽ không giải phóng các khóa đã được thiết lập trong giao dịch đó. Để giải phóng khóa, bạn cần kết thúc giao dịch bằng `COMMIT` hoặc `ROLLBACK`.

### Thực hành

Hãy thử tạo một giao dịch, thực hiện các thay đổi, tạo Safe Point, và sau đó rollback về Safe Point. Bạn cũng có thể tạo nhiều Safe Point trong một giao dịch và thử rollback về từng Safe Point để thấy sự khác biệt.
