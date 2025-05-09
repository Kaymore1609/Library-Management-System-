# Library Management System Database

![MySQL](https://img.shields.io/badge/MySQL-8.0+-blue)
![Database](https://img.shields.io/badge/Type-Relational_DB-orange)

A comprehensive MySQL database solution for managing library operations including book tracking, member management, loans, and fines.

## Features

- **Book Management**: Track books, authors, publishers, and categories
- **Member System**: Manage library members and their accounts
- **Loan Processing**: Handle book checkouts, returns, and due dates
- **Fine System**: Automatically calculate and track overdue fines
- **Reporting**: Generate insights on book popularity and overdue items

## Database Schema

### Entity Relationship Diagram
![ERD Diagram](https://via.placeholder.com/800x500.png?text=Library+ERD+Diagram)

### Tables
| Table | Description |
|-------|-------------|
| `books` | Stores book information |
| `authors` | Contains author details |
| `publishers` | Publisher information |
| `categories` | Book genres/categories |
| `members` | Library member records |
| `loans` | Book checkout transactions |
| `fine_payments` | Fine payment records |

## Installation

1. **Prerequisites**:
   - MySQL Server (8.0+ recommended)
   - MySQL Workbench or command-line client

2. **Setup**:
   ```bash
   # Clone the repository
   git clone https://github.com/yourusername/library-management-db.git
   cd library-management-db
   
   # Import the database
   mysql -u username -p < library_management.sql
