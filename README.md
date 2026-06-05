-- Tạo cơ sở dữ liệu
CREATE DATABASE library_management;
USE library_management;

-- Bảng sách
CREATE TABLE books (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    author VARCHAR(100) NOT NULL,
    publish_year INT,
    quantity INT DEFAULT 0
);

-- Bảng độc giả
CREATE TABLE readers (
    reader_id INT PRIMARY KEY AUTO_INCREMENT,
    full_name VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    address VARCHAR(200)
);

-- Bảng phiếu mượn
CREATE TABLE borrow_records (
    record_id INT PRIMARY KEY AUTO_INCREMENT,
    reader_id INT,
    book_id INT,
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (reader_id) REFERENCES readers(reader_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id)
);

-- Thêm dữ liệu vào bảng books
INSERT INTO books (title, author, publish_year, quantity)
VALUES
('Lap Trinh Python', 'Nguyen Van A', 2021, 10),
('Co So Du Lieu', 'Tran Van B', 2020, 8),
('Java Can Ban', 'Le Van C', 2022, 12),
('Hoc SQL', 'Pham Van D', 2023, 15),
('Cau Truc Du Lieu', 'Hoang Van E', 2019, 7);

-- Thêm dữ liệu vào bảng readers
INSERT INTO readers (full_name, phone, address)
VALUES
('Nguyen Thi Lan', '0987654321', 'Ha Noi'),
('Tran Van Minh', '0912345678', 'Hai Phong'),
('Le Thi Hoa', '0977123456', 'Da Nang');

-- Thêm dữ liệu vào bảng borrow_records
INSERT INTO borrow_records (reader_id, book_id, borrow_date, return_date)
VALUES
(1, 1, '2025-06-01', '2025-06-10'),
(2, 2, '2025-06-02', '2025-06-11'),
(1, 3, '2025-06-03', '2025-06-12'),
(3, 1, '2025-06-04', '2025-06-13'),
(2, 1, '2025-06-05', '2025-06-14');

-- 1. Hiển thị toàn bộ sách
SELECT * FROM books;

-- 2. Tìm sách xuất bản sau năm 2020
SELECT * FROM books
WHERE publish_year > 2020;

-- 3. Hiển thị thông tin độc giả đã mượn sách
SELECT r.reader_id, r.full_name, b.title
FROM readers r
JOIN borrow_records br ON r.reader_id = br.reader_id
JOIN books b ON br.book_id = b.book_id;

-- 4. Đếm tổng số lượt mượn sách
SELECT COUNT(*) AS total_borrow
FROM borrow_records;

-- 5. Tìm cuốn sách được mượn nhiều nhất
SELECT b.book_id, b.title, COUNT(*) AS borrow_count
FROM borrow_records br
JOIN books b ON br.book_id = b.book_id
GROUP BY b.book_id, b.title
ORDER BY borrow_count DESC
LIMIT 1;

-- 6. Cập nhật số điện thoại độc giả
UPDATE readers
SET phone = '0909999999'
WHERE reader_id = 1;

-- 7. Xóa phiếu mượn theo record_id
DELETE FROM borrow_records
WHERE record_id = 5;