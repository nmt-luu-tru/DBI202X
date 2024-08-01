### Giới thiệu về các mức độ cô lập trong MySQL

Chúng ta sẽ xem xét các mức độ cô lập transaction khác trong MySQL và cách chúng hoạt động. Mặc dù có thể bạn sẽ không sử dụng tất cả các mức độ này thường xuyên, nhưng việc hiểu chúng sẽ giúp bạn có cái nhìn sâu sắc hơn về cách các giao dịch (transactions) hoạt động.

### Thiết lập mức độ cô lập

Đầu tiên, chúng ta cần thiết lập mức độ cô lập cho phiên làm việc của mình. Chúng ta sẽ bắt đầu với mức độ `REPEATABLE READ` (đọc lặp lại).

### Mức độ cô lập REPEATABLE READ

1. **User 1:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
   START TRANSACTION;
   SELECT * FROM person WHERE id = 1;
   -- Khóa dòng với id = 1
   -- Kết quả vẫn là: The Mountain
   ```

2. **User 2:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
   START TRANSACTION;
   INSERT INTO person (name) VALUES ('Ravi');
   COMMIT;
   ```

3. **User 1:**
   ```sql
   SELECT * FROM person WHERE id = 1;
   -- Kết quả vẫn là: The Mountain (không thay đổi)
   COMMIT;
   -- Thực hiện lại lệnh SELECT sau khi COMMIT
   SELECT * FROM person WHERE id = 1;
   -- Kết quả bây giờ sẽ là: Ravi (thay đổi từ commit của user 2)
   ```

### Mức độ cô lập READ COMMITTED

1. **User 1:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
   START TRANSACTION;
   SELECT * FROM person WHERE id = 1;
   -- Kết quả: The Mountain
   ```

2. **User 2:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
   START TRANSACTION;
   INSERT INTO person (name) VALUES ('Simon');
   COMMIT;
   ```

3. **User 1:**
   ```sql
   SELECT * FROM person WHERE id = 1;
   -- Kết quả là: Simon (thay đổi đã commit)
   COMMIT;
   ```

### Mức độ cô lập READ UNCOMMITTED

1. **User 1:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
   START TRANSACTION;
   SELECT * FROM person WHERE id = 1;
   -- Kết quả: The Mountain
   ```

2. **User 2:**
   ```sql
   SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
   START TRANSACTION;
   INSERT INTO person (name) VALUES ('River');
   -- Thay đổi chưa commit
   ```

3. **User 1:**
   ```sql
   SELECT * FROM person WHERE id = 1;
   -- Kết quả là: River (thay đổi chưa commit)
   ROLLBACK;
   -- Thay đổi không còn
   ```

### Kết luận

Các mức độ cô lập khác nhau trong MySQL cung cấp các cấp độ bảo vệ và hiệu suất khác nhau cho các giao dịch của bạn. Mức độ cô lập `REPEATABLE READ` là mức độ mặc định và được sử dụng phổ biến nhất vì nó cân bằng giữa hiệu suất và tính toàn vẹn dữ liệu. Tuy nhiên, tùy vào yêu cầu cụ thể, bạn có thể chọn các mức độ cô lập khác như `READ COMMITTED` hoặc `READ UNCOMMITTED`.

1. **REPEATABLE READ:** Đảm bảo rằng các kết quả đọc lặp lại trong cùng một transaction sẽ nhất quán.
2. **READ COMMITTED:** Cho phép thấy các thay đổi đã được commit bởi các giao dịch khác ngay lập tức.
3. **READ UNCOMMITTED:** Cho phép thấy các thay đổi chưa được commit bởi các giao dịch khác, điều này có thể dẫn đến **dirty read**.

Mặc dù các mức độ cô lập thấp hơn có thể cải thiện hiệu suất, nhưng chúng cũng có thể dẫn đến các vấn đề về tính toàn vẹn dữ liệu. Do đó, việc chọn mức độ cô lập phù hợp phụ thuộc vào tình huống cụ thể của bạn.

Hãy thử thiết lập các mức độ cô lập khác nhau và quan sát các hiệu ứng của chúng để hiểu rõ hơn về cách chúng hoạt động. 
