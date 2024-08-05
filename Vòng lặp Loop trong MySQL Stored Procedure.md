# Vòng lặp Loop trong MySQL Stored Procedure

Chúng ta sẽ xem xét vòng lặp Label và Loop trong MySQL. Vòng lặp này đặc biệt hữu ích trong các tình huống bạn không biết trước số lần lặp lại của vòng lặp.

### Khai báo và sử dụng Vòng lặp Loop

Dưới đây là cách sử dụng vòng lặp Loop trong Stored Procedure:

1. **Khai báo biến đếm**:
    ```sql
    DECLARE count INT DEFAULT 0;
    ```

2. **Sử dụng từ khóa Loop**:
    ```sql
    loop_label: LOOP
        -- Thực hiện các câu lệnh trong vòng lặp
        SET count = count + 1;

        -- Điều kiện thoát vòng lặp
        IF count = 10 THEN
            LEAVE loop_label;
        END IF;
    END LOOP loop_label;
    ```

### Ví dụ Stored Procedure với Loop

Dưới đây là ví dụ về Stored Procedure sử dụng vòng lặp Loop để nối các giá trị số thành một chuỗi:

```sql
DELIMITER //

CREATE PROCEDURE loop_demo()
BEGIN
    DECLARE count INT DEFAULT 0;
    DECLARE numbers VARCHAR(30) DEFAULT '';
    
    loop_label: LOOP
        SET numbers = CONCAT(numbers, count);
        SET count = count + 1;
        
        IF count = 10 THEN
            LEAVE loop_label;
        END IF;
    END LOOP loop_label;
    
    SELECT numbers;
END //

DELIMITER ;
```

### Gọi Stored Procedure

Để gọi stored procedure này, bạn sử dụng:

```sql
CALL loop_demo();
```

### Kết Quả

Stored procedure này sẽ trả về chuỗi các số từ 0 đến 9.

### Bài Tập

1. **Thêm dấu phẩy giữa các số**:
    - Hãy thử thêm dấu phẩy giữa các số. Bạn có thể làm điều này bằng cách thay đổi câu lệnh `CONCAT` như sau:
    ```sql
    SET numbers = CONCAT(numbers, count, ', ');
    ```

2. **Loại bỏ dấu phẩy cuối cùng**:
    - Bạn sẽ nhận thấy rằng có một dấu phẩy ở cuối chuỗi. Hãy sửa đổi stored procedure để loại bỏ dấu phẩy cuối cùng.

### Sửa đổi Stored Procedure để loại bỏ dấu phẩy cuối cùng

Dưới đây là cách sửa đổi stored procedure để loại bỏ dấu phẩy cuối cùng:

```sql
DELIMITER //

CREATE PROCEDURE loop_demo()
BEGIN
    DECLARE count INT DEFAULT 0;
    DECLARE numbers VARCHAR(30) DEFAULT '';
    
    loop_label: LOOP
        SET numbers = CONCAT(numbers, count);
        SET count = count + 1;

        IF count < 10 THEN
            SET numbers = CONCAT(numbers, ', ');
        END IF;

        IF count = 10 THEN
            LEAVE loop_label;
        END IF;
    END LOOP loop_label;
    
    SELECT numbers;
END //

DELIMITER ;
```

### Kết Quả Sửa Đổi

Stored procedure sửa đổi này sẽ trả về chuỗi các số từ 0 đến 9, có dấu phẩy giữa các số nhưng không có dấu phẩy ở cuối chuỗi.

### Kết Luận

Vòng lặp Loop với Label rất hữu ích khi bạn không biết trước số lần lặp lại. Hãy thử tự mình thực hành và điều chỉnh ví dụ này để hiểu rõ hơn về cách hoạt động của vòng lặp Loop trong stored procedure.
