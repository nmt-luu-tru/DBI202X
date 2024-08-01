### Giới Thiệu về ACID trong Cơ Sở Dữ Liệu

Trong video này, chúng ta sẽ tìm hiểu về một thuật ngữ viết tắt quan trọng trong cơ sở dữ liệu: ACID. Thuật ngữ này bao gồm bốn tính chất quan trọng của giao dịch (transaction) trong cơ sở dữ liệu.

### ACID Là Gì?

**1. Atomicity (Tính nguyên tử):**
   - Tính nguyên tử có nghĩa là bạn có thể thực thi một loạt các câu lệnh SQL như thể chúng là một câu lệnh duy nhất, trong một bước duy nhất. Điều này đảm bảo rằng toàn bộ loạt câu lệnh được thực hiện hoàn toàn hoặc không thực hiện gì cả. 

**2. Consistency (Tính nhất quán):**
   - Tính nhất quán đảm bảo rằng cơ sở dữ liệu luôn chuyển từ trạng thái hợp lệ này sang trạng thái hợp lệ khác sau mỗi giao dịch. Ví dụ, trong một hệ thống ngân hàng, nếu bạn chuyển tiền từ một tài khoản này sang tài khoản khác, bạn muốn đảm bảo rằng cả hai bước (rút tiền và nạp tiền) đều hoàn thành hoặc không thực hiện bất kỳ bước nào. Điều này ngăn cơ sở dữ liệu rơi vào trạng thái không nhất quán.

**3. Isolation (Tính độc lập):**
   - Tính độc lập đảm bảo rằng các giao dịch khác nhau không gây ảnh hưởng lẫn nhau. Giao dịch của một người dùng không được can thiệp hoặc xung đột với giao dịch của người dùng khác. Điều này giúp duy trì tính toàn vẹn của dữ liệu trong môi trường đa người dùng.

**4. Durability (Tính bền vững):**
   - Tính bền vững đảm bảo rằng một khi giao dịch đã được cam kết (committed), nó sẽ tồn tại và không bị mất mát ngay cả khi hệ thống gặp sự cố. Dữ liệu đã cam kết phải được lưu trữ một cách an toàn và có thể khôi phục sau sự cố.

### MySQL và ACID

- **InnoDB**: Đây là engine mặc định của MySQL và hỗ trợ đầy đủ các tính chất ACID thông qua việc sử dụng giao dịch.
- **MyISAM**: Đây là một engine khác trong MySQL nhưng không hỗ trợ đầy đủ các tính chất ACID. MyISAM nhanh hơn nhưng không đảm bảo tính nhất quán và bền vững như InnoDB.

### Ví Dụ Về Sử Dụng Giao Dịch

Chúng ta sẽ bắt đầu với một ví dụ đơn giản về cách sử dụng giao dịch trong MySQL để thấy rõ tính chất độc lập (Isolation) trong hành động:

**Ví dụ: Giao dịch đơn giản**
```sql
START TRANSACTION;

-- Rút tiền từ tài khoản A
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

-- Nạp tiền vào tài khoản B
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

COMMIT;
```
Trong ví dụ này, nếu bất kỳ bước nào trong giao dịch gặp lỗi, toàn bộ giao dịch sẽ được rollback và không có thay đổi nào được thực hiện, đảm bảo tính nguyên tử và nhất quán.

### Kết Luận

Hiểu rõ các tính chất ACID là rất quan trọng để đảm bảo cơ sở dữ liệu hoạt động đúng đắn và an toàn. Trong video tiếp theo, chúng ta sẽ xem xét các ví dụ cụ thể hơn về việc sử dụng giao dịch và cách chúng ta có thể áp dụng các tính chất ACID trong thực tế. Hẹn gặp lại và chúc bạn lập trình vui vẻ!
