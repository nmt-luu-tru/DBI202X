### Ràng Buộc Khóa Trong Mô Hình Quan Hệ

#### 1. Ràng Buộc Khóa (Key Constraints)

Ràng buộc khóa là một trong những quy tắc quan trọng trong mô hình quan hệ, giúp đảm bảo rằng không có hai bản ghi (tuple) nào trong một bảng có thể có cùng giá trị cho tất cả các thuộc tính.

**Nguyên tắc**: Không có hai bản ghi nào trong bảng có cùng giá trị cho tất cả các thuộc tính.

Ví dụ: Giả sử ta có một bảng `Employee` với các thuộc tính như sau:
- **Employee ID**: 1000
- **Employee Name**: ABC
- **Age**: 35
- **Sex**: Male

Bản ghi này sẽ như sau:
```
Employee ID | Employee Name | Age | Sex
------------|---------------|-----|-----
1000        | ABC           | 35  | Male
```

Nếu tồn tại một bản ghi khác có cùng giá trị cho tất cả các thuộc tính, thì đây là bản ghi trùng lặp và không được phép trong mô hình quan hệ. 

**Giải thích**: Một quan hệ (relation) trong cơ sở dữ liệu thực chất là một tập hợp các bản ghi. Trong lý thuyết tập hợp, không cho phép các phần tử trùng lặp. Do đó, tương tự, trong quan hệ (set of records), không được phép có các bản ghi trùng lặp.

### 2. Các Loại Khóa (Types of Keys)

#### Super Key (Siêu Khóa)

Một siêu khóa là một tập hợp các thuộc tính có thể được sử dụng để xác định duy nhất một bản ghi trong một bảng. Mỗi bảng có thể có nhiều siêu khóa.

#### Candidate Key (Khóa Ứng Viên)

Một khóa ứng viên là một siêu khóa tối thiểu, nghĩa là nó không chứa thuộc tính thừa. Mỗi bảng có thể có nhiều khóa ứng viên.

#### Primary Key (Khóa Chính)

Khóa chính là một khóa ứng viên được chọn để xác định duy nhất các bản ghi trong bảng. Một bảng chỉ có một khóa chính.

#### Example:

Giả sử bảng `Employee` có các thuộc tính sau:
- Employee ID
- Employee Name
- Age
- Sex

**Super Key**: {Employee ID}, {Employee ID, Employee Name}, {Employee ID, Age}, {Employee ID, Sex}, ...

**Candidate Key**: {Employee ID}, {Employee Name}

**Primary Key**: {Employee ID} (Chọn một trong các khóa ứng viên làm khóa chính)

### Kết Luận

Ràng buộc khóa là quy tắc quan trọng trong mô hình quan hệ để đảm bảo tính duy nhất và toàn vẹn của dữ liệu. Mỗi bảng phải có một khóa chính và không được phép có các bản ghi trùng lặp. Việc tuân thủ các quy tắc này giúp duy trì sự nhất quán và chính xác của dữ liệu trong hệ thống cơ sở dữ liệu.
