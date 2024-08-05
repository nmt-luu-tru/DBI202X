# User Defined Function trong MySQL  

### Sự Khác Biệt Giữa Functions và Procedures Trong MySQL

Chúng ta sẽ tìm hiểu sự khác biệt giữa functions và procedures trong MySQL trước khi đi vào định nghĩa functions của riêng mình.

### Procedures trong MySQL

- **Procedures** là các khối mã lệnh có thể được gọi bằng cách sử dụng cú pháp `CALL procedure_name(parameters)`.
- **Procedures** không trả về giá trị trực tiếp. Chúng có thể thay đổi dữ liệu trong cơ sở dữ liệu hoặc thực hiện các tác vụ mà không cần trả về giá trị.
- Các parameters trong procedures có thể là in hoặc out.

Ví dụ:

```sql
CALL my_procedure(parameter1, parameter2);
```

### Functions trong MySQL

- **Functions** thường được gọi trong câu lệnh `SELECT`.
- **Functions** luôn trả về một giá trị duy nhất.
- Các functions có thể chấp nhận các giá trị hoặc tên cột và thực hiện các phép tính hoặc thao tác và trả về kết quả cuối cùng.
- Functions không thể cập nhật dữ liệu trong cơ sở dữ liệu, chúng chủ yếu được sử dụng để tính toán và trả về kết quả.

Ví dụ về function:

```sql
SELECT LEAST(6, 4, 9);  -- Trả về giá trị nhỏ nhất trong các giá trị đã cho, kết quả là 4.
```

### Ví dụ về Aggregate Functions

Aggregate functions như `SUM`, `AVG`, `COUNT` dùng để tính toán các giá trị tổng hợp từ các hàng trong bảng.

Ví dụ:

```sql
SELECT SUM(transaction_value), DATE(sold_at)
FROM sales
GROUP BY DATE(sold_at);
```

Câu lệnh trên sẽ tính tổng giá trị giao dịch hàng ngày từ bảng `sales`.

### Tạo Functions trong MySQL

Mặc dù không thể tạo aggregate functions tùy chỉnh như `SUM` trong MySQL, bạn có thể tạo các functions để thực hiện các phép tính hoặc thao tác cụ thể.

## Định Nghĩa Functions

### Cú pháp cơ bản
```sql
CREATE FUNCTION function_name (parameters)
RETURNS return_type
BEGIN
    -- function body
    RETURN return_value;
END;
```

Sau đây là ví dụ về cách định nghĩa một function tùy chỉnh trong MySQL:

1. **Đặt DELIMITER**: Chúng ta cần thay đổi delimiter để phân biệt các câu lệnh trong stored procedure.
    ```sql
    DELIMITER $$
    ```

2. **Tạo Function**:
    ```sql
    CREATE FUNCTION calculate_discount(price DECIMAL(10,2), discount_rate DECIMAL(5,2))
    RETURNS DECIMAL(10,2)
    BEGIN
        DECLARE final_price DECIMAL(10,2);
        SET final_price = price - (price * discount_rate / 100);
        RETURN final_price;
    END $$
    ```

3. **Đặt lại DELIMITER**:
    ```sql
    DELIMITER ;
    ```

### Gọi Function

Để sử dụng function vừa tạo:

```sql
SELECT calculate_discount(100, 10);  -- Trả về 90.00 vì 10% của 100 là 10, và giá cuối cùng là 100 - 10 = 90.
```

### Giải Thích Mã Lệnh

- **Định nghĩa Function**: Sử dụng cú pháp `CREATE FUNCTION`.
- **Parameters**: `price` và `discount_rate` là các parameters đầu vào của function.
- **Khai báo biến**: `DECLARE final_price DECIMAL(10,2);` khai báo biến để lưu giá trị cuối cùng.
- **Thiết lập giá trị**: `SET final_price = price - (price * discount_rate / 100);` tính toán giá cuối cùng sau khi áp dụng giảm giá.
- **Trả về giá trị**: `RETURN final_price;` trả về giá trị cuối cùng.

### Kết Luận

Functions và procedures trong MySQL có các ứng dụng và mục đích sử dụng khác nhau. Procedures chủ yếu để thực hiện các tác vụ và thao tác cơ sở dữ liệu mà không cần trả về giá trị, trong khi functions được sử dụng để tính toán và trả về một giá trị duy nhất. Hiểu rõ sự khác biệt này giúp bạn sử dụng đúng công cụ cho đúng mục đích trong MySQL.
