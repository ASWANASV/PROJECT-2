                                        LIBRARY MANAGEMENT SYSTEM

    This project is based on Library Management System. it keeps track of information about books in the library,
    their cost,status and total number of books available in the library.
    I created a database named Library and following tables in the database:
              1. Branch 
              2. Employee 
              3. Books
              4. Customer
              5. IssueStatus
              6. ReturnStatus 
    Attributes for each tables and their queries i used are ;
          1. Branch
               * Branch_no - Set as PRIMARY KEY  
               * Manager_Id  
               * Branch_address  
               * Contact_no 
         => create table BRANCH(BRANCH_NO INT primary key,
                       MANAGER_ID INT,
                       BRANCH_ADDRESS varchar(200),
                       CONTACT_NO bigint);
         2. Employee  
               * Emp_Id – Set as PRIMARY KEY  
               * Emp_name  
               * Position  
               * Salary
               * Branch_no - Set as FOREIGN KEY and it refer Branch_no in Branch table  
        => create table EMPLOYEE(EMP_ID INT primary key,
                        EMP_NAME varchar(50),
                        POSITION varchar(50),
                        SALARY INT,BRANCH_NO INT,
                        foreign key(BRANCH_NO)references BRANCH(BRANCH_NO));
         3. Books  
               * ISBN - Set as PRIMARY KEY  
               * Book_title  
               * Category  
               * Rental_Price  
               * Status [Give yes if book available and no if book not available]  
               * Author  
               * Publisher
        => create table BOOKS(ISBN varchar(20) primary key,
                        BOOK_TITLE varchar(100),
                        CATEGORY varchar(40),
                        RENTAL_PRICE INT,
                        STATUS varchar(20),
                        AUTHOR varchar(50),
                        PUBLISHER varchar(50));
        4. Customer  
               * Customer_Id - Set as PRIMARY KEY  
               * Customer_name  
               * Customer_address  
               * Reg_date 
        => create table CUSTOMER(CUST_ID INT primary key,
                        CUST_NAME varchar(100),
                        CUST_ADDRESS varchar(200),
                        REG_DATE DATE);
        5. IssueStatus  
               * Issue_Id - Set as PRIMARY KEY  
               * Issued_cust – Set as FOREIGN KEY and it refer customer_id in CUSTOMER table  Issued_book_name 
               * Issue_date 
               * Isbn_book – Set as FOREIGN KEY and it should refer isbn in BOOKS table 
        => create table ISSUESTATUS(ISSUE_ID INT primary key,
                        ISSUED_CUST INT,
                        ISSUED_BOOK_NAME varchar(100),
                        ISSUE_DATE DATE,
                        ISBN_BOOK varchar(20),
                        foreign key(ISSUED_CUST) references CUSTOMER(CUST_ID),
                        foreign key(ISBN_BOOK)references BOOKS(ISBN));
        6. ReturnStatus  
               * Return_Id - Set as PRIMARY KEY  
               * Return_cust  
               * Return_book_name  
               * Return_date  
               * Isbn_book2 - Set as FOREIGN KEY and it should refer isbn in BOOKS table 
        => create table RETURNSTATUS(RETURN_ID INT primary key,
                        RETURN_CUST INT,
                        RETURN_BOOK_NAME VARCHAR(100),
                        RETURN_DATE DATE,
                        ISBN_BOOK2 varchar(20),
                        foreign key(RETURN_CUST)references CUSTOMER(CUST_ID),
                        foreign key(ISBN_BOOK2)references BOOKS(ISBN));
Then i inserted a minimum of 10 rows for each table.And displayed all the tables.
    ## The queries that i used here are
           1. Retrieve the book title, category, and rental price of all available books. 
                    =>  select BOOK_TITLE,CATEGORY,RENTAL_PRICE FROM BOOKS where STATUS='YES';
           2. List the employee names and their respective salaries in descending order of salary.
                    =>  select EMP_NAME,SALARY FROM EMPLOYEE order by SALARY DESC;
           3. Retrieve the book titles and the corresponding customers who have issued those books. 
                    =>  select BOOKS.BOOK_TITLE,CUSTOMER.CUST_NAME FROM BOOKS 
                        JOIN ISSUESTATUS ON BOOKS.ISBN=ISSUESTATUS.ISSUED_BOOK_NAME
                        JOIN CUSTOMER ON ISSUESTATUS.ISSUED_CUST=CUSTOMER.CUST_ID;
           4. Display the total count of books in each category. 
                   =>  select CATEGORY,COUNT(*)AS TOTAL_BOOKS FROM BOOKS group by CATEGORY;
           5. Retrieve the employee names and their positions for the employees whose salaries are above Rs.50,000. 
                   =>  select EMP_NAME,POSITION from EMPLOYEE where SALARY >50000;
           6. List the customer names who registered before 2022-01-01 and have not issued any books yet. 
                   =>  select CUST_NAME from CUSTOMER where REG_DATE AND CUST_ID NOT IN
                       (SELECT ISSUED_CUST from ISSUESTATUS);
           7. Display the branch numbers and the total count of employees in each branch. 
                   =>  select BRANCH_NO,COUNT(*) AS TOTAL_EMPLOYEES from EMPLOYEE group by BRANCH_NO;
           8. Display the names of customers who have issued books in the month of January 2020.
                   =>  select distinct CUST_NAME from CUSTOMER 
                       JOIN ISSUESTATUS ON CUSTOMER.CUST_ID=ISSUESTATUS.ISSUED_CUST where
                       ISSUESTATUS.ISSUE_DATE >='2020-01-01' AND ISSUESTATUS.ISSUE_DATE <'2020-02-01';
           9. Retrieve book_title from book table containing fiction. 
                   =>  select BOOK_TITLE FROM BOOKS where CATEGORY='FICTION';
           10.Retrieve the branch numbers along with the count of employees for branches having more than 2 employees
                   =>  select BRANCH_NO,COUNT(*) AS EMPLOYEE_COUNT from EMPLOYEE group by BRANCH_NO having count(*)>2;
           11. Retrieve the names of employees who manage branches and their respective branch addresses.
                   =>  select EMPLOYEE.EMP_NAME,BRANCH.BRANCH_ADDRESS from EMPLOYEE 
                       JOIN BRANCH ON EMPLOYEE.EMP_ID=BRANCH.MANAGER_ID;
           12.  Display the names of customers who have issued books with a rental price higher than Rs. 10.
                   =>  select distinct CUST_NAME from CUSTOMER
                       JOIN ISSUESTATUS ON CUSTOMER.CUST_ID=ISSUESTATUS.ISSUED_CUST
                       JOIN BOOKS ON ISSUESTATUS.ISSUED_BOOK_NAME=BOOKS.ISBN where BOOKS.RENTAL_PRICE >10;
          
