# Sử dụng Triggers để kiểm tra và sửa đổi giá trị trong MySQL

Chúng ta sẽ tìm hiểu cách sử dụng triggers trong MySQL để kiểm tra và sửa đổi giá trị trước khi lưu vào bảng. Triggers rất hữu ích khi bạn muốn tự động kiểm tra và sửa đổi dữ liệu trước khi thực hiện thao tác `INSERT` hoặc `UPDATE`.

### Ví dụ về Trigger để kiểm tra giá trị

Giả sử chúng ta có một bảng `products` với các cột `id`, `name`, và `value`. Chúng ta muốn đảm bảo rằng giá trị (`value`) của sản phẩm không được vượt quá 100. Nếu ai đó cố gắng chèn hoặc cập nhật giá trị lớn hơn 100, chúng ta sẽ tự động thay đổi giá trị đó thành 100.

#### Tạo bảng `products`

```sql
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    value DECIMAL(10, 2) NOT NULL
);
```

### Tạo Trigger cho thao tác INSERT

Trước khi tạo trigger, chúng ta cần thay đổi delimiter để dễ phân biệt các câu lệnh:

```sql
DELIMITER $$
```

Tạo trigger `before_insert_products` để kiểm tra giá trị trước khi chèn:

```sql
CREATE TRIGGER before_insert_products
BEFORE INSERT ON products
FOR EACH ROW
BEGIN
    IF NEW.value > 100 THEN
        SET NEW.value = 100;
    END IF;
END $$
```

### Tạo Trigger cho thao tác UPDATE

Tạo trigger `before_update_products` để kiểm tra giá trị trước khi cập nhật:

```sql
CREATE TRIGGER before_update_products
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    IF NEW.value > 100 THEN
        SET NEW.value = 100;
    END IF;
END $$
```

Sau khi tạo các trigger, đặt lại delimiter về mặc định:

```sql
DELIMITER ;
```

### Giải thích

- **DELIMITER**: Đặt lại delimiter để phân biệt các câu lệnh.
- **CREATE TRIGGER**: Tạo trigger `before_insert_products` và `before_update_products`.
- **BEFORE INSERT/UPDATE ON products**: Trigger sẽ được thực thi trước khi thực hiện thao tác `INSERT` hoặc `UPDATE` trên bảng `products`.
- **FOR EACH ROW**: Trigger sẽ được thực thi cho mỗi hàng bị thay đổi.
- **IF NEW.value > 100 THEN**: Kiểm tra xem giá trị mới (`NEW.value`) có lớn hơn 100 hay không.
- **SET NEW.value = 100**: Nếu giá trị mới lớn hơn 100, thay đổi giá trị đó thành 100.

### Kiểm tra Trigger

Chèn dữ liệu vào bảng `products`:

```sql
INSERT INTO products (name, value) VALUES ('Item1', 99.99);
INSERT INTO products (name, value) VALUES ('Item2', 150.00);
```

Kiểm tra bảng `products`:

```sql
SELECT * FROM products;
```

Bạn sẽ thấy rằng giá trị của `Item2` đã được thay đổi thành 100.

Cập nhật giá trị của sản phẩm:

```sql
UPDATE products SET value = 200.00 WHERE id = 1;
```

Kiểm tra lại bảng `products`:

```sql
SELECT * FROM products;
```

Bạn sẽ thấy rằng giá trị của `Item1` đã được thay đổi thành 100.

### Bài Tập Thực Hành

Hãy thử tạo một bảng và các trigger để kiểm tra rằng giá trị của sản phẩm phải nằm trong khoảng từ 20 đến 100. Nếu giá trị nhỏ hơn 20, nó sẽ được thay đổi thành 20; nếu lớn hơn 100, nó sẽ được thay đổi thành 100.
