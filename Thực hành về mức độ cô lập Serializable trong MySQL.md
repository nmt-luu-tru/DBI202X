### Thực hành về mức độ cô lập Serializable trong MySQL

Chúng ta sẽ tìm hiểu về mức độ cô lập transaction Serializable trong MySQL và cách mà row locking (khóa cấp dòng) và table locking (khóa cấp bảng) hoạt động.

### Cài đặt môi trường

Chúng ta sẽ sử dụng một cơ sở dữ liệu thử nghiệm với bảng `person` có hai cột: `ID` (primary key) và `name`. Bảng này đã được thêm ba người. Chúng ta cũng cần kiểm tra các chỉ mục trên bảng `person`:
```sql
SHOW INDEX IN person;
```
Chỉ có một chỉ mục trên cột `ID` (primary key).

### Thiết lập mức độ cô lập Serializable

Đầu tiên, chúng ta cần thiết lập mức độ cô lập cho phiên làm việc của mình:
```sql
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```
Lặp lại bước này cho cả hai user.

### Thực hiện các giao dịch

1. **User 1:**
   ```sql
   START TRANSACTION;
   SELECT * FROM person WHERE id = 1;
   -- Khóa dòng với id = 1
   ```

2. **User 2:**
   ```sql
   START TRANSACTION;
   UPDATE person SET name = 'Linda' WHERE id = 1;
   -- Bị treo do User 1 đang giữ khóa
   ```

3. **User 1:**
   ```sql
   COMMIT;
   -- Giải phóng khóa, User 2 có thể tiếp tục
   ```

4. **User 2:**
   ```sql
   -- Sau khi User 1 commit
   -- Update sẽ tiếp tục chạy và hoàn thành
   ```

### Ví dụ về khóa cấp dòng với chỉ mục

1. **User 1:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
   START TRANSACTION;
   SELECT * FROM person WHERE id = 1;
   -- Khóa dòng với id = 1
   ```

2. **User 2:**
   ```sql
   START TRANSACTION;
   UPDATE person SET name = 'Linda' WHERE id = 2;
   -- Cập nhật dòng khác vẫn hoạt động bình thường
   ```

3. **User 1:**
   ```sql
   COMMIT;
   -- Giải phóng khóa
   ```

### Ví dụ về khóa bảng khi không có chỉ mục

1. **User 1:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
   START TRANSACTION;
   SELECT * FROM person WHERE name = 'Roger';
   -- Khóa bảng do cột name không có chỉ mục
   ```

2. **User 2:**
   ```sql
   START TRANSACTION;
   UPDATE person SET name = 'Clara' WHERE id = 3;
   -- Bị treo do User 1 khóa bảng
   ```

3. **User 1:**
   ```sql
   COMMIT;
   -- Giải phóng khóa, User 2 có thể tiếp tục
   ```

### Thêm chỉ mục và kiểm tra lại

1. **Thêm chỉ mục vào cột name:**
   ```sql
   ALTER TABLE person ADD INDEX idx_name (name);
   ```

2. **Kiểm tra lại với chỉ mục:**
   - **User 1:**
     ```sql
     SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
     START TRANSACTION;
     SELECT * FROM person WHERE name = 'Roger';
     -- Khóa dòng với name = 'Roger'
     ```

   - **User 2:**
     ```sql
     START TRANSACTION;
     UPDATE person SET name = 'Clara' WHERE id = 3;
     -- Cập nhật dòng khác vẫn hoạt động bình thường do đã có chỉ mục
     ```

### Kết luận

Mức độ cô lập `SERIALIZABLE` cung cấp mức độ bảo mật cao nhất bằng cách khóa các dòng hoặc bảng mà giao dịch đang truy vấn, ngăn chặn các giao dịch khác thực hiện thay đổi trên cùng các dòng/bảng đó cho đến khi giao dịch hiện tại hoàn tất. Tuy nhiên, điều này có thể gây ra giảm hiệu suất nếu không được sử dụng hợp lý. Ví dụ, khi nhiều giao dịch cùng cố gắng truy cập hoặc cập nhật dữ liệu, chúng có thể bị treo và chờ đợi nhau, dẫn đến "độ trễ chờ đợi" và làm giảm hiệu suất của hệ thống. Các chỉ mục giúp MySQL thực hiện khóa cấp dòng thay vì khóa bảng, cải thiện hiệu suất và tính linh hoạt của giao dịch. Khi các cột có chỉ mục, MySQL có thể khóa chính xác các dòng cần thiết thay vì khóa toàn bộ bảng, giúp giảm thiểu xung đột và tăng hiệu suất khi thực hiện các giao dịch đồng thời.

Hy vọng rằng bạn đã hiểu rõ hơn về cách sử dụng `SERIALIZABLE` và sự khác biệt giữa `row locking` và `table locking`.
