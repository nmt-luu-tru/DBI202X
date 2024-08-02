
Chúng ta sẽ tìm hiểu về việc tạo một stored procedure đơn giản trong MySQL. Stored procedures là các tập hợp các câu lệnh SQL mà bạn có thể chạy chỉ bằng cách nhập một lệnh duy nhất. Chúng có nhiều mục đích và ứng dụng khác nhau.

### Tạo Stored Procedure "Hello World"

Stored procedure đơn giản nhất có thể là một stored procedure chỉ chạy một câu lệnh SQL duy nhất. Dưới đây là các bước để tạo một stored procedure "Hello World".

1. **Đặt Delimiter**:
   Để bắt đầu, chúng ta cần thay đổi delimiter (ký tự kết thúc câu lệnh) để MySQL hiểu nơi kết thúc của toàn bộ stored procedure. Delimiter mặc định là dấu chấm phẩy (`;`), nhưng trong stored procedure, chúng ta cần sử dụng dấu chấm phẩy để kết thúc từng câu lệnh SQL. Vì vậy, chúng ta sẽ thay đổi delimiter tạm thời.

   ```sql
   DELIMITER $$
   ```

2. **Tạo Stored Procedure**:
   Sử dụng cú pháp `CREATE PROCEDURE`, chúng ta sẽ tạo một stored procedure có tên là `hello_world`.

   ```sql
   CREATE PROCEDURE hello_world()
   BEGIN
       SELECT 'Hello, World!';
   END$$
   ```

3. **Đặt lại Delimiter**:
   Sau khi tạo stored procedure, chúng ta đặt lại delimiter về dấu chấm phẩy (`;`).

   ```sql
   DELIMITER ;
   ```

### Gọi Stored Procedure

Để chạy stored procedure mà chúng ta vừa tạo, chúng ta sử dụng lệnh `CALL`.

```sql
CALL hello_world();
```

Kết quả sẽ là một bảng kết quả chứa dòng "Hello, World!".

### Xóa Stored Procedure

Nếu bạn muốn xóa stored procedure, bạn có thể sử dụng lệnh `DROP PROCEDURE`.

```sql
DROP PROCEDURE hello_world;
```

### Ví dụ Thực hành

1. **Thay đổi Delimiter**:
   ```sql
   DELIMITER $$
   ```

2. **Tạo Stored Procedure**:
   ```sql
   CREATE PROCEDURE hello_world()
   BEGIN
       SELECT 'Hello, World!';
   END$$
   ```

3. **Đặt lại Delimiter**:
   ```sql
   DELIMITER ;
   ```

4. **Gọi Stored Procedure**:
   ```sql
   CALL hello_world();
   ```

5. **Xóa Stored Procedure**:
   ```sql
   DROP PROCEDURE hello_world;
   ```

### Kết luận

Stored procedures là một cách hiệu quả để tập hợp và tái sử dụng các câu lệnh SQL phức tạp. Trong ví dụ này, chúng ta đã tạo một stored procedure đơn giản để hiển thị "Hello, World!". Bạn có thể mở rộng kiến thức của mình bằng cách thêm nhiều câu lệnh SQL phức tạp hơn vào stored procedure và sử dụng các tham số để làm cho chúng linh hoạt hơn.

Hãy thử tự tạo một stored procedure của riêng bạn và thực hành các bước trên để hiểu rõ hơn về cách làm việc với stored procedures trong MySQL.
