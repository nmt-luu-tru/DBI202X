
### 1. Giới thiệu về transaction trong MySQL
Trong MySQL, một transaction là một tập hợp các câu lệnh SQL (Structured Query Language) được thực thi như một đơn vị công việc. Điều này có nghĩa là tất cả các câu lệnh trong transaction phải thành công; nếu có bất kỳ câu lệnh nào thất bại, toàn bộ transaction có thể được rollback.

### 2. Sử dụng engine InnoDB và sự khác biệt với MyISAM
InnoDB là engine mặc định trong MySQL từ năm 2016 và hỗ trợ transaction, trong khi MyISAM thì không. Transaction cho phép các thao tác trên dữ liệu có thể được rollback nếu có lỗi, đảm bảo tính toàn vẹn của dữ liệu.

### 3. Cách tắt chế độ auto commit
Mặc định, MySQL sẽ tự động commit sau mỗi câu lệnh DML (Data Manipulation Language - Ngôn ngữ thao tác dữ liệu) như INSERT, UPDATE, DELETE. Để tắt chế độ này, bạn có thể sử dụng câu lệnh:
```sql
SET AUTOCOMMIT = 0;
```
Điều này có nghĩa là bất kỳ thay đổi nào bạn thực hiện trên bảng sẽ không được lưu lại cho đến khi bạn thực hiện commit thủ công.

### 4. Ví dụ về thao tác với transaction: INSERT, DELETE, UPDATE
Giả sử chúng ta có một bảng tên là `books` với các cột `id`, `title`.

#### Bắt đầu transaction và thêm dữ liệu:
```sql
START TRANSACTION;

INSERT INTO books (id, title) VALUES (1, 'The Journey');
INSERT INTO books (id, title) VALUES (2, 'The Mountain');
```
Kiểm tra dữ liệu đã thêm:
```sql
SELECT * FROM books;
-- Kết quả: [1, 'The Journey'], [2, 'The Mountain']
```
Lưu ý rằng dữ liệu này sẽ không thực sự được thêm vào bảng cho đến khi bạn thực hiện commit.

#### Commit transaction:
```sql
COMMIT;
```
Bây giờ, các thay đổi sẽ được lưu lại vĩnh viễn trong bảng `books`.

#### Thao tác DELETE và UPDATE:
```sql
START TRANSACTION;

DELETE FROM books WHERE id = 1;
UPDATE books SET title = 'The Universe' WHERE id = 2;

SELECT * FROM books;
-- Kết quả: [2, 'The Universe']
```
#### Rollback transaction:
```sql
ROLLBACK;
```
Các thay đổi sẽ bị hoàn tác và bảng sẽ trở lại trạng thái trước khi transaction bắt đầu:
```sql
SELECT * FROM books;
-- Kết quả: [1, 'The Journey'], [2, 'The Mountain']
```

### 5. Khái niệm commit và rollback
- **Commit:** Khi bạn thực hiện commit, tất cả các thay đổi kể từ lần commit hoặc rollback cuối cùng sẽ được lưu lại vĩnh viễn trong bảng.
- **Rollback:** Khi bạn thực hiện rollback, tất cả các thay đổi kể từ lần commit hoặc rollback cuối cùng sẽ bị hoàn tác.

### Ví dụ cụ thể:
1. **Thêm dữ liệu:**
   ```sql
   SET AUTOCOMMIT = 0;
   START TRANSACTION;
   INSERT INTO books (id, title) VALUES (3, 'The Ocean');
   SELECT * FROM books;
   -- Kết quả: [1, 'The Journey'], [2, 'The Mountain'], [3, 'The Ocean']
   COMMIT;
   ```

2. **Xóa và cập nhật dữ liệu:**
   ```sql
   START TRANSACTION;
   DELETE FROM books WHERE id = 2;
   UPDATE books SET title = 'The Sea' WHERE id = 3;
   SELECT * FROM books;
   -- Kết quả: [1, 'The Journey'], [3, 'The Sea']
   ROLLBACK;
   SELECT * FROM books;
   -- Kết quả: [1, 'The Journey'], [2, 'The Mountain'], [3, 'The Ocean']
   ```

Như vậy, qua các ví dụ cụ thể này, bạn có thể thấy cách mà transaction giúp quản lý các thay đổi dữ liệu một cách an toàn và hiệu quả trong MySQL.
