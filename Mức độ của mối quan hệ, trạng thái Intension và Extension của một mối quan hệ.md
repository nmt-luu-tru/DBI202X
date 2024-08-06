### Thuật Ngữ Cơ Bản trong Hệ Quản Trị Cơ Sở Dữ Liệu (DBMS)

#### 1. Degree của một Relation

**Degree** của một relation là số lượng thuộc tính trong relation đó.

- **Ví dụ**: Relation **Employee** có ba thuộc tính: **EmployeeID**, **Name**, và **Age**.
- **Degree của relation Employee**: 3 (vì có ba thuộc tính).

#### 2. State của một Relation

**State** của một relation là trạng thái hiện tại của các bản ghi trong relation đó tại một thời điểm nhất định.

- **Ví dụ**: Giả sử chúng ta có một cơ sở dữ liệu chứa thông tin của các nhân viên trong một công ty.
  - Ban đầu, có một nhân viên tên là Ram, 30 tuổi, giới tính nam.
  - **State hiện tại của relation Employee**:
    ```
    EmployeeID | Name | Age | Sex
    -----------|------|-----|-----
    1          | Ram  | 30  | M
    ```
  - Khi có một nhân viên mới tên là Sam, 35 tuổi, giới tính nam gia nhập:
    ```
    EmployeeID | Name | Age | Sex
    -----------|------|-----|-----
    1          | Ram  | 30  | M
    2          | Sam  | 35  | M
    ```
  - Nếu Sam rời công ty, state của relation sẽ thay đổi:
    ```
    EmployeeID | Name | Age | Sex
    -----------|------|-----|-----
    1          | Ram  | 30  | M
    ```

#### 3. Intention và Extension của một Relation

- **Intention**: Còn được gọi là **Schema** hoặc **Entity Type**, mô tả cấu trúc của relation bằng cách liệt kê tên bảng và các thuộc tính của nó.
  - **Ví dụ**: 
    ```
    Employee (EmployeeID, Name, Age, Sex)
    ```
  - Đây là cấu trúc hoặc **Intention** của bảng Employee.

- **Extension**: Còn được gọi là **Entity**, là tập hợp các bản ghi cụ thể tại một thời điểm nhất định trong relation.
  - **Ví dụ**:
    ```
    EmployeeID | Name | Age | Sex
    -----------|------|-----|-----
    1          | Ram  | 30  | M
    2          | Sam  | 35  | M
    ```

#### 4. Tổng Kết

- **Intention (Schema)**: Định nghĩa cấu trúc của bảng, bao gồm tên bảng và các thuộc tính.
- **Extension (Entity)**: Các bản ghi cụ thể trong bảng tại một thời điểm.

### Ví Dụ Minh Họa

Giả sử chúng ta có bảng **Employee** với các thuộc tính **EmployeeID**, **Name**, **Age**, và **Sex**.

**Intention** của bảng Employee:
```
Employee (EmployeeID, Name, Age, Sex)
```

**Extension** của bảng Employee:
```
EmployeeID | Name | Age | Sex
-----------|------|-----|-----
1          | Ram  | 30  | M
2          | Sam  | 35  | M
```

Mỗi khi một bản ghi mới được thêm vào hoặc xóa khỏi bảng, **state** của relation thay đổi. **Intention** của bảng vẫn giữ nguyên, nhưng **extension** thay đổi theo các bản ghi mới.

### Kết Luận

Hiểu rõ các khái niệm **degree**, **state**, **intention**, và **extension** của một relation sẽ giúp bạn quản lý và thiết kế cơ sở dữ liệu một cách hiệu quả. Những thuật ngữ này là nền tảng của các khái niệm phức tạp hơn trong hệ quản trị cơ sở dữ liệu.
