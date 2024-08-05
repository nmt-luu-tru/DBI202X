# Trigger và transaction

Chúng ta sẽ xem xét một ví dụ khác về trigger và nói một chút về trigger và transaction trong MySQL.
### Thiết lập và Mục tiêu

Đầu tiên, tôi đã tạo hai bảng:

1. **Bảng Sales (Bán hàng)**: Bảng này có khóa chính (`id`), tên sản phẩm (`product_name`) và giá trị bán (`sold_value`). Nó ghi lại các giao dịch bán hàng.

   ```sql
   CREATE TABLE sales (
       id INT AUTO_INCREMENT PRIMARY KEY,
       product_name VARCHAR(50),
       sold_value DECIMAL(10, 2)
   );
   ```

2. **Bảng Sales Totals (Tổng doanh thu)**: Bảng này có khóa chính (`id`), tổng số tiền (`total`) và ngày (`date`). Mục tiêu ở đây là giữ tổng doanh thu hàng ngày.

   ```sql
   CREATE TABLE sales_totals (
       id INT AUTO_INCREMENT PRIMARY KEY,
       total DECIMAL(10, 2),
       date DATE
   );
   ```

### Mục tiêu

Chúng ta muốn bất kỳ `INSERT` nào trên bảng `sales` tự động cập nhật bảng `sales_totals`. Ví dụ, nếu chúng ta bán một sản phẩm với giá $10, nó sẽ thêm $10 vào dòng tương ứng với ngày hôm nay trong bảng `sales_totals`. Bằng cách này, bảng `sales_totals` luôn được cập nhật với tổng doanh thu hàng ngày.

### Thực hiện Trigger

Chúng ta có thể đạt được điều này bằng cách sử dụng trigger. Hãy bắt đầu bằng việc tạo trigger cơ bản để xử lý logic này:

1. Thiết lập delimiter để xử lý việc tạo trigger:

   ```sql
   DELIMITER $$
   ```

2. Tạo trigger:

   ```sql
   CREATE TRIGGER before_sales_insert
   BEFORE INSERT ON sales
   FOR EACH ROW
   BEGIN
       DECLARE today DATE;
       DECLARE count INT DEFAULT 0;
       
       SET today = DATE(NOW());
       
       SELECT COUNT(*) INTO count 
       FROM sales_totals 
       WHERE date = today 
       FOR UPDATE;
       
       IF count = 0 THEN
           INSERT INTO sales_totals (total, date) 
           VALUES (NEW.sold_value, today);
       ELSE
           UPDATE sales_totals 
           SET total = total + NEW.sold_value 
           WHERE date = today;
       END IF;
   END $$
   ```

3. Đặt lại delimiter về mặc định:

   ```sql
   DELIMITER ;
   ```

### Giải thích

- **DELIMITER**: Chúng ta đặt một delimiter khác để cho phép tạo trigger đa dòng.
- **CREATE TRIGGER**: Chúng ta tạo trigger có tên `before_sales_insert` kích hoạt trước khi `INSERT` trên bảng `sales`.
- **FOR EACH ROW**: Điều này đảm bảo trigger chạy cho mỗi dòng được chèn.
- **DECLARE today**: Chúng ta khai báo biến `today` để lưu trữ ngày hiện tại.
- **DECLARE count**: Chúng ta khai báo biến `count` để lưu trữ số lượng dòng trong bảng `sales_totals` cho ngày hôm nay.
- **SET today = DATE(NOW())**: Chúng ta đặt biến `today` bằng ngày hiện tại.
- **SELECT COUNT(*) INTO count**: Chúng ta kiểm tra xem đã có dòng trong bảng `sales_totals` cho ngày hôm nay chưa và khóa dòng đó.
- **IF count = 0**: Nếu không có dòng nào cho ngày hôm nay, chúng ta chèn một dòng mới.
- **ELSE**: Nếu đã có dòng cho ngày hôm nay, chúng ta cập nhật tổng số bằng cách thêm giá trị mới vào.

### Trigger và Transaction

Một vấn đề quan trọng cần lưu ý khi làm việc với trigger là chúng thường chạy trong ngữ cảnh của một transaction. Điều này có nghĩa là nếu một `INSERT` xảy ra trong một transaction, trigger tương ứng cũng sẽ chạy trong cùng một transaction đó. Điều này giúp đảm bảo tính nhất quán và toàn vẹn của dữ liệu.

Ví dụ, khi bạn thực hiện `INSERT INTO sales`, nếu transaction đó bị rollback vì bất kỳ lý do gì, mọi thay đổi được thực hiện bởi trigger cũng sẽ bị rollback. Điều này giúp bảo vệ dữ liệu của bạn khỏi các trạng thái không nhất quán.

### Vấn đề với Transaction Đồng Thời

Một vấn đề tiềm ẩn khi làm việc với các trigger và transaction là xử lý các tình huống mà nhiều transaction có thể chạy đồng thời. Trong ví dụ này, nếu hai `INSERT` vào bảng `sales` xảy ra gần như đồng thời, có thể có tình huống cả hai trigger đều cố gắng chèn một dòng mới vào `sales_totals` cùng một lúc. Điều này có thể gây ra lỗi vì vi phạm ràng buộc duy nhất (unique constraint) trên cột `date`.

Để giải quyết vấn đề này, chúng ta sử dụng câu lệnh `SELECT ... FOR UPDATE` để khóa hàng tương ứng trong bảng `sales_totals`. Điều này đảm bảo rằng chỉ có một transaction có thể thực hiện thay đổi tại một thời điểm, giúp tránh các xung đột dữ liệu tiềm ẩn.

### Kiểm tra Trigger

Hãy kiểm tra trigger của chúng ta bằng cách chèn một số giao dịch bán hàng:

1. Chèn một giao dịch bán hàng vào bảng `sales`:

   ```sql
   INSERT INTO sales (product_name, sold_value) VALUES ('Product A', 10.00);
   ```

2. Kiểm tra bảng `sales_totals`:

   ```sql
   SELECT * FROM sales_totals;
   ```

   Bạn sẽ thấy một mục với ngày hôm nay và tổng số là 10.00.

3. Chèn thêm một giao dịch bán hàng cho ngày hôm nay:

   ```sql
   INSERT INTO sales (product_name, sold_value) VALUES ('Product B', 15.00);
   ```

4. Kiểm tra bảng `sales_totals` một lần nữa:

   ```sql
   SELECT * FROM sales_totals;
   ```

   Bây giờ bạn sẽ thấy tổng số được cập nhật lên 25.00 cho ngày hôm nay.

### Kết luận

Ví dụ này minh họa cách tạo trigger tự động cập nhật tổng doanh thu trong một bảng khác. Trigger là công cụ mạnh mẽ trong SQL để tự động duy trì tính nhất quán và toàn vẹn của dữ liệu. Bằng cách sử dụng transaction và logic cẩn thận, chúng ta có thể đảm bảo rằng cơ sở dữ liệu của chúng ta luôn nhất quán ngay cả khi có nhiều insert diễn ra đồng thời.

Hãy thử sao chép ví dụ này vào thiết lập cơ sở dữ liệu của bạn và thử nghiệm với việc thay đổi nó để phù hợp với các kịch bản khác nhau.
