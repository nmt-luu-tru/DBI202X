# Tìm hiểu về Triggers trong MySQL

Chúng ta sẽ tìm hiểu về Triggers trong MySQL. Triggers rất giống với stored procedures, nhưng chúng được thực thi tự động khi có một thay đổi nào đó xảy ra trên dữ liệu của bạn.  

### Cú pháp cơ bản  

```sql
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
    -- Các câu lệnh SQL thực thi khi trigger được kích hoạt
    -- Có thể sử dụng các từ khóa OLD và NEW để truy cập các giá trị trước và sau khi thay đổi
END;
$$
```

### Chú thích chi tiết:

1. **`CREATE TRIGGER trigger_name`**: 
   - `trigger_name` là tên của trigger bạn muốn tạo.

2. **`{BEFORE | AFTER}`**:
   - `BEFORE` chỉ định rằng trigger sẽ được thực thi trước khi hành động (INSERT, UPDATE, DELETE) diễn ra.
   - `AFTER` chỉ định rằng trigger sẽ được thực thi sau khi hành động (INSERT, UPDATE, DELETE) đã hoàn tất.

3. **`{INSERT | UPDATE | DELETE}`**:
   - `INSERT` chỉ định rằng trigger sẽ kích hoạt khi một bản ghi mới được chèn vào bảng.
   - `UPDATE` chỉ định rằng trigger sẽ kích hoạt khi một bản ghi trong bảng được cập nhật.
   - `DELETE` chỉ định rằng trigger sẽ kích hoạt khi một bản ghi trong bảng bị xóa.

4. **`ON table_name`**:
   - `table_name` là tên của bảng mà trigger sẽ được gắn vào.

5. **`FOR EACH ROW`**:
   - Chỉ định rằng trigger sẽ được thực thi cho mỗi bản ghi bị ảnh hưởng bởi hành động (INSERT, UPDATE, DELETE).

6. **`BEGIN ... END;`**:
   - Khối mã giữa `BEGIN` và `END` chứa các câu lệnh SQL sẽ được thực thi khi trigger được kích hoạt.
   - Trong khối này, bạn có thể sử dụng các từ khóa `OLD` và `NEW` để truy cập các giá trị của bản ghi trước và sau khi thay đổi (đối với UPDATE), hoặc chỉ các giá trị mới (đối với INSERT) hoặc cũ (đối với DELETE).

### Ví Dụ Về Trigger

Giả sử chúng ta có một bảng `sales` chứa các thông tin về các sản phẩm đã bán được, bao gồm `id`, `product_name`, và `value`. Mỗi khi có thay đổi về giá trị bán của sản phẩm nào đó, chúng ta muốn ghi lại thông tin thay đổi này vào một bảng khác gọi là `sales_update`.

#### Tạo Bảng `sales` và `sales_update`

```sql
CREATE TABLE sales (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(50),
    value DECIMAL(10, 2)
);

CREATE TABLE sales_update (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    change_date TIMESTAMP,
    before_value DECIMAL(10, 2),
    after_value DECIMAL(10, 2)
);
```

### Chèn Dữ Liệu Vào Bảng `sales`

```sql
INSERT INTO sales (product_name, value) VALUES ('dog lead', 5.40);
INSERT INTO sales (product_name, value) VALUES ('melon', 1.20);
INSERT INTO sales (product_name, value) VALUES ('cake', 0.80);
```

### Tạo Trigger

Để tạo trigger, chúng ta cần thay đổi delimiter để phân biệt các câu lệnh:

```sql
DELIMITER $$
```

Sau đó, chúng ta tạo trigger `before_update_sales`:

```sql
CREATE TRIGGER before_update_sales
BEFORE UPDATE ON sales
FOR EACH ROW
BEGIN
    INSERT INTO sales_update (product_id, change_date, before_value, after_value)
    VALUES (OLD.id, NOW(), OLD.value, NEW.value);
END $$
```

Cuối cùng, đặt lại delimiter:

```sql
DELIMITER ;
```

### Giải Thích

- **DELIMITER**: Đặt lại delimiter để phân biệt các câu lệnh.
- **CREATE TRIGGER**: Tạo trigger `before_update_sales`.
- **BEFORE UPDATE ON sales**: Trigger sẽ được thực thi trước khi có cập nhật trên bảng `sales`.
- **FOR EACH ROW**: Trigger sẽ được thực thi cho mỗi hàng bị thay đổi.
- **BEGIN...END**: Khối lệnh được thực thi bởi trigger.
- **INSERT INTO sales_update**: Chèn thông tin về thay đổi vào bảng `sales_update` với các giá trị `OLD.id`, `NOW()`, `OLD.value`, và `NEW.value`.
- `OLD` và `NEW` là các từ khóa đặc biệt được sử dụng để tham chiếu đến các bản ghi trước và sau khi thực hiện hành động trên một bảng.

### Thử Trigger

Cập nhật giá trị của sản phẩm `dog lead`:

```sql
UPDATE sales SET value = 3.20 WHERE id = 1;
```

Kiểm tra bảng `sales_update`:

```sql
SELECT * FROM sales_update;
```

Bạn sẽ thấy một dòng ghi lại thay đổi giá trị của `dog lead` từ $5.40 xuống $3.20.

### Kết Luận

Triggers rất hữu ích để tự động ghi lại thông tin mỗi khi có thay đổi trên dữ liệu của bạn. Bạn có thể tạo các trigger để theo dõi các hành động như `INSERT`, `UPDATE`, và `DELETE` trên bảng.
