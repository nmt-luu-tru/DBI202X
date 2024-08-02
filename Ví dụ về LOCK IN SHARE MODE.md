### Giới thiệu
Chúng ta sẽ xem xét việc khóa hàng bằng cú pháp `LOCK IN SHARE MODE`.

### Khóa Hàng với LOCK IN SHARE MODE
Trong video trước, chúng ta đã thấy một ví dụ về việc khóa các hàng theo **write lock** để có được khóa độc quyền trên các hàng đó. Đôi khi, việc có **read lock** trên một số hàng để có khóa chia sẻ cũng có thể hữu ích, ngăn chặn người khác cập nhật các hàng mà bạn đang đọc cho đến khi bạn hoàn thành giao dịch.

### Ví dụ về Thư Viện và Sách
Hãy tạo một bảng `libraries` với các trường như `id` và `name`. Mỗi thư viện có một tên và một ID là khóa chính. Chúng ta cũng có bảng `books` với các trường như `title`, `id` (khóa chính) và `library_id`. Một thư viện có thể chứa nhiều sách, do đó, chúng ta có một mối quan hệ nhiều - một giữa `books` và `libraries`.

```sql
CREATE TABLE libraries (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255)
);

CREATE TABLE books (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    library_id INT,
    FOREIGN KEY (library_id) REFERENCES libraries(id)
);
```

### Ví dụ với Khóa Đọc
Hãy tưởng tượng rằng chúng ta muốn thêm một cuốn sách vào một thư viện nhất định. Đầu tiên, chúng ta cần lấy `id` của thư viện đó và sau đó thêm sách vào thư viện.

```sql
START TRANSACTION;

SELECT id FROM libraries WHERE name = 'Nottingham' LOCK IN SHARE MODE;
-- Kết quả là 2

INSERT INTO books (title, library_id) VALUES ('Painting for Beginners', 2);

COMMIT;
```

### Giải Thích
- `LOCK IN SHARE MODE` đảm bảo rằng các hàng có id = 2 sẽ bị khóa ở chế độ đọc, ngăn chặn người khác cập nhật, xóa hàng này cho đến khi giao dịch hoàn thành.
- Điều này ngăn chặn các vấn đề như xóa hoặc cập nhật hàng bởi các phiên khác khi chúng ta đang thực hiện giao dịch của mình.

### Vấn Đề và Giải Pháp
Giả sử ai đó xóa thư viện Nottingham trong khi chúng ta đang thực hiện giao dịch trên. Điều này có thể dẫn đến việc chúng ta cố gắng chèn một cuốn sách với một `library_id` không tồn tại. Sử dụng `LOCK IN SHARE MODE` sẽ ngăn chặn điều này bằng cách khóa hàng được đọc cho đến khi giao dịch hoàn tất.

### Thử Nghiệm
Hãy xem xét ví dụ sau:

```sql
START TRANSACTION;

SELECT id FROM libraries WHERE name = 'Nottingham' LOCK IN SHARE MODE;

-- Trong một phiên khác
DELETE FROM libraries WHERE id = 2;
-- Điều này sẽ bị chặn cho đến khi giao dịch đầu tiên hoàn thành

-- Quay lại phiên đầu tiên
INSERT INTO books (title, library_id) VALUES ('Painting for Beginners', 2);

COMMIT;
```

### Kết Luận
Sử dụng `LOCK IN SHARE MODE` đảm bảo rằng các hàng được bảo vệ khỏi sự can thiệp của các phiên khác trong suốt quá trình thực hiện giao dịch. Điều này rất quan trọng trong việc duy trì tính toàn vẹn và nhất quán của dữ liệu, đặc biệt là trong các giao dịch liên quan đến nhiều bảng.

Hãy thử tự triển khai điều này để hiểu rõ hơn về cách hoạt động của `LOCK IN SHARE MODE`.
