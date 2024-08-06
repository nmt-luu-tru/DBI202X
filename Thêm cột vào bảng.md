

Chúng ta sẽ bắt đầu tìm hiểu về việc thay đổi các lược đồ hiện có. Thường thì bạn muốn thêm một cột vào một bảng, thêm một khóa ngoại hoặc thêm một chỉ mục vào bảng. Chúng ta sẽ tìm hiểu về chỉ mục sau. Thật may mắn là bạn có thể làm tất cả những điều này mà không cần phải xóa bảng và tạo lại chúng.

### Thêm cột vào bảng
Tôi đã tạo một cơ sở dữ liệu mới và bây giờ hãy tạo một bảng trong cơ sở dữ liệu này. Chúng ta sẽ gọi bảng là `person` và nó sẽ chỉ có một cột `id`. Sau đó, chúng ta sẽ thêm các cột khác vào bảng này.

#### Tạo bảng ban đầu
```sql
CREATE DATABASE mydatabase;
USE mydatabase;

CREATE TABLE person (
    id INT PRIMARY KEY AUTO_INCREMENT
);
```

#### Xem cấu trúc bảng
```sql
DESC person;
```

#### Thêm cột vào bảng
Cú pháp để thêm cột khá trực quan nếu bạn chia nó ra. Chúng ta sử dụng từ khóa `ALTER TABLE` để thay đổi bảng.

```sql
ALTER TABLE person ADD COLUMN email VARCHAR(50) NOT NULL;
```

Chúng ta đã thêm một cột `email` vào bảng `person`. Hãy kiểm tra lại cấu trúc bảng:
```sql
DESC person;
```

### Thêm cột tại vị trí cụ thể
Bạn cũng có thể chỉ định vị trí của cột mới. Có hai khả năng: thêm cột ở vị trí đầu tiên hoặc sau một cột cụ thể nào đó.

#### Thêm cột ở vị trí đầu tiên
```sql
ALTER TABLE person ADD COLUMN name VARCHAR(50) NOT NULL FIRST;
```

#### Thêm cột sau cột cụ thể
```sql
ALTER TABLE person ADD COLUMN age INT NOT NULL AFTER id;
```

Kiểm tra lại cấu trúc bảng để xác nhận các thay đổi:
```sql
DESC person;
```

### Xóa cột
Bạn có thể xóa cột khỏi bảng bằng cú pháp sau:
```sql
ALTER TABLE person DROP COLUMN age;
```

Kiểm tra lại cấu trúc bảng để xác nhận cột đã bị xóa:
```sql
DESC person;
```

### Thực hành
Cách tốt nhất để thực hành là tạo một bảng với một cột ban đầu và sau đó thêm nhiều cột vào bảng bằng cách sử dụng `ALTER TABLE`. Sau đó, thử xóa các cột bằng cách sử dụng `ALTER TABLE`.

Trong các bài học tiếp theo, chúng ta sẽ tìm hiểu về cách thêm chỉ mục và khóa ngoại vào bảng, điều này rất hữu ích trong việc quản lý cơ sở dữ liệu.
