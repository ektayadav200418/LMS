
# librarymanagementsystem
 Design a relational database for a Library Management System (LMS) to manage books, members, staff, loans, and reservations. Key tables with primary and foreign keys ensure data integrity. Features include normalization to eliminate redundancy, constraints for consistency, and SQL queries for efficient operations, ensuring seamless managenent
1.Authors Table
CREATE TABLE Authors (
    author_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    dob DATE
);

INSERT INTO Authors (first_name, last_name, dob) VALUES
('Chetan', 'Bhagat', '1974-04-22'),
('Ruskin', 'Bond', '1934-05-19'),
('Arundhati', 'Roy', '1961-11-24'),
('R.K.', 'Narayan', '1906-10-10'),
('Jhumpa', 'Lahiri', '1967-07-11'),
('Kiran', 'Desai', '1971-09-03'); 

2. Publishers Table
CREATE TABLE Publishers (
    publisher_id INT PRIMARY KEY AUTO_INCREMENT,
    publisher_name VARCHAR(255) NOT NULL,
    address VARCHAR(255),
    contact_number VARCHAR(15)
);

3. Categories Table
CREATE TABLE Categories (
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(100) NOT NULL
);

4. Books Table
CREATE TABLE Books (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    author_id INT,
    publisher_id INT,
    category_id INT,
    publication_year INT,
    ISBN VARCHAR(20),
    quantity INT DEFAULT 1,
    FOREIGN KEY (author_id) REFERENCES Authors(author_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (publisher_id) REFERENCES Publishers(publisher_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (category_id) REFERENCES Categories(category_id) ON DELETE CASCADE ON UPDATE CASCADE
);
5. Users Table
CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    phone VARCHAR(15),
    membership_date DATE,
    membership_type VARCHAR(50) -- e.g., Regular, Premium
);
6. Loans Table
CREATE TABLE Loans (
    loan_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    book_id INT,
    loan_date DATE,
    due_date DATE,
    return_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE
);
7. Reservations Table
CREATE TABLE Reservations (
    reservation_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    book_id INT,
    reservation_date DATE,
    status VARCHAR(20), -- e.g., Pending, Fulfilled
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE
);
8. Payments Table
CREATE TABLE Payments (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    amount DECIMAL(10, 2),
    payment_date DATE,
    payment_method VARCHAR(50), -- e.g., Credit, Debit, Cash
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
9. Fines Table
CREATE TABLE Fines (
    fine_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    loan_id INT,
    fine_amount DECIMAL(10, 2),
    fine_date DATE,
    paid BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (loan_id) REFERENCES Loans(loan_id) ON DELETE CASCADE ON UPDATE CASCADE
);
10. Book Reviews Table
CREATE TABLE Book_Reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    book_id INT,
    review_text TEXT,
    rating INT CHECK(rating >= 1 AND rating <= 5), -- Rating from 1 to 5
    review_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE
);
11. Book Damages Table
CREATE TABLE Book_Damages (
    damage_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    user_id INT,
    damage_details TEXT,
    damage_date DATE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
12. Transactions Table
CREATE TABLE Transactions (
    transaction_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    transaction_type VARCHAR(50), -- e.g., Loan, Return, Reservation
    transaction_date DATE,
    details TEXT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
13. Library Events Table
CREATE TABLE Events (
    event_id INT PRIMARY KEY AUTO_INCREMENT,
    event_name VARCHAR(255),
    event_date DATE,
    event_details TEXT
);
14. Event Registration Table
CREATE TABLE Event_Registration (
    registration_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    event_id INT,
    registration_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (event_id) REFERENCES Events(event_id) ON DELETE CASCADE ON UPDATE CASCADE
);
15. Library Branches Table
CREATE TABLE Library_Branches (
    branch_id INT PRIMARY KEY AUTO_INCREMENT,
    branch_name VARCHAR(100),
    location VARCHAR(255),
    contact_number VARCHAR(15)
);
16. Branch Books Table
CREATE TABLE Branch_Books (
    branch_book_id INT PRIMARY KEY AUTO_INCREMENT,
    branch_id INT,
    book_id INT,
    quantity INT,
    FOREIGN KEY (branch_id) REFERENCES Library_Branches(branch_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE
);
17. Staff Table
CREATE TABLE Staff (
    staff_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    position VARCHAR(100),
    contact_number VARCHAR(15),
    email VARCHAR(100)
);
18. Librarians Table
CREATE TABLE Librarians (
    librarian_id INT PRIMARY KEY AUTO_INCREMENT,
    staff_id INT,
    hire_date DATE,
    FOREIGN KEY (staff_id) REFERENCES Staff(staff_id) ON DELETE CASCADE ON UPDATE CASCADE
);
19. Notifications Table
CREATE TABLE Notifications (
    notification_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    notification_date DATE,
    notification_text TEXT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
20. User Activity Logs Table
CREATE TABLE User_Activity_Logs (
    activity_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    activity_type VARCHAR(100),
    activity_date DATE,
    activity_details TEXT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
21. Library Announcements Table
CREATE TABLE Library_Announcements (
    announcement_id INT PRIMARY KEY AUTO_INCREMENT,
    announcement_text TEXT,
    announcement_date DATE
);
22. Bookshelf Table
CREATE TABLE Bookshelves (
    shelf_id INT PRIMARY KEY AUTO_INCREMENT,
    branch_id INT,
    shelf_number VARCHAR(50),
    FOREIGN KEY (branch_id) REFERENCES Library_Branches(branch_id) ON DELETE CASCADE ON UPDATE CASCADE
);
23. Book Shelf Assignment Table
CREATE TABLE Book_Shelf_Assignment (
    assignment_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    shelf_id INT,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (shelf_id) REFERENCES Bookshelves(shelf_id) ON DELETE CASCADE ON UPDATE CASCADE
);
24. Book Suggestion Table
CREATE TABLE Book_Suggestions (
    suggestion_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    book_title VARCHAR(255),
    author_name VARCHAR(255),
    suggestion_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
25. User Membership History Table
CREATE TABLE Membership_History (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    membership_type VARCHAR(50),
    change_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);
26. Popular Books Table
CREATE TABLE Popular_Books (
    record_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    borrow_count INT,
    review_count INT,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE
);
27. Book Author History Table
CREATE TABLE Book_Author_History (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    old_author_id INT,
    new_author_id INT,
    change_date DATE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (old_author_id) REFERENCES Authors(author_id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (new_author_id) REFERENCES Authors(author_id) ON DELETE CASCADE ON UPDATE CASCADE
);
28. Book Lending Limits Table
CREATE TABLE Lending_Limits (
    limit_id INT PRIMARY KEY AUTO_INCREMENT,
    membership_type VARCHAR(50),
    max_books INT,
    max_days INT
);
29. Book Inventory Table
CREATE TABLE Book_Inventory (
    inventory_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    available_copies INT,
    FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE ON UPDATE CASCADE
);
30. Library Fees Table
CREATE TABLE Library_Fees (
    fee_id INT PRIMARY KEY AUTO_INCREMENT,
    fee_type VARCHAR(100),
    amount DECIMAL(10, 2)
);



2. Publishers Table

INSERT INTO Publishers (publisher_name, address, contact_number) VALUES
('Penguin India', '3rd Floor, 11A, Sultanpur, New Delhi', '011-12345678'),
('HarperCollins India', 'C-27, Sector 58, Noida', '0120-2345678'),
('Rupa Publications', '234, Shahpur Jat, New Delhi', '011-34567890'),
('Oxford University Press', '74, Mahatma Gandhi Road, Kolkata', '033-22345678'),
('Jaico Publishing House', 'Bharat Building, 48, J. C. Road, Mumbai', '022-22334455'),
('Westland Publications', '39, Community Center, New Delhi', '011-45678901');

3. Categories Table
INSERT INTO Categories (category_name) VALUES
('Fiction'),
('Non-Fiction'),
('Science'),
('Philosophy'),
('History'),
('Biographies');
4. Books Table
INSERT INTO Books (title, author_id, publisher_id, category_id, publication_year, ISBN, quantity) VALUES
('The Alchemist', 1, 1, 1, 1993, '978-0062315007', 5),
('The Room on the Roof', 2, 3, 1, 1956, '978-0143065114', 3),
('The God of Small Things', 3, 1, 1, 1997, '978-0670085357', 4),
('Malgudi Days', 4, 2, 1, 1982, '978-8184000907', 6),
('Interpreter of Maladies', 5, 5, 6, 1999, '978-0618485216', 2),
('The Inheritance of Loss', 6, 6, 2, 2006, '978-0143033814', 3);
5. Users Table
INSERT INTO Users (first_name, last_name, email, phone, membership_date, membership_type) VALUES
('Amit', 'Sharma', 'amit.sharma@gmail.com', '9876543210', '2022-01-15', 'Regular'),
('Priya', 'Verma', 'priya.verma@yahoo.com', '9998887777', '2021-11-03', 'Premium'),
('Rajesh', 'Kumar', 'rajesh.kumar@outlook.com', '9456782345', '2022-03-11', 'Regular'),
('Sita', 'Patel', 'sita.patel@hotmail.com', '8889991122', '2021-07-19', 'Premium'),
('Vikram', 'Yadav', 'vikram.yadav@aol.com', '9034567890', '2023-02-22', 'Regular'),
('Neha', 'Singh', 'neha.singh@icloud.com', '9901223344', '2023-05-18', 'Regular');
6. Loans Table
INSERT INTO Loans (user_id, book_id, loan_date, due_date, return_date) VALUES
(1, 1, '2023-05-01', '2023-05-21', '2023-05-18'),
(2, 2, '2023-06-10', '2023-06-24', '2023-06-21'),
(3, 3, '2023-05-12', '2023-06-02', '2023-06-01'),
(4, 4, '2023-06-01', '2023-06-15', '2023-06-14'),
(5, 5, '2023-04-10', '2023-05-01', '2023-04-28'),
(6, 6, '2023-06-05', '2023-06-20', '2023-06-18');
7. Reservations Table
INSERT INTO Reservations (user_id, book_id, reservation_date, status) VALUES
(1, 2, '2023-06-02', 'Pending'),
(2, 3, '2023-05-15', 'Fulfilled'),
(3, 4, '2023-06-03', 'Pending'),
(4, 5, '2023-06-07', 'Fulfilled'),
(5, 6, '2023-05-22', 'Pending'),
(6, 1, '2023-06-04', 'Fulfilled');
8. Payments Table
INSERT INTO Payments (user_id, amount, payment_date, payment_method) VALUES
(1, 500.00, '2023-05-10', 'Credit'),
(2, 150.00, '2023-06-01', 'Debit'),
(3, 300.00, '2023-06-05', 'Cash'),
(4, 100.00, '2023-05-20', 'Credit'),
(5, 200.00, '2023-04-15', 'Debit'),
(6, 250.00, '2023-06-02', 'Cash');
9. Fines Table
INSERT INTO Fines (user_id, loan_id, fine_amount, fine_date, paid) VALUES
(1, 1, 50.00, '2023-05-19', TRUE),
(2, 2, 100.00, '2023-06-10', FALSE),
(3, 3, 150.00, '2023-06-04', FALSE),
(4, 4, 30.00, '2023-06-14', TRUE),
(5, 5, 200.00, '2023-04-25', TRUE),
(6, 6, 50.00, '2023-06-19', FALSE);
10. Book Reviews Table
INSERT INTO Book_Reviews (user_id, book_id, review_text, rating, review_date) VALUES
(1, 1, 'An inspiring and motivational read!', 5, '2023-05-19'),
(2, 2, 'A heartwarming story of growth and courage.', 4, '2023-06-08'),
(3, 3, 'An emotional and poignant tale of family.', 5, '2023-06-02'),
(4, 4, 'A collection of interesting short stories.', 3, '2023-06-11'),
(5, 5, 'Beautifully written and thought-provoking.', 4, '2023-04-30'),
(6, 6, 'A captivating story with rich characters.', 5, '2023-06-15');
11. Book Damages Table
INSERT INTO Book_Damages (book_id, user_id, damage_details, damage_date) VALUES
(1, 1, 'Minor tear on the cover', '2023-05-12'),
(2, 2, 'Pages 45-50 are torn', '2023-06-05'),
(3, 3, 'Water damage on several pages', '2023-05-25'),
(4, 4, 'Cover slightly bent', '2023-06-07'),
(5, 5, 'Pages crinkled', '2023-04-18'),
(6, 6, 'Spilled ink on a few pages', '2023-06-20');
12. Transactions Table
INSERT INTO Transactions (user_id, transaction_type, transaction_date, details) VALUES
(1, 'Loan', '2023-05-01', 'Loaned "The Alchemist"'),
(2, 'Reservation', '2023-06-02', 'Reserved "The Room on the Roof"'),
(3, 'Loan', '2023-05-12', 'Loaned "The God of Small Things"'),
(4, 'Return', '2023-06-01', 'Returned "Malgudi Days"'),
(5, 'Loan', '2023-04-10', 'Loaned "Interpreter of Maladies"'),
(6, 'Return', '2023-06-05', 'Returned "The Inheritance of Loss"');
13. Events Table
INSERT INTO Events (event_name, event_date, event_details) VALUES
('Book Launch: The Alchemist', '2023-07-10', 'Launch event for Chetan Bhagat\'s new edition of "The Alchemist".'),
('Seminar on Indian Literature', '2023-08-15', 'A discussion on the works of famous Indian authors.'),
('Reading Session for Kids', '2023-06-20', 'A fun-filled reading session for children aged 5-10 years.'),
('Poetry Night', '2023-07-22', 'An evening of poetry recitals by local poets.'),
('Book Fair 2023', '2023-09-05', 'A book fair with discounts and author signings.'),
('Author Meet and Greet', '2023-10-15', 'Meet the author of "The God of Small Things".');
14. Event Registration Table
INSERT INTO Event_Registration (user_id, event_id, registration_date) VALUES
(1, 1, '2023-07-01'),
(2, 2, '2023-08-10'),
(3, 3, '2023-06-15'),
(4, 4, '2023-07-10'),
(5, 5, '2023-09-01'),
(6, 6, '2023-10-05');
15. Library Branches Table
INSERT INTO Library_Branches (branch_name, location, contact_number) VALUES
('Central Library', 'Connaught Place, New Delhi', '011-23456789'),
('South Delhi Branch', 'Saket, New Delhi', '011-98765432'),
('Kolkata Branch', 'Salt Lake City, Kolkata', '033-11223344'),
('Bangalore Branch', 'MG Road, Bangalore', '080-22334455'),
('Mumbai Branch', 'Colaba, Mumbai', '022-33445566'),
('Chennai Branch', 'T. Nagar, Chennai', '044-44556677');
16. Branch Books Table
INSERT INTO Branch_Books (branch_id, book_id, quantity) VALUES
(1, 1, 3),
(1, 2, 2),
(2, 3, 4),
(3, 4, 5),
(4, 5, 2),
(5, 6, 3);
17. Staff Table
INSERT INTO Staff (first_name, last_name, position, contact_number, email) VALUES
('Arun', 'Mehta', 'Manager', '9812345670', 'arun.mehta@library.com'),
('Pooja', 'Sharma', 'Librarian', '9976543210', 'pooja.sharma@library.com'),
('Anil', 'Reddy', 'Assistant Librarian', '9998765432', 'anil.reddy@library.com'),
('Sunita', 'Patel', 'Clerk', '9966554433', 'sunita.patel@library.com'),
('Ravi', 'Gupta', 'Manager', '9898765432', 'ravi.gupta@library.com'),
('Neha', 'Singh', 'Security', '9801234567', 'neha.singh@library.com');
18. Librarians Table
INSERT INTO Librarians (staff_id, hire_date) VALUES
(2, '2019-06-12'),
(3, '2020-08-05'),
(5, '2021-01-20');
19. Notifications Table
INSERT INTO Notifications (user_id, notification_date, notification_text) VALUES
(1, '2023-06-15', 'Your reservation for "The Room on the Roof" has been fulfilled.'),
(2, '2023-06-20', 'Reminder: Return the book "The God of Small Things" by 2023-06-25.'),
(3, '2023-06-01', 'Your fine of ₹100 is due for payment.'),
(4, '2023-06-10', 'New book arrivals! Check out "Interpreter of Maladies".'),
(5, '2023-06-25', 'Your membership is expiring soon. Please renew your membership.'),
(6, '2023-07-01', 'Your book "The Inheritance of Loss" is due for return on 2023-07-05.');
20. User Activity Logs Table
INSERT INTO User_Activity_Logs (user_id, activity_type, activity_date, activity_details) VALUES
(1, 'Loan', '2023-05-01', 'Loaned "The Alchemist"'),
(2, 'Reservation', '2023-06-02', 'Reserved "The Room on the Roof"'),
(3, 'Payment', '2023-06-05', 'Paid ₹100 fine for overdue book'),
(4, 'Return', '2023-06-07', 'Returned "Malgudi Days"'),
(5, 'Loan', '2023-04-10', 'Loaned "Interpreter of Maladies"'),
(6, 'Review', '2023-06-10', 'Reviewed "The Inheritance of Loss" with a rating of 5.');
21. Library Announcements Table
INSERT INTO Library_Announcements (announcement_text, announcement_date) VALUES
('The library will be closed on 15th August for Independence Day.', '2023-08-01'),
('New books are available in the Science category!', '2023-07-01'),
('Book Fair 2023 is coming soon. Get ready for great discounts!', '2023-08-10'),
('We are offering 20% off on membership renewals until September!', '2023-07-15'),
('Seminar on Indian Literature will be held on 15th August. Register now.', '2023-08-05'),
('Please note: The library will remain closed for maintenance from 20th-22nd September.', '2023-09-01');
22. Bookshelves Table
INSERT INTO Bookshelves (branch_id, shelf_number) VALUES
(1, 'A1'),
(1, 'B2'),
(2, 'C3'),
(3, 'D4'),
(4, 'E5'),
(5, 'F6');
23. Book Shelf Assignment Table
INSERT INTO Book_Shelf_Assignment (book_id, shelf_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6);
24. Book Suggestion Table
INSERT INTO Book_Suggestions (user_id, book_title, author_name, suggestion_date) VALUES
(1, 'Sapiens: A Brief History of Humankind', 'Yuval Noah Harari', '2023-06-15'),
(2, 'Educated', 'Tara Westover', '2023-06-05'),
(3, 'The Silent Patient', 'Alex Michaelides', '2023-06-01'),
(4, 'Atomic Habits', 'James Clear', '2023-06-10'),
(5, 'Where the Crawdads Sing', 'Delia Owens', '2023-07-01'),
(6, 'The Midnight Library', 'Matt Haig', '2023-06-20');
25. User Membership History Table
INSERT INTO Membership_History (user_id, membership_type, change_date) VALUES
(1, 'Premium', '2023-01-10'),
(2, 'Regular', '2022-06-20'),
(3, 'Premium', '2023-03-15'),
(4, 'Regular', '2021-08-30'),
(5, 'Premium', '2022-11-05'),
(6, 'Regular', '2023-04-18');
26. Popular Books Table
INSERT INTO Popular_Books (book_id, borrow_count, review_count) VALUES
(1, 120, 25),
(2, 75, 30),
(3, 50, 20),
(4, 90, 35),
(5, 65, 28),
(6, 110, 40);
27. Book Author History Table
INSERT INTO Book_Author_History (book_id, old_author_id, new_author_id, change_date) VALUES
(1, 1, 2, '2023-06-05'),
(2, 2, 3, '2023-05-25'),
(3, 3, 4, '2023-04-15'),
(4, 4, 5, '2023-03-01'),
(5, 5, 6, '2023-07-01'),
(6, 6, 1, '2023-06-12');
28. Lending Limits Table
INSERT INTO Lending_Limits (membership_type, max_books, max_days) VALUES
('Regular', 5, 14),
('Premium', 10, 30);
29. Book Inventory Table
INSERT INTO Book_Inventory (book_id, available_copies) VALUES
(1, 5),
(2, 4),
(3, 3),
(4, 6),
(5, 2),
(6, 3);
30. Library Fees Table
INSERT INTO Library_Fees (fee_type, amount) VALUES
('Late Fee', 10.00),
('Membership Fee', 200.00),
('Event Registration Fee', 50.00),
('Book Reservation Fee', 20.00),
('Damage Fee', 100.00),
('Lost Book Fee', 500.00);


1. Authors Table
SELECT * FROM Authors;
2. Publishers Table
SELECT * FROM Publishers;
3. Categories Table
SELECT * FROM Categories;
4. Books Table
SELECT * FROM Books;
5. Users Table
SELECT * FROM Users;
6. Loans Table
SELECT * FROM Loans;
7. Reservations Table
SELECT * FROM Reservations;
8. Payments Table
SELECT * FROM Payments;
9. Fines Table
SELECT * FROM Fines;
10. Book Reviews Table
SELECT * FROM Book_Reviews;
11. Book Damages Table
SELECT * FROM Book_Damages;
12. Transactions Table
SELECT * FROM Transactions;
13. Events Table
SELECT * FROM Events;
14. Event Registration Table
SELECT * FROM Event_Registration;
15. Library Branches Table
SELECT * FROM Library_Branches;
16. Branch Books Table
SELECT * FROM Branch_Books;
17. Staff Table
SELECT * FROM Staff;
18. Librarians Table
SELECT * FROM Librarians;
19. Notifications Table
SELECT * FROM Notifications;
20. User Activity Logs Table
SELECT * FROM User_Activity_Logs;
21. Library Announcements Table
SELECT * FROM Library_Announcements;
22. Bookshelves Table
SELECT * FROM Bookshelves;
23. Book Shelf Assignment Table
SELECT * FROM Book_Shelf_Assignment;
24. Book Suggestion Table
SELECT * FROM Book_Suggestions;
25. User Membership History Table
SELECT * FROM Membership_History;
26. Popular Books Table
SELECT * FROM Popular_Books;
27. Book Author History Table
SELECT * FROM Book_Author_History;
28. Lending Limits Table
SELECT * FROM Lending_Limits;
29. Book Inventory Table
SELECT * FROM Book_Inventory;
30. Library Fees Table
SELECT * FROM Library_Fees;

1. Get All Books with Author, Publisher, and Category
SELECT b.title, a.first_name AS author_first_name, a.last_name AS author_last_name, 
       p.publisher_name, c.category_name
FROM Books b
JOIN Authors a ON b.author_id = a.author_id
JOIN Publishers p ON b.publisher_id = p.publisher_id
JOIN Categories c ON b.category_id = c.category_id;
2. Get All Users Who Have Reserved Books
SELECT u.first_name, u.last_name, r.book_id, r.reservation_date
FROM Users u
JOIN Reservations r ON u.user_id = r.user_id
WHERE r.status = 'Pending';
3. Get All Books Currently on Loan
SELECT b.title, l.loan_date, l.due_date, u.first_name AS user_first_name, u.last_name AS user_last_name
FROM Loans l
JOIN Books b ON l.book_id = b.book_id
JOIN Users u ON l.user_id = u.user_id
WHERE l.return_date IS NULL;
4. Get All Payments Made by Users
SELECT u.first_name, u.last_name, p.amount, p.payment_date, p.payment_method
FROM Payments p
JOIN Users u ON p.user_id = u.user_id;
5. Get All Fines Collected
SELECT u.first_name, u.last_name, f.fine_amount, f.fine_date, f.paid
FROM Fines f
JOIN Users u ON f.user_id = u.user_id;
6. Get Book Reviews with Rating
SELECT b.title, r.review_text, r.rating, u.first_name AS user_first_name, u.last_name AS user_last_name
FROM Book_Reviews r
JOIN Books b ON r.book_id = b.book_id
JOIN Users u ON r.user_id = u.user_id;
7. Get Books Reserved by a Specific User
SELECT b.title, r.reservation_date, r.status
FROM Reservations r
JOIN Books b ON r.book_id = b.book_id
WHERE r.user_id = 1;  -- Replace 1 with a specific user_id
8. Get the Most Popular Books (by Borrow Count)
SELECT b.title, p.borrow_count
FROM Popular_Books p
JOIN Books b ON p.book_id = b.book_id
ORDER BY p.borrow_count DESC;
9. Get All Books That Are Currently Available in the Library
SELECT b.title, bi.available_copies
FROM Book_Inventory bi
JOIN Books b ON bi.book_id = b.book_id
WHERE bi.available_copies > 0;
10. Get All Events Happening in the Library
SELECT * FROM Events;
11. Get Users Who Have Registered for Events
SELECT u.first_name, u.last_name, e.event_name, er.registration_date
FROM Event_Registration er
JOIN Users u ON er.user_id = u.user_id
JOIN Events e ON er.event_id = e.event_id;
12. Get a Summary of Books Damaged by Users
SELECT b.title, d.damage_details, u.first_name AS user_first_name, u.last_name AS user_last_name
FROM Book_Damages d
JOIN Books b ON d.book_id = b.book_id
JOIN Users u ON d.user_id = u.user_id
13. Get All Events in the Library
SELECT * FROM Events;
14. Get Users Who Have Registered for Specific Events
SELECT u.first_name, u.last_name, e.event_name, er.registration_date
FROM Event_Registration er
JOIN Users u ON er.user_id = u.user_id
JOIN Events e ON er.event_id = e.event_id;
15. Get All Branches in the Library
SELECT * FROM Library_Branches;
16. Get All Books in a Specific Branch
SELECT b.title, bb.quantity
FROM Branch_Books bb
JOIN Books b ON bb.book_id = b.book_id
WHERE bb.branch_id = 1;  -- Replace with branch_id to get books for specific branch
17. Get All Staff in the Library
SELECT * FROM Staff;
18. Get Librarians Information
SELECT s.first_name, s.last_name, l.hire_date
FROM Librarians l
JOIN Staff s ON l.staff_id = s.staff_id;
19. Get All Notifications Sent to Users
SELECT u.first_name, u.last_name, n.notification_date, n.notification_text
FROM Notifications n
JOIN Users u ON n.user_id = u.user_id;
20. Get User Activity Logs
SELECT u.first_name, u.last_name, ul.activity_type, ul.activity_date, ul.activity_details
FROM User_Activity_Logs ul
JOIN Users u ON ul.user_id = u.user_id;
21. Get All Library Announcements
SELECT * FROM Library_Announcements;
22. Get All Bookshelves in a Branch
SELECT b.branch_name, s.shelf_number
FROM Bookshelves s
JOIN Library_Branches b ON s.branch_id = b.branch_id;
23. Get All Books Assigned to Shelves
SELECT b.title, bs.shelf_number
FROM Book_Shelf_Assignment bs
JOIN Books b ON bs.book_id = b.book_id
JOIN Bookshelves s ON bs.shelf_id = s.shelf_id;
24. Get All Book Suggestions Made by Users
SELECT u.first_name, u.last_name, b.book_title, b.author_name, bs.suggestion_date
FROM Book_Suggestions bs
JOIN Users u ON bs.user_id = u.user_id;
25. Get User Membership History
SELECT u.first_name, u.last_name, mh.membership_type, mh.change_date
FROM Membership_History mh
JOIN Users u ON mh.user_id = u.user_id;
26. Get Popular Books by Borrow Count
SELECT b.title, pb.borrow_count
FROM Popular_Books pb
JOIN Books b ON pb.book_id = b.book_id
ORDER BY pb.borrow_count DESC;
27. Get Author History for a Specific Book
SELECT b.title, ah.old_author_id, ah.new_author_id, ah.change_date
FROM Book_Author_History ah
JOIN Books b ON ah.book_id = b.book_id
WHERE b.book_id = 1;  -- Replace 1 with a specific book_id
28. Get Lending Limits for Membership Types
SELECT * FROM Lending_Limits;
29. Get Book Inventory in All Branches
SELECT b.title, bi.available_copies, bb.branch_id
FROM Book_Inventory bi
JOIN Books b ON bi.book_id = b.book_id
JOIN Branch_Books bb ON b.book_id = bb.book_id;
30. Get All Library Fees
SELECT * FROM Library_Fees;

1. Get All Books Loaned by a Specific User
SELECT b.title, l.loan_date, l.due_date, l.return_date
FROM Loans l
JOIN Books b ON l.book_id = b.book_id
WHERE l.user_id = 1;  -- Replace 1 with a specific user_id
2. Get All Books with Their Current Availability and Reservation Status
SELECT b.title, bi.available_copies, r.status AS reservation_status
FROM Books b
JOIN Book_Inventory bi ON b.book_id = bi.book_id
LEFT JOIN Reservations r ON b.book_id = r.book_id
WHERE bi.available_copies > 0;  -- Shows only available books
3. Get All Overdue Books
SELECT b.title, u.first_name, u.last_name, l.due_date, l.loan_date
FROM Loans l
JOIN Books b ON l.book_id = b.book_id
JOIN Users u ON l.user_id = u.user_id
WHERE l.return_date IS NULL AND l.due_date < CURRENT_DATE;
4. Get Fine Details for Users Who Have Overdue Books
SELECT u.first_name, u.last_name, f.fine_amount, f.paid, f.fine_date
FROM Fines f
JOIN Users u ON f.user_id = u.user_id
WHERE f.paid = 'No';  -- Show users who haven't paid fines
5. Get Total Number of Books Loaned by Each User
SELECT u.first_name, u.last_name, COUNT(l.book_id) AS books_loaned
FROM Loans l
JOIN Users u ON l.user_id = u.user_id
GROUP BY u.user_id;
6. Get All Books Borrowed by a Specific User along with Due Date
SELECT b.title, l.due_date
FROM Loans l
JOIN Books b ON l.book_id = b.book_id
WHERE l.user_id = 2;  -- Replace 2 with the user_id of the person you're interested in
7. Get All Payments Made for Fines
SELECT p.amount, p.payment_method, p.payment_date, f.fine_amount, u.first_name, u.last_name
FROM Payments p
JOIN Fines f ON p.transaction_id = f.transaction_id
JOIN Users u ON f.user_id = u.user_id;
8. Get Events Registered by Users in a Specific Month
SELECT u.first_name, u.last_name, e.event_name, er.registration_date
FROM Event_Registration er
JOIN Users u ON er.user_id = u.user_id
JOIN Events e ON er.event_id = e.event_id
WHERE MONTH(er.registration_date) = 7;  -- Replace with the month number you're interested in
9. Get the List of Books with the Most Reservations
SELECT b.title, COUNT(r.reservation_id) AS reservation_count
FROM Reservations r
JOIN Books b ON r.book_id = b.book_id
GROUP BY b.book_id
ORDER BY reservation_count DESC;
10. Get All Active Members Who Have Not Paid Fines
SELECT u.first_name, u.last_name, u.email
FROM Users u
LEFT JOIN Fines f ON u.user_id = f.user_id
WHERE f.fine_id IS NULL OR f.paid = 'No';  
