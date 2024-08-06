# Giới thiệu về Mô Hình Dữ Liệu (Data Model)

#### Mô Hình Dữ Liệu Là Gì?

Mô hình dữ liệu (Data model) được sử dụng để biểu diễn cấu trúc của dữ liệu trong cơ sở dữ liệu. Bạn đã biết rằng cơ sở dữ liệu là một tập hợp dữ liệu. Bây giờ chúng ta có một tập hợp dữ liệu, nhưng làm thế nào để cấu trúc dữ liệu này?

Mô hình dữ liệu là cách chúng ta cấu trúc dữ liệu trong cơ sở dữ liệu. Có hai cách chính để cấu trúc dữ liệu trong cơ sở dữ liệu, đó là:
- Mô hình ER (Entity-Relationship Model)
- Mô hình quan hệ (Relational Model)

Đây là hai mô hình quan trọng mà chúng ta sẽ xem xét trong khóa học này.

#### Mô Hình ER (Entity-Relationship Model)

Mô hình ER được sử dụng để biểu diễn cấu trúc của cơ sở dữ liệu dưới dạng sơ đồ. Trong sơ đồ này, mỗi ký hiệu đều có một ý nghĩa riêng. Chúng ta sẽ tìm hiểu chi tiết sau, nhưng hiện tại, hãy hiểu rằng mô hình ER giúp chúng ta hình dung cấu trúc cơ sở dữ liệu bằng sơ đồ, dễ dàng hơn cho việc hiểu và thiết kế cơ sở dữ liệu.

#### Mô Hình Quan Hệ (Relational Model)

Mô hình quan hệ biểu diễn dữ liệu dưới dạng bảng (table). Trong mô hình này:
- **Bảng (Relation)**: Còn được gọi là "relation".
- **Hàng (Row)**: Còn được gọi là "record".
- **Cột (Column)**: Còn được gọi là "field". Ví dụ, cột Employee ID có thể được gọi là trường Employee ID.

Mô hình quan hệ giúp chúng ta biểu diễn dữ liệu trong cơ sở dữ liệu dưới dạng các bảng, mỗi bảng chứa nhiều hàng và cột.

#### Sự Khác Biệt Giữa Mô Hình ER và Mô Hình Quan Hệ

Điểm quan trọng cần hiểu là cả hai mô hình này đều biểu diễn cùng một thông tin, nhưng theo cách khác nhau. Mô hình ER tập trung vào việc biểu diễn tổng quát cấu trúc của cơ sở dữ liệu và mối quan hệ giữa các thực thể (entities), trong khi mô hình quan hệ tập trung vào việc biểu diễn chi tiết dữ liệu dưới dạng các bảng.

- **Mô Hình ER**: Dễ hiểu và trực quan cho người dùng mới. Nó giúp hình dung cách các thực thể liên kết với nhau trong cơ sở dữ liệu.
- **Mô Hình Quan Hệ**: Cung cấp chi tiết cụ thể về dữ liệu, chẳng hạn như tên, tuổi, giới tính của từng nhân viên trong cơ sở dữ liệu.

#### Ví Dụ Cụ Thể

Giả sử chúng ta có một bảng Employee trong mô hình quan hệ, bảng này chứa các cột như Employee ID, Name, Age, và Sex. Mỗi hàng trong bảng này đại diện cho một nhân viên cụ thể với thông tin chi tiết.

Trong mô hình ER, chúng ta có thể biểu diễn thực thể Employee với các thuộc tính (attributes) như EmpID, Name, Age, và Sex, và mối quan hệ (relationship) giữa Employee với các thực thể khác như Department.

#### Kết Luận

- **Mô Hình ER**: Được sử dụng để biểu diễn cấu trúc của cơ sở dữ liệu dưới dạng sơ đồ.
- **Mô Hình Quan Hệ**: Được sử dụng để biểu diễn dữ liệu trong cơ sở dữ liệu dưới dạng các bảng.

Cả hai mô hình đều có vai trò quan trọng trong việc thiết kế và quản lý cơ sở dữ liệu. Khi chúng ta hiểu rõ cách sử dụng cả hai mô hình này, chúng ta sẽ có thể thiết kế cơ sở dữ liệu một cách hiệu quả và dễ dàng quản lý dữ liệu trong cơ sở dữ liệu.
