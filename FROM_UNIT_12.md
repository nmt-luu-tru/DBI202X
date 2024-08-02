# Bài 12 - Transaction
## 1. Giới thiệu về TRANSACTION và bốn tính chất ACID
### 1.1 Giới Thiệu về ACID trong Cơ Sở Dữ Liệu

Trong video này, chúng ta sẽ tìm hiểu về một thuật ngữ viết tắt quan trọng trong cơ sở dữ liệu: ACID. Thuật ngữ này bao gồm bốn tính chất quan trọng của giao dịch (transaction) trong cơ sở dữ liệu.

#### ACID Là Gì?

**1. Atomicity (Tính nguyên tử):**
   - Tính nguyên tử có nghĩa là bạn có thể thực thi một loạt các câu lệnh SQL như thể chúng là một câu lệnh duy nhất, trong một bước duy nhất. Điều này đảm bảo rằng toàn bộ loạt câu lệnh được thực hiện hoàn toàn hoặc không thực hiện gì cả. 

**2. Consistency (Tính nhất quán):**
   - Tính nhất quán đảm bảo rằng cơ sở dữ liệu luôn chuyển từ trạng thái hợp lệ này sang trạng thái hợp lệ khác sau mỗi giao dịch. Ví dụ, trong một hệ thống ngân hàng, nếu bạn chuyển tiền từ một tài khoản này sang tài khoản khác, bạn muốn đảm bảo rằng cả hai bước (rút tiền và nạp tiền) đều hoàn thành hoặc không thực hiện bất kỳ bước nào. Điều này ngăn cơ sở dữ liệu rơi vào trạng thái không nhất quán.

**3. Isolation (Tính độc lập):**
   - Tính độc lập đảm bảo rằng các giao dịch khác nhau không gây ảnh hưởng lẫn nhau. Giao dịch của một người dùng không được can thiệp hoặc xung đột với giao dịch của người dùng khác. Điều này giúp duy trì tính toàn vẹn của dữ liệu trong môi trường đa người dùng.

**4. Durability (Tính bền vững):**
   - Tính bền vững đảm bảo rằng một khi giao dịch đã được cam kết (committed), nó sẽ tồn tại và không bị mất mát ngay cả khi hệ thống gặp sự cố. Dữ liệu đã cam kết phải được lưu trữ một cách an toàn và có thể khôi phục sau sự cố.

#### MySQL và ACID

- **InnoDB**: Đây là engine mặc định của MySQL và hỗ trợ đầy đủ các tính chất ACID thông qua việc sử dụng giao dịch.
- **MyISAM**: Đây là một engine khác trong MySQL nhưng không hỗ trợ đầy đủ các tính chất ACID. MyISAM nhanh hơn nhưng không đảm bảo tính nhất quán và bền vững như InnoDB.

#### Ví Dụ Về Sử Dụng Giao Dịch

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

#### Kết Luận

Hiểu rõ các tính chất ACID là rất quan trọng để đảm bảo cơ sở dữ liệu hoạt động đúng đắn và an toàn. 
Trong video tiếp theo, chúng ta sẽ xem xét các ví dụ cụ thể hơn về việc sử dụng giao dịch và cách chúng ta có thể áp dụng các tính chất ACID trong thực tế. Hẹn gặp lại và chúc bạn lập trình vui vẻ!


### 1.2 Giải thích nội dung về ACID

#### 1. Atomicity (Tính nguyên tử)
**Định nghĩa:** Tính nguyên tử trong cơ sở dữ liệu có nghĩa là một loạt các câu lệnh SQL sẽ được thực thi như thể chúng chỉ là một câu lệnh duy nhất, trong một bước nguyên tử duy nhất.

**Ví dụ:** Giả sử bạn có hai câu lệnh SQL để chuyển tiền từ tài khoản A sang tài khoản B:
- Câu lệnh 1: Trừ tiền từ tài khoản A.
- Câu lệnh 2: Cộng tiền vào tài khoản B.

Nếu bất kỳ câu lệnh nào trong hai câu lệnh này thất bại, thì toàn bộ giao dịch sẽ bị hoàn nguyên, nghĩa là không có thay đổi nào được thực hiện trong cơ sở dữ liệu. Điều này đảm bảo rằng hoặc tất cả các thay đổi được thực hiện, hoặc không có thay đổi nào cả.

#### 2. Consistency (Tính nhất quán)
**Định nghĩa:** Tính nhất quán đảm bảo rằng cơ sở dữ liệu luôn ở trong trạng thái hợp lệ, ngay cả sau khi thực hiện các giao dịch. Điều này có nghĩa là bất kỳ giao dịch nào cũng phải đưa cơ sở dữ liệu từ một trạng thái hợp lệ sang một trạng thái hợp lệ khác.

**Ví dụ:** Trong cơ sở dữ liệu ngân hàng, nếu bạn chuyển tiền từ tài khoản này sang tài khoản khác, tính nhất quán đảm bảo rằng tổng số tiền trong ngân hàng trước và sau khi thực hiện giao dịch là như nhau.

#### 3. Isolation (Tính độc lập)
**Định nghĩa:** Tính độc lập đảm bảo rằng các giao dịch khác nhau thực hiện trên cơ sở dữ liệu sẽ không gây ảnh hưởng lẫn nhau. Các thay đổi thực hiện bởi một giao dịch sẽ không được nhìn thấy bởi các giao dịch khác cho đến khi giao dịch đó hoàn thành.

**Ví dụ:** Nếu hai người dùng cùng thực hiện giao dịch chuyển tiền trên cùng một tài khoản, tính độc lập đảm bảo rằng các giao dịch này sẽ không gây ra các xung đột dữ liệu và sẽ không làm ảnh hưởng đến kết quả cuối cùng.

#### 4. Durability (Tính bền vững)
**Định nghĩa:** Tính bền vững đảm bảo rằng khi một giao dịch đã được cam kết, các thay đổi của giao dịch đó sẽ được lưu trữ vĩnh viễn trong cơ sở dữ liệu ngay cả khi hệ thống gặp sự cố.

**Ví dụ:** Sau khi hoàn tất giao dịch chuyển tiền, các thay đổi trong cơ sở dữ liệu sẽ được lưu trữ vĩnh viễn. Nếu hệ thống bị hỏng ngay sau khi giao dịch hoàn tất, các thay đổi này sẽ không bị mất.

#### Ví dụ tổng quan về ACID trong thực tế
Giả sử chúng ta có một ứng dụng ngân hàng sử dụng cơ sở dữ liệu để quản lý tài khoản người dùng. Khi người dùng thực hiện giao dịch chuyển tiền từ tài khoản A sang tài khoản B, các thuộc tính ACID sẽ đảm bảo rằng:

1. **Atomicity:** Nếu việc trừ tiền từ tài khoản A hoặc cộng tiền vào tài khoản B thất bại, toàn bộ giao dịch sẽ bị hủy bỏ và tài khoản của người dùng sẽ không bị ảnh hưởng.
2. **Consistency:** Sau khi giao dịch hoàn tất, tổng số tiền trong tài khoản ngân hàng sẽ không thay đổi và hệ thống vẫn giữ được tính nhất quán.
3. **Isolation:** Các giao dịch khác thực hiện cùng lúc trên các tài khoản khác sẽ không gây ảnh hưởng đến giao dịch này.
4. **Durability:** Sau khi giao dịch hoàn tất và được cam kết, các thay đổi sẽ được lưu trữ vĩnh viễn trong cơ sở dữ liệu, đảm bảo rằng dữ liệu không bị mất ngay cả khi hệ thống gặp sự cố.

#### Lời kết
Những nguyên tắc ACID này rất quan trọng để đảm bảo tính toàn vẹn và độ tin cậy của cơ sở dữ liệu trong các ứng dụng yêu cầu độ chính xác cao như ngân hàng, tài chính, và nhiều lĩnh vực khác.
