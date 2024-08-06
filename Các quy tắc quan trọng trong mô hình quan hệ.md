### Các Thuộc Tính Quan Trọng của Mô Hình Quan Hệ

#### 1. Không Có Giá Trị Trùng Lặp

**Giá trị trùng lặp** trong một relation không được phép tồn tại. Điều này là do một relation trong cơ sở dữ liệu được coi là một **tập hợp** (set) các bản ghi (record).

- **Tập hợp**: Trong lý thuyết tập hợp, các phần tử trong một tập hợp không được phép trùng lặp. Ví dụ, nếu chúng ta có một tập hợp A = {1, 2}, thì chúng ta không thể có hai phần tử "1" trong tập hợp.
- **Relation là một tập hợp**: Mỗi relation là một tập hợp của các bản ghi. Ví dụ:
  ```
  EmployeeID | FirstName | LastName | PhoneNumber
  -----------|-----------|----------|------------
  1000       | Ram       | Singh    | 94435
  ```
  Không thể có bản ghi trùng lặp:
  ```
  1000       | Ram       | Singh    | 94435
  ```
  Điều này là rất quan trọng trong mô hình quan hệ, không được phép có hai bản ghi giống nhau hoàn toàn.

#### 2. Giá Trị Null

**Giá trị Null** được sử dụng để biểu thị hai tình huống: không tồn tại hoặc không áp dụng.

- **Không tồn tại**: Khi một thuộc tính của một bản ghi không có giá trị, giá trị Null có thể được sử dụng. Ví dụ, nếu một nhân viên không có số điện thoại, chúng ta có thể lưu giá trị Null cho thuộc tính PhoneNumber.
  ```
  EmployeeID | FirstName | MiddleName | LastName | PhoneNumber
  -----------|-----------|------------|----------|------------
  2000       | A         | B          | C        | NULL
  ```

- **Không áp dụng**: Khi một thuộc tính không áp dụng cho một bản ghi, giá trị Null cũng có thể được sử dụng. Ví dụ, nếu một nhân viên không có tên đệm, chúng ta có thể lưu giá trị Null cho thuộc tính MiddleName.
  ```
  EmployeeID | FirstName | MiddleName | LastName | PhoneNumber
  -----------|-----------|------------|----------|------------
  1050       | X         | NULL       | Y        | 95567
  ```

#### Phân Biệt Giữa "Không Tồn Tại" và "Không Áp Dụng"

- **Không tồn tại**: Giá trị Null được sử dụng vì thuộc tính không có giá trị. Ví dụ, một nhân viên không có số điện thoại hiện tại, nhưng có thể thêm vào sau.
  ```
  EmployeeID | FirstName | LastName | PhoneNumber
  -----------|-----------|----------|------------
  2000       | A         | B        | NULL
  ```

- **Không áp dụng**: Giá trị Null được sử dụng vì thuộc tính không áp dụng cho bản ghi đó. Ví dụ, một nhân viên không có tên đệm và sẽ không bao giờ có.
  ```
  EmployeeID | FirstName | MiddleName | LastName
  -----------|-----------|------------|----------
  1050       | X         | NULL       | Y
  ```

### Tổng Kết

- **Không có giá trị trùng lặp**: Mỗi bản ghi trong một relation phải là duy nhất. Điều này đảm bảo tính nhất quán và tính duy nhất của dữ liệu.
- **Giá trị Null**: Được sử dụng để biểu thị rằng giá trị không tồn tại hoặc không áp dụng, mặc dù nó không nằm trong miền của thuộc tính.

Việc hiểu và áp dụng đúng các nguyên tắc này là rất quan trọng trong việc thiết kế và quản lý cơ sở dữ liệu quan hệ, đảm bảo tính toàn vẹn và hiệu quả của dữ liệu.
