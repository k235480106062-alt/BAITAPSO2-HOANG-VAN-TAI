# BAITAPSO2-HOANG VAN TAI
MSSV .K235480106062 
Đề Tài . Quản lý sinh viên

Đề tài quản lý sinh viên là một hệ thống giúp lưu trữ, xử lý và khai thác thông tin liên quan đến sinh viên trong một trường học. Mục tiêu chính của đề tài là xây dựng một cơ sở dữ liệu khoa học, cho phép quản lý các thông tin như: mã sinh viên, họ tên, ngày sinh, lớp, khoa, điểm số và kết quả học tập.

Quá trình thực hiện đề tài thường gồm các bước chính sau:

-Phân tích yêu cầu: Xác định hệ thống cần quản lý những gì (sinh viên, môn học, điểm, giảng viên…).
-Thiết kế cơ sở dữ liệu: Xây dựng các bảng như SinhVien, MonHoc, KetQua, Lop…, xác định khóa chính, khóa ngoại và mối quan hệ giữa các bảng.
- Xây dựng chức năng: Viết các câu lệnh SQL để thêm, sửa, xóa, tìm kiếm dữ liệu; có thể sử dụng Trigger, View, Procedure để tự động hóa xử lý (ví dụ: tự động tính điểm trung bình).
- Xử lý nâng cao: Áp dụng CURSOR hoặc các câu lệnh SQL nâng cao để xử lý dữ liệu theo yêu cầu cụ thể.
- Kiểm thử và đánh giá: Chạy thử hệ thống, kiểm tra tính đúng đắn và hiệu năng.

Phần 1. Khởi tạo bảng 
- Mô tả logic

Hệ thống được thiết kế để quản lý sinh viên trong trường học, gồm các thực thể chính:

+ Khoa: Lưu thông tin các khoa
+ SinhVien: Lưu thông tin sinh viên, thuộc về một khoa
+ MonHoc: Lưu danh sách môn học
+ KetQua: Lưu điểm của sinh viên theo từng môn

- Quan hệ:
+ 1 Khoa có nhiều SinhVien (1-n)
+ 1 SinhVien có nhiều KetQua
+ 1 MonHoc có nhiều KetQua

- Ràng buộc:
+ PK (Primary Key): định danh duy nhất
+ FK (Foreign Key): liên kết bảng
+ CK (Check): kiểm tra dữ liệu hợp lệ
- Bài code:
-- XÓA DB CŨ (nếu có) để tránh lỗi
IF EXISTS (SELECT * FROM sys.databases WHERE name = 'QuanLySinhVien_K235480106062')
BEGIN

  DROP DATABASE QuanLySinhVien_K235480106062;

END
GO
-- TẠO DATABASE
CREATE DATABASE QuanLySinhVien_K235480106062;
GO
USE QuanLySinhVien_K235480106062;
GO
-- Bảng Khoa

CREATE TABLE [Khoa] (
    [MaKhoa] INT PRIMARY KEY, -- PK
    [TenKhoa] NVARCHAR(100) NOT NULL
);
-- Bảng SinhVien

CREATE TABLE [SinhVien] (

    [MaSV] INT PRIMARY KEY, -- PK

    [HoTen] NVARCHAR(100) NOT NULL,

    [NgaySinh] DATE,

    [GioiTinh] NVARCHAR(10),

    [SoDienThoai] VARCHAR(15),

    [MaKhoa] INT, -- FK

    CONSTRAINT FK_SinhVien_Khoa FOREIGN KEY ([MaKhoa])

    REFERENCES [Khoa]([MaKhoa])

);
CREATE TABLE [MonHoc] (

    [MaMon] INT PRIMARY KEY, -- PK

    [TenMon] NVARCHAR(100) NOT NULL,

    [SoTinChi] INT CHECK (SoTinChi > 0) -- CK

);
-- Bảng KetQua
CREATE TABLE [KetQua] (

  [MaSV] INT,

  [MaMon] INT,

  [Diem] FLOAT CHECK (Diem BETWEEN 0 AND 10), -- CK

  PRIMARY KEY ([MaSV], [MaMon]), -- PK kép

  CONSTRAINT FK_KetQua_SV FOREIGN KEY ([MaSV])

  REFERENCES [SinhVien]([MaSV]),

  CONSTRAINT FK_KetQua_Mon FOREIGN KEY ([MaMon])

   REFERENCES [MonHoc]([MaMon])

);

-- ======================

-- 2. THÊM DỮ LIỆU

-- ======================

-- Khoa

INSERT INTO [Khoa] VALUES

(1, N'Công nghệ thông tin'),

(2, N'Kinh tế'),

(3, N'Ngôn ngữ');

-- Sinh viên
INSERT INTO [SinhVien] VALUES

(1, N'Hoàng Văn Tài', '2005-06-11', N'Nam', '0964636694', 1),

(2, N'Nguyễn Văn A', '2004-02-10', N'Nam', '0912345678', 1),

(3, N'Trần Thị B', '2003-08-20', N'Nữ', '0987654321', 2);

-- Môn học
INSERT INTO [MonHoc] VALUES

(1, N'Lập trình Python', 3),

(2, N'Cơ sở dữ liệu', 3),

(3, N'Tiếng Anh', 2);

-- Kết quả
INSERT INTO [KetQua] VALUES

(1, 1, 8.5),

(1, 2, 7.0),

(2, 1, 6.5),

(3, 3, 9.0);

-- 3. KIỂM TRA DỮ LIỆU

SELECT * FROM [Khoa];

SELECT * FROM [SinhVien];

SELECT * FROM [MonHoc];
SELECT * FROM [KetQua];


- ảnh chạy ra kết quả
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b77752e7-2e8c-4e79-8640-7df92d0ac2d6" />
PHẦN 2. Xây dụng function
- một số hàm tiêu biểu
+ hàm getdate:lấy thời gian hiện tại
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0f0bc2dc-1a2b-4694-9cc6-98c062eb54fd" />
+ hamf newid: tạo mã ngẫu nhiên
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/429bfcf5-988e-43a9-968f-5431b1605f56" />
+ Ham LEN(): độ dài chuỗi
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2c915d54-af25-4300-a123-6d4177a0faf4" />
+ hàm ROUND() : làm tròn số
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6a5b0c5a-9004-4166-ac0a-a761d9add93b" />
- Hàm do người dùng tự viết trong SQL (User-defined Function)

 1. Mục đích của hàm tự định nghĩa

Hàm do người dùng tự viết (User-defined Function – UDF) được tạo ra nhằm:

+ Tái sử dụng logic: tránh viết lại nhiều lần cùng một đoạn SQL  
+ Đóng gói xử lý dữ liệu: giúp code rõ ràng, dễ hiểu hơn  
+ Hỗ trợ truy vấn phức tạp: như tính toán, lọc dữ liệu, xử lý chuỗi  
+ Tăng tính modular (chia nhỏ chức năng) trong hệ thống  

 2. Các loại Function trong SQL Server
-Scalar Function (Hàm vô hướng)
+ Trả về 1 giá trị duy nhất (INT, FLOAT, NVARCHAR…)
+ Có thể dùng trong SELECT, WHERE  
- Khi dùng:
+ Khi cần tính toán 1 giá trị đơn lẻ
- Inline Table-Valued Function
+ Trả về 1 bảng (table)  
+ Chỉ có 1 câu SELECT  
+ Không có BEGIN…END  
- Khi dùng:
+ Khi cần lọc dữ liệu đơn giản giống SELECT

- Multi-statement Table-Valued Function
+ Trả về bảng (table)  
+ Có thể dùng nhiều câu lệnh (BEGIN…END)  
+ Có bảng tạm  
- Khi dùng:
+ Khi cần xử lý phức tạp (nhiều bước)  
 3. Tại sao cần viết Function riêng khi đã có Built-in Function?
Mặc dù SQL Server đã có rất nhiều hàm có sẵn, nhưng vẫn cần viết hàm riêng vì:
-Built-in Function:
+ Chỉ xử lý các tác vụ cơ bản (chuỗi, số, ngày…)

- User-defined Function:
+ Cho phép xử lý theo nghiệp vụ riêng  
+ Có thể kết hợp nhiều bảng, nhiều điều kiện  
+ Phù hợp với từng bài toán cụ thể  

- Viết 01 Scalar Function (Hàm trả về một giá trị): Đưa ra 1 logic cho cơ sở dữ liệu của em, mà cần dùng đến function này. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)
Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/099c6a7b-7b54-45e7-a776-094961fe451a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/16a1fc28-28d4-4edc-b50b-323b509f1166" />
Viết 01 Inline Table-Valued Function: Trả về danh sách các bản ghi theo một điều kiện lọc cụ thể (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)
Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a377c4c-e7cf-4f22-afda-6178accbd86a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/82364170-e7c7-4235-b407-b765c44c511c" />
Viết 01 Multi-statement Table-Valued Function: Thực hiện xử lý logic phức tạp bên trong (có sử dụng biến bảng) trước khi trả về kết quả. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)
Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d1b1b76-1c25-4b54-9769-f59181f3b106" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b283cd64-3244-4a5f-abda-110f37d3d248" />

Phần 3. Xây dựng store procedure 
Một số System Stored Procedure phổ biến trong SQL Server:
1. sp_help
Công dụng:
Hiển thị thông tin về bảng, view, procedure hoặc đối tượng trong database.
Cú pháp:
sp_help TenBang
2. sp_helpdb
Công dụng:
Xem thông tin các database trong hệ thống.
Cú pháp:
sp_helpdb
Hoặc:
sp_helpdb QuanLySinhVien
3. sp_tables
Công dụng:
Liệt kê danh sách bảng trong database.
Cú pháp:
sp_tables
4. sp_columns
Công dụng:
Hiển thị thông tin cột của bảng.
Cú pháp:
sp_columns SinhVien
5. sp_rename
Công dụng:
Đổi tên bảng hoặc cột.
Cú pháp:
sp_rename 'TenCu', 'TenMoi'
6. sp_databases
Công dụng:
Liệt kê toàn bộ database trong SQL Server.
Cú pháp:
sp_databases
7. sp_who
Công dụng:
Kiểm tra người dùng và tiến trình đang kết nối SQL Server.
Cú pháp:
sp_who
8. sp_spaceused
công dụng:
Kiểm tra dung lượng database hoặc bảng.
Cú pháp:
sp_spaceused
Hoặc:
sp_spaceused SinhVien

Viết 01 Store Procedure đơn giản để thực hiện lệnh INSERT hoặc UPDATE dữ liệu, có kiểm tra điều kiện logic (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/143247b2-6e78-4944-a132-2d4d2b39192b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cfdcc27c-786b-4f39-a56f-f6daa57aebaf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8b0cc39c-f6bf-49e6-b223-aa71749642e6" />
-Viết 01 Store Procedure có sử dụng tham số OUTPUT để trả về một giá trị tính toán (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ, SP NÀY CÓ DÙNG THAM SỐ LOẠI OUTPUT)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7d4b2d6f-76df-42e0-828f-675de37738d4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/16fae90f-1104-47e8-9edb-1e5feb63aba7" />
Viết 01 Store Procedure trả về một tập kết quả (Result set) từ lệnh SELECT sau khi đã join nhiều bảng. (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/358617ea-3744-42b7-8224-6015b7e55f17" />

Phần 4  Trigger và Xử lý logic nghiệp vụ
Viết 01 Trigger để tự động làm gì đó tại 1 bảng B khi mà dữ liệu thay đổi dữ liệu ở bảng A. Logic giải quyết do sv tự nghĩ ra, sao cho thực tế và thuyết phục.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df490824-7737-4c01-8d75-e7662d9dffb7" />
Thử viết Trigger cho Bảng A : Khi insert thì cập nhật dữ liệu vào bảng B; sau đó viết trigger cho bảng B để khi B được cập nhật thì cập nhật sang bảng A : Quan sát các thông báo (nếu có) của hệ thống, giải thích các thông báo đó (nếu có). Đưa ra nhật xét cuối cùng về tình trạng này.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a3ec3282-2860-48eb-ac6f-100f35b4a6b0" />
- Giải thích

- Hai trigger được thiết kế liên kết hai chiều giữa bảng A và B. Khi dữ liệu ở bảng A thay đổi sẽ cập nhật sang bảng B, và ngược lại khi bảng B thay đổi sẽ cập nhật lại bảng A.
Tuy nhiên, cách thiết kế này có thể gây ra hiện tượng vòng lặp trigger (recursive trigger), làm giảm hiệu năng hoặc gây lỗi nếu không kiểm soát tốt. Vì vậy trong thực tế cần hạn chế hoặc có cơ chế kiểm tra để tránh lặp vô hạn.
Phần 5: Cursor và Duyệt dữ liệu 
Viết một đoạn script sử dụng CURSOR để duyệt qua danh sách của 1 câu lệnh SQL dạng SELECT, duyệt qua từng bản ghi, xử lý riêng từng bản ghi (THEO LOGIC SV TỰ ĐẶT RA: SAO CHO HỢP LÝ VÀ THUYẾT PHỤC)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6df06a74-ae11-4da5-b0fa-5a376ed12ca2" />
Tìm cách không sử dụng CURSOR để giải quyết bài toán mà em đã dùng CURSOR mới giải quyết được ở trên. thử so sánh tốc độ giữa có dùng cursor và không dùng cursor (nếu cùng kết quả) thì thời gian xử lý cái nào nhanh hơn, cần ảnh chụp màn hình minh chứng.
Thời gian DÙNG CURSOR
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d745adef-212b-4744-8089-8c199bef01cf" />
Thời gian Không dùng  CURSOR.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/44080f9c-d6b3-4a29-bffe-beacd026067b" />
Nếu vẫn tìm được cách dùng SQL để giải quyết vấn đề mà ko cần CURSOR: thử nghĩ bài toán khác, mà chỉ CURSOR mới giải quyết được, còn SQL rất khó giải quyết đc (theo logic suy nghĩ của em)

Trong thực tế, hầu hết các bài toán đều có thể giải bằng SQL mà không cần CURSOR. Tuy nhiên, đối với các bài toán yêu cầu xử lý tuần tự từng bản ghi, phụ thuộc vào kết quả trước đó (ví dụ: tính toán tích luỹ, so sánh dữ liệu liên tiếp), việc sử dụng CURSOR giúp code dễ hiểu và trực quan hơn.
Ngược lại, SQL thuần (set-based) tuy vẫn giải được nhưng thường phức tạp và khó triển khai hơn.





