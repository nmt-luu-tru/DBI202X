# Vòng lặp While trong MySQL Stored Procedure

Chúng ta sẽ xem xét vòng lặp While trong MySQL. Vòng lặp có thể được sử dụng để thực hiện các tác vụ như thêm dữ liệu vào bảng hoặc duyệt qua tất cả các hàng trong một bảng.

### Ví Dụ Cơ Bản về Vòng Lặp While

1. **Khai báo biến đếm**:
    ```sql
    DECLARE count INT DEFAULT 0;
    ```

2. **Vòng lặp While**:
    ```sql
    WHILE count < 10 DO
        -- Thực hiện các câu lệnh trong vòng lặp
        SELECT 'Hello';
        SET count = count + 1;
    END WHILE;
    ```

Trong ví dụ trên, vòng lặp sẽ lặp lại 10 lần. Ban đầu biến `count` được gán giá trị 0. Mỗi lần vòng lặp chạy, biến `count` sẽ được tăng lên 1. Khi giá trị của `count` đạt đến 10, điều kiện `count < 10` không còn đúng nữa và vòng lặp sẽ dừng lại.

### Định Nghĩa Stored Procedure

Dưới đây là cách định nghĩa một stored procedure sử dụng vòng lặp While:

```sql
DELIMITER //

CREATE PROCEDURE while_demo()
BEGIN
    DECLARE count INT DEFAULT 0;
    DECLARE numbers VARCHAR(32) DEFAULT '';

    WHILE count < 10 DO
        SET numbers = CONCAT(numbers, count);
        SET count = count + 1;
    END WHILE;

    SELECT numbers;
END //

DELIMITER ;
```

1. **Khai báo biến**:
    - `count`: Biến đếm, kiểu INT, bắt đầu từ 0.
    - `numbers`: Biến chuỗi để lưu các giá trị số, kiểu VARCHAR(32), bắt đầu là chuỗi rỗng.

2. **Vòng lặp While**:
    - Khi `count` nhỏ hơn 10, thực hiện các câu lệnh bên trong vòng lặp.
    - `SET numbers = CONCAT(numbers, count)`: Nối giá trị của `count` vào biến `numbers`.
    - `SET count = count + 1`: Tăng giá trị của `count` lên 1.

### Gọi Stored Procedure

Để gọi stored procedure này, bạn sử dụng:

```sql
CALL while_demo();
```

### Kết Quả

Stored procedure này sẽ trả về chuỗi các số từ 0 đến 9.

### Kết Luận

Vòng lặp While rất hữu ích trong việc thực hiện các tác vụ lặp lại trong MySQL. Hãy thử tự mình thực hành và điều chỉnh ví dụ này để hiểu rõ hơn về cách hoạt động của vòng lặp While trong stored procedure.
