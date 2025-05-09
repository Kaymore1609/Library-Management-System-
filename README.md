# Library-Management-System-
-- Library Management System Database
-- Created by: [Kamogelo Aphane]
-- Date: [09/05/2025]


DROP DATABASE IF EXISTS library_management;
CREATE DATABASE library_management;
USE library_management;


CREATE TABLE publishers (
    publisher_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    address VARCHAR(200),
    phone VARCHAR(20),
    email VARCHAR(100),
    UNIQUE (name)
) COMMENT 'Stores publisher information';


CREATE TABLE authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    biography TEXT,
    birth_date DATE,
    nationality VARCHAR(50)
) COMMENT 'Stores author information';


CREATE TABLE categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    description TEXT,
    UNIQUE (name)
) COMMENT 'Book categories/genres';


CREATE TABLE shelves (
    shelf_id INT AUTO_INCREMENT PRIMARY KEY,
    location VARCHAR(50) NOT NULL,
    description VARCHAR(100),
    UNIQUE (location)
) COMMENT 'Physical book locations in the library';


CREATE TABLE books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    ISBN VARCHAR(20) NOT NULL UNIQUE,
    title VARCHAR(200) NOT NULL,
    edition VARCHAR(20),
    pub_year INT,
    category_id INT,
    publisher_id INT,
    shelf_id INT,
    status ENUM('available', 'checked_out', 'lost', 'under_repair') DEFAULT 'available',
    FOREIGN KEY (category_id) REFERENCES categories(category_id),
    FOREIGN KEY (publisher_id) REFERENCES publishers(publisher_id),
    FOREIGN KEY (shelf_id) REFERENCES shelves(shelf_id)
) COMMENT 'Main book inventory';


CREATE TABLE book_authors (
    book_id INT NOT NULL,
    author_id INT NOT NULL,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES authors(author_id) ON DELETE CASCADE
) COMMENT 'Links books to their authors';


CREATE TABLE members (
    member_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(20),
    address VARCHAR(200),
    join_date DATE NOT NULL,
    membership_type ENUM('student', 'faculty', 'public') NOT NULL,
    status ENUM('active', 'inactive', 'suspended') DEFAULT 'active'
) COMMENT 'Library members information';


CREATE TABLE loans (
    loan_id INT AUTO_INCREMENT PRIMARY KEY,
    book_id INT NOT NULL,
    member_id INT NOT NULL,
    loan_date DATE NOT NULL,
    due_date DATE NOT NULL,
    return_date DATE,
    fine_amount DECIMAL(10,2) DEFAULT 0.00,
    FOREIGN KEY (book_id) REFERENCES books(book_id),
    FOREIGN KEY (member_id) REFERENCES members(member_id),
    CONSTRAINT chk_dates CHECK (due_date >= loan_date AND (return_date IS NULL OR return_date >= loan_date))
) COMMENT 'Book loan transactions';


CREATE TABLE fine_payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    loan_id INT NOT NULL,
    payment_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    amount DECIMAL(10,2) NOT NULL,
    payment_method ENUM('cash', 'credit_card', 'debit_card', 'online') NOT NULL,
    FOREIGN KEY (loan_id) REFERENCES loans(loan_id)
) COMMENT 'Records of fine payments';


CREATE INDEX idx_books_title ON books(title);
CREATE INDEX idx_books_status ON books(status);
CREATE INDEX idx_members_email ON members(email);
CREATE INDEX idx_loans_dates ON loans(loan_date, due_date, return_date);
CREATE INDEX idx_loans_member ON loans(member_id);
CREATE INDEX idx_loans_book ON loans(book_id);
