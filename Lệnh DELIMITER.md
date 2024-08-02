Lệnh `DELIMITER` trong MySQL chủ yếu được sử dụng khi làm việc với các thủ tục lưu trữ (stored procedures), trigger, hoặc function vì các thành phần này thường chứa nhiều câu lệnh SQL. Các câu lệnh này cần được phân biệt rõ ràng trong cùng một khối mã, và dấu phân cách mặc định `;` sẽ không đủ để phân biệt các câu lệnh bên trong và bên ngoài khối mã. 

Khi viết một thủ tục lưu trữ, trigger, hoặc function, bạn thường cần sử dụng nhiều câu lệnh SQL trong khối `BEGIN ... END`. Nếu không thay đổi ký tự phân cách, MySQL sẽ hiểu sai các câu lệnh bên trong khối mã, gây ra lỗi cú pháp. Do đó, `DELIMITER` được sử dụng để thay đổi ký tự phân cách, cho phép MySQL nhận diện chính xác điểm kết thúc của từng câu lệnh SQL.

Dưới đây là một ví dụ với các lệnh SQL thông thường để minh họa cách sử dụng lệnh `DELIMITER` và giải thích công dụng của nó:

1. **Thay đổi ký tự phân cách**: Giả sử bạn muốn thay đổi ký tự phân cách từ `;` sang `//` để dễ dàng phân biệt các câu lệnh.

```sql
DELIMITER //
```

2. **Chạy các lệnh SQL với ký tự phân cách mới**: Thay vì sử dụng `;`, bạn sẽ sử dụng `//` để phân cách các câu lệnh.

```sql
SELECT 'First statement'//
SELECT 'Second statement'//
SELECT 'Third statement'//
```

3. **Đặt lại ký tự phân cách mặc định**: Sau khi hoàn tất các lệnh SQL, bạn có thể đặt lại ký tự phân cách về `;`.

```sql
DELIMITER ;
```

Dưới đây là một ví dụ đầy đủ:

```sql
-- Thay đổi ký tự phân cách thành //
DELIMITER //

-- Chạy các lệnh SQL
SELECT 'First statement'//
SELECT 'Second statement'//
SELECT 'Third statement'//

-- Đặt lại ký tự phân cách mặc định
DELIMITER ;
```

Trong ví dụ này:

- `DELIMITER //` thay đổi ký tự phân cách thành `//`.
- Các câu lệnh SQL được phân cách bằng `//` thay vì `;`.
- `DELIMITER ;` đặt lại ký tự phân cách về `;`.

Lệnh `DELIMITER` cho phép bạn tạm thời sử dụng một ký tự khác để phân cách các câu lệnh SQL, điều này có thể hữu ích khi bạn cần chạy một tập hợp các câu lệnh mà không muốn mỗi câu lệnh kết thúc bằng dấu `;`. Tuy nhiên, trong các tình huống thông thường (không phải thủ tục lưu trữ, trigger, hoặc function), bạn hiếm khi cần thay đổi ký tự phân cách mặc định. 

Do đó, `DELIMITER` thường được sử dụng khi làm việc với các thủ tục lưu trữ, trigger, hoặc function để giúp MySQL nhận diện chính xác điểm kết thúc của từng câu lệnh bên trong khối `BEGIN ... END`, tránh lỗi cú pháp và đảm bảo mã SQL được thực thi chính xác.
