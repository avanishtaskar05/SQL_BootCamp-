CREATE DATABASE fincoredb;

USE fincoredb;

CREATE TABLE dim_customers (
    customer_key INT AUTO_INCREMENT PRIMARY KEY,
    customer_id VARCHAR(20) NOT NULL UNIQUE,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    gender ENUM('Male', 'Female', 'Other') DEFAULT 'Male',
    date_of_birth DATE,
    mobile_number VARCHAR(15),
    email VARCHAR(30),
    city VARCHAR(30),
    state VARCHAR(30),
    country VARCHAR(30),
    customer_type VARCHAR(30),
    KYC_Status VARCHAR(14)
);

CREATE TABLE dim_accounts (
    account_key INT AUTO_INCREMENT PRIMARY KEY,
    account_number VARCHAR(20) NOT NULL UNIQUE,
    account_type VARCHAR(30),
    account_status VARCHAR(20),
    currency VARCHAR(10),
    opening_date DATE,
    minimum_balance DECIMAL(15 , 2 ),
    nominee_available BOOLEAN
);

CREATE TABLE dim_branches (
    branch_key INT AUTO_INCREMENT PRIMARY KEY,
    branch_code VARCHAR(20) UNIQUE,
    branch_name VARCHAR(40),
    city VARCHAR(40),
    state VARCHAR(30),
    region VARCHAR(30),
    ifsc_code VARCHAR(11)
);

CREATE TABLE dim_dates (
    date_key INT PRIMARY KEY UNIQUE,
    full_date DATE,
    account_opening_date DATE
);

CREATE TABLE dim_employees (
    employee_key INT AUTO_INCREMENT PRIMARY KEY,
    employee_id VARCHAR(20) UNIQUE,
    employee_name VARCHAR(20),
    designation VARCHAR(30),
    department VARCHAR(40),
    branch_name VARCHAR(40),
    hire_date DATE
);

CREATE TABLE dim_transaction_types (
    transaction_type_key INT AUTO_INCREMENT PRIMARY KEY,
    transaction_code VARCHAR(20) UNIQUE,
    transaction_name VARCHAR(50),
    transaction_category VARCHAR(50)
);

CREATE TABLE dim_channels (
    channel_key INT AUTO_INCREMENT PRIMARY KEY,
    channel_code VARCHAR(20) UNIQUE,
    channel_name VARCHAR(40),
    channel_type VARCHAR(40)
);

CREATE TABLE dim_loan_products (
    loan_product_Key INT AUTO_INCREMENT PRIMARY KEY,
    loan_product_Code VARCHAR(20) UNIQUE,
    loan_product_Name VARCHAR(100),
    loan_category VARCHAR(50),
    maximum_tenure INT
);

CREATE TABLE dim_cards (
    card_key INT AUTO_INCREMENT PRIMARY KEY,
    card_number VARCHAR(25) UNIQUE,
    card_type VARCHAR(30),
    card_network VARCHAR(30),
    card_category VARCHAR(30),
    issue_date DATE,
    expiry_date DATE,
    card_status VARCHAR(20)
);

CREATE TABLE fact_transactions (
    transaction_fact_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    customer_key INT NOT NULL,
    account_key INT NOT NULL,
    branch_key INT NOT NULL,
    date_key INT NOT NULL,
    employee_key INT,
    transaction_type_key INT NOT NULL,
    channel_key INT NOT NULL,
    transaction_amount DECIMAL(18 , 2 ) NOT NULL,
    transaction_fee DECIMAL(18 , 2 ) DEFAULT 0,
    balance_after_transaction DECIMAL(18 , 2 ),
    transaction_count INT DEFAULT 1,
    FOREIGN KEY (customer_key)
        REFERENCES Dim_Customers (Customer_Key),
    FOREIGN KEY (account_key)
        REFERENCES Dim_Accounts (Account_Key),
    FOREIGN KEY (branch_key)
        REFERENCES Dim_Branches (Branch_Key),
    FOREIGN KEY (date_key)
        REFERENCES Dim_Dates (Date_Key),
    FOREIGN KEY (employee_key)
        REFERENCES Dim_Employees (Employee_Key),
    FOREIGN KEY (transaction_type_key)
        REFERENCES Dim_Transaction_Types (Transaction_Type_Key),
    FOREIGN KEY (channel_key)
        REFERENCES Dim_Channels (Channel_Key)
);

CREATE TABLE Fact_Loans (
    loan_fact_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    customer_key INT NOT NULL,
    branch_key INT NOT NULL,
    employee_key INT,
    date_key INT NOT NULL,
    loan_product_key INT NOT NULL,
    loan_amount DECIMAL(18 , 2 ) NOT NULL,
    interest_rate DECIMAL(5 , 2 ),
    emi_amount DECIMAL(18 , 2 ),
    outstanding_balance DECIMAL(18 , 2 ),
    loan_count INT DEFAULT 1,
    FOREIGN KEY (Customer_Key)
        REFERENCES dim_customers (customer_Key),
    FOREIGN KEY (Branch_Key)
        REFERENCES Dim_Branches (Branch_Key),
    FOREIGN KEY (Employee_Key)
        REFERENCES Dim_Employees (Employee_Key),
    FOREIGN KEY (Date_Key)
        REFERENCES Dim_Dates (Date_Key),
    FOREIGN KEY (Loan_Product_Key)
        REFERENCES Dim_Loan_Products (Loan_Product_Key)
);

CREATE TABLE fact_card_transactions (
    card_transaction_fact_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    customer_key INT NOT NULL,
    card_key INT NOT NULL,
    branch_key INT NOT NULL,
    date_key INT NOT NULL,
    channel_key INT NOT NULL,
    transaction_amount DECIMAL(18 , 2 ),
    cashback_amount DECIMAL(18 , 2 ),
    reward_points INT DEFAULT 0,
    transaction_count INT DEFAULT 1,
    FOREIGN KEY (Customer_Key)
        REFERENCES Dim_Customers (Customer_Key),
    FOREIGN KEY (Card_Key)
        REFERENCES Dim_Cards (Card_Key),
    FOREIGN KEY (Branch_Key)
        REFERENCES Dim_Branches (Branch_Key),
    FOREIGN KEY (Date_Key)
        REFERENCES Dim_Dates (Date_Key),
    FOREIGN KEY (Channel_Key)
        REFERENCES Dim_Channels (Channel_Key)
);

CREATE TABLE fact_fixed_deposits (
    fd_fact_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    customer_Key INT NOT NULL,
    branch_Key INT NOT NULL,
    employee_Key INT,
    date_Key INT NOT NULL,
    deposit_amount DECIMAL(18 , 2 ),
    interest_rate DECIMAL(5 , 2 ),
    maturity_amount DECIMAL(18 , 2 ),
    deposit_term_months INT,
    FOREIGN KEY (Customer_Key)
        REFERENCES Dim_Customers (Customer_Key),
    FOREIGN KEY (Branch_Key)
        REFERENCES Dim_Branches (Branch_Key),
    FOREIGN KEY (Employee_Key)
        REFERENCES Dim_Employees (Employee_Key),
    FOREIGN KEY (Date_Key)
        REFERENCES Dim_Dates (Date_Key)
);
INSERT INTO Dim_Customers
(Customer_ID, First_Name, Last_Name, Gender, Date_of_Birth, Mobile_Number, Email, City, State, Country, Customer_Type, KYC_Status)
VALUES
('CUST1001','Rahul','Sharma','Male','1992-05-10','9876543210','rahul@gmail.com','Mumbai','Maharashtra','India','Retail','Verified'),
('CUST1002','Priya','Patil','Female','1995-07-18','9876543211','priya@gmail.com','Pune','Maharashtra','India','Retail','Verified'),
('CUST1003','Amit','Verma','Male','1988-11-20','9876543212','amit@gmail.com','Delhi','Delhi','India','Corporate','Verified'),
('CUST1004','Sneha','Joshi','Female','1991-02-15','9876543213','sneha@gmail.com','Nagpur','Maharashtra','India','Retail','Pending'),
('CUST1005','Rohan','Mehta','Male','1986-09-30','9876543214','rohan@gmail.com','Ahmedabad','Gujarat','India','HNI','Verified'),
('CUST1006','Anjali','Singh','Female','1994-12-11','9876543215','anjali@gmail.com','Lucknow','UP','India','Retail','Verified'),
('CUST1007','Vikas','Kumar','Male','1990-04-22','9876543216','vikas@gmail.com','Jaipur','Rajasthan','India','Corporate','Verified'),
('CUST1008','Neha','Gupta','Female','1993-06-17','9876543217','neha@gmail.com','Indore','MP','India','Retail','Verified'),
('CUST1009','Karan','Malhotra','Male','1987-08-08','9876543218','karan@gmail.com','Chandigarh','Punjab','India','HNI','Verified'),
('CUST1010','Pooja','Kulkarni','Female','1996-01-12','9876543219','pooja@gmail.com','Nashik','Maharashtra','India','Retail','Verified');

INSERT INTO Dim_Accounts
(Account_Number,Account_Type,Account_Status,Currency,Opening_Date,Minimum_Balance,Nominee_Available)
VALUES
('ACC10001','Savings','Active','INR','2022-01-10',5000,TRUE),
('ACC10002','Current','Active','INR','2021-03-12',10000,TRUE),
('ACC10003','Savings','Active','INR','2020-05-15',5000,FALSE),
('ACC10004','Salary','Active','INR','2023-01-20',0,TRUE),
('ACC10005','Savings','Dormant','INR','2019-07-10',5000,TRUE),
('ACC10006','Current','Active','INR','2021-09-18',10000,TRUE),
('ACC10007','Savings','Active','INR','2022-12-05',5000,FALSE),
('ACC10008','Salary','Active','INR','2023-04-01',0,TRUE),
('ACC10009','Savings','Active','INR','2024-01-15',5000,TRUE),
('ACC10010','Current','Active','INR','2020-08-22',10000,FALSE);

INSERT INTO Dim_Branches
(Branch_Code,Branch_Name,City,State,Region,IFSC_Code)
VALUES
('BR001','Mumbai Main','Mumbai','Maharashtra','West','FIN000001'),
('BR002','Pune City','Pune','Maharashtra','West','FIN000002'),
('BR003','Delhi Central','Delhi','Delhi','North','FIN000003'),
('BR004','Nagpur Branch','Nagpur','Maharashtra','West','FIN000004'),
('BR005','Ahmedabad Branch','Ahmedabad','Gujarat','West','FIN000005'),
('BR006','Lucknow Branch','Lucknow','UP','North','FIN000006'),
('BR007','Jaipur Branch','Jaipur','Rajasthan','North','FIN000007'),
('BR008','Indore Branch','Indore','MP','Central','FIN000008'),
('BR009','Chandigarh Branch','Chandigarh','Punjab','North','FIN000009'),
('BR010','Nashik Branch','Nashik','Maharashtra','West','FIN000010');

INSERT INTO Dim_Dates
(Date_Key,Full_Date,Day_Number,Month_Number,Month_Name,Quarter_Number,Year_Number,Week_Number,Is_Weekend)
VALUES
(20260101,'2026-01-01',1,1,'January',1,2026,1,'N'),
(20260102,'2026-01-02',2,1,'January',1,2026,1,'N'),
(20260103,'2026-01-03',3,1,'January',1,2026,1,'Y'),
(20260104,'2026-01-04',4,1,'January',1,2026,1,'Y'),
(20260105,'2026-01-05',5,1,'January',1,2026,2,'N'),
(20260106,'2026-01-06',6,1,'January',1,2026,2,'N'),
(20260107,'2026-01-07',7,1,'January',1,2026,2,'N'),
(20260108,'2026-01-08',8,1,'January',1,2026,2,'N'),
(20260109,'2026-01-09',9,1,'January',1,2026,2,'N'),
(20260110,'2026-01-10',10,1,'January',1,2026,2,'Y');

INSERT INTO Dim_Employees
(Employee_ID,Employee_Name,Designation,Department,Branch_Name,Hire_Date)
VALUES
('EMP001','Arun Desai','Manager','Operations','Mumbai Main','2018-01-10'),
('EMP002','Kiran Shah','Officer','Loans','Pune City','2019-02-11'),
('EMP003','Meera Patel','Cashier','Operations','Delhi Central','2020-03-12'),
('EMP004','Sanjay Rao','Relationship Manager','Sales','Nagpur Branch','2017-04-10'),
('EMP005','Nitin Gupta','Manager','Deposits','Ahmedabad Branch','2016-05-20'),
('EMP006','Ritu Singh','Officer','Loans','Lucknow Branch','2021-06-15'),
('EMP007','Deepak Jain','Cashier','Operations','Jaipur Branch','2022-07-10'),
('EMP008','Anita Roy','Manager','Cards','Indore Branch','2018-08-08'),
('EMP009','Mohit Sharma','Officer','Operations','Chandigarh Branch','2023-01-15'),
('EMP010','Sonal Kulkarni','Manager','Sales','Nashik Branch','2015-09-25');

INSERT INTO Dim_Transaction_Types
(Transaction_Code,Transaction_Name,Transaction_Category)
VALUES
('TR001','Cash Deposit','Deposit'),
('TR002','Cash Withdrawal','Withdrawal'),
('TR003','NEFT','Transfer'),
('TR004','RTGS','Transfer'),
('TR005','IMPS','Transfer'),
('TR006','UPI','Digital'),
('TR007','ATM Withdrawal','ATM'),
('TR008','Cheque Deposit','Deposit'),
('TR009','Interest Credit','Interest'),
('TR010','Service Charge','Fee');

INSERT INTO Dim_Channels
(Channel_Code,Channel_Name,Channel_Type)
VALUES
('CH001','Branch','Offline'),
('CH002','ATM','Offline'),
('CH003','Internet Banking','Online'),
('CH004','Mobile Banking','Online'),
('CH005','UPI','Digital'),
('CH006','POS','Digital'),
('CH007','Call Center','Support'),
('CH008','NEFT Portal','Online'),
('CH009','RTGS Portal','Online'),
('CH010','Self Service Kiosk','Offline');

INSERT INTO Dim_Loan_Products
(Loan_Product_Code,Loan_Product_Name,Loan_Category,Maximum_Tenure)
VALUES
('LN001','Home Loan','Housing',360),
('LN002','Car Loan','Vehicle',84),
('LN003','Personal Loan','Personal',60),
('LN004','Education Loan','Education',180),
('LN005','Business Loan','Business',120),
('LN006','Gold Loan','Secured',36),
('LN007','MSME Loan','Business',180),
('LN008','Mortgage Loan','Housing',240),
('LN009','Agriculture Loan','Agri',120),
('LN010','Consumer Loan','Retail',48);


INSERT INTO Dim_Cards
(Card_Number,Card_Type,Card_Network,Card_Category,Issue_Date,Expiry_Date,Card_Status)
VALUES
('4111111111111001','Debit','Visa','Classic','2024-01-01','2029-01-01','Active'),
('4111111111111002','Credit','Visa','Gold','2024-01-01','2029-01-01','Active'),
('5555555555551003','Credit','MasterCard','Platinum','2023-05-01','2028-05-01','Active'),
('5555555555551004','Debit','MasterCard','Classic','2023-06-01','2028-06-01','Active'),
('4111111111111005','Debit','Visa','Classic','2022-07-01','2027-07-01','Blocked'),
('4111111111111006','Credit','Visa','Gold','2022-08-01','2027-08-01','Active'),
('5555555555551007','Credit','MasterCard','Gold','2024-02-01','2029-02-01','Active'),
('4111111111111008','Debit','Visa','Classic','2024-03-01','2029-03-01','Active'),
('5555555555551009','Credit','MasterCard','Platinum','2023-09-01','2028-09-01','Active'),
('4111111111111010','Debit','Visa','Classic','2024-04-01','2029-04-01','Active');

INSERT INTO Fact_Transactions
(Customer_Key,Account_Key,Branch_Key,Date_Key,Employee_Key,Transaction_Type_Key,Channel_Key,
Transaction_Amount,Transaction_Fee,Balance_After_Transaction,Transaction_Count)
VALUES
(1,1,1,20260101,1,1,1,25000,0,125000,1),
(2,2,2,20260102,2,2,2,5000,20,45000,1),
(3,3,3,20260103,3,3,3,100000,50,350000,1),
(4,4,4,20260104,4,4,4,75000,25,98000,1),
(5,5,5,20260105,5,5,5,15000,5,89000,1),
(6,6,6,20260106,6,6,4,22000,0,150000,1),
(7,7,7,20260107,7,7,2,8000,15,65000,1),
(8,8,8,20260108,8,8,1,30000,0,120000,1),
(9,9,9,20260109,9,9,3,1200,0,212000,1),
(10,10,10,20260110,10,10,5,350,50,45000,1);

INSERT INTO Fact_Loan
(Customer_Key, Branch_Key, Employee_Key, Date_Key, Loan_Product_Key,
Loan_Amount, Interest_Rate, EMI_Amount, Outstanding_Balance, Loan_Count)
VALUES
(1,1,1,20260101,1,5000000.00,8.50,42150.00,4850000.00,1),
(2,2,2,20260102,2,800000.00,9.25,18500.00,765000.00,1),
(3,3,3,20260103,3,300000.00,12.50,6850.00,285000.00,1),
(4,4,4,20260104,4,1200000.00,10.00,12500.00,1180000.00,1),
(5,5,5,20260105,5,2500000.00,11.20,45200.00,2400000.00,1),
(6,6,6,20260106,6,400000.00,7.50,11800.00,375000.00,1),
(7,7,7,20260107,7,1800000.00,9.80,22800.00,1750000.00,1),
(8,8,8,20260108,8,3500000.00,8.90,35600.00,3400000.00,1),
(9,9,9,20260109,9,600000.00,6.75,9200.00,580000.00,1),
(10,10,10,20260110,10,150000.00,13.50,4200.00,142000.00,1);

INSERT INTO Fact_Card_Transactions
(Customer_Key, Card_Key, Branch_Key, Date_Key, Channel_Key,
Transaction_Amount, Cashback_Amount, Reward_Points, Transaction_Count)
VALUES
(1,1,1,20260101,6,2500.00,25.00,250,1),
(2,2,2,20260102,5,5000.00,50.00,500,1),
(3,3,3,20260103,3,12500.00,125.00,1250,1),
(4,4,4,20260104,6,3200.00,32.00,320,1),
(5,5,5,20260105,2,1500.00,15.00,150,1),
(6,6,6,20260106,4,8700.00,87.00,870,1),
(7,7,7,20260107,5,6400.00,64.00,640,1),
(8,8,8,20260108,6,9200.00,92.00,920,1),
(9,9,9,20260109,3,11500.00,115.00,1150,1),
(10,10,10,20260110,5,2800.00,28.00,280,1);

INSERT INTO Fact_Fixed_Deposits
(Customer_Key, Branch_Key, Employee_Key, Date_Key,
Deposit_Amount, Interest_Rate, Maturity_Amount, Deposit_Term_Months)
VALUES
(1,1,1,20260101,500000.00,6.80,604000.00,24),
(2,2,2,20260102,250000.00,6.50,283500.00,18),
(3,3,3,20260103,1000000.00,7.00,1210000.00,36),
(4,4,4,20260104,150000.00,6.25,169000.00,24),
(5,5,5,20260105,750000.00,6.90,902000.00,30),
(6,6,6,20260106,300000.00,6.40,338500.00,18),
(7,7,7,20260107,450000.00,6.75,544000.00,24),
(8,8,8,20260108,900000.00,7.10,1095000.00,36),
(9,9,9,20260109,200000.00,6.30,212800.00,12),
(10,10,10,20260110,600000.00,6.85,723000.00,24);




use fincore_db;
SELECT 
    COUNT(*) AS total_branches
FROM
    dim_branches
WHERE
    state = 'Maharashtra'
        AND region = 'west';
SELECT 
    branch_name
FROM
    dim_branches
WHERE
    branch_name LIKE 'p%';
UPDATE dim_branches 
SET 
    state = 'uttar pradesh'
WHERE
    branch_key = 6;
SELECT 
    *
FROM
    dim_branches;
SELECT 
    *
FROM
    dim_cards;

SELECT 
    *
FROM
    dim_employees;
SELECT 
    COUNT(*) AS tottal_count
FROM
    dim_employees
WHERE
    hire_date BETWEEN '2026-01-01' AND '2027-01-01';

SELECT 
    *
FROM
    fact_loans;

use acb;
