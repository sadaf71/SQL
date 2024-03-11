- list all books borrowed by a specific member:

      SELECT BOOKS.TITLE , MEMBERS.MEMBER_NAME FROM BOOKS JOIN MEMBERS ON MEMBERS.MEMBER_ID=BOOKS.BOOK_ID;
- Find the most popular genres:

       SELECT MAX(GENRE) FROM BOOKS ;
- Identify books with the highest average rating:

        SELECT BOOKS.TITLE,AVG(REVIEWS.RATING) FROM BOOKS JOIN REVIEWS ON BOOKS.BOOK_ID=REVIEWS.REVIEW_ID GROUP BY BOOKS.TITLE;
      
- List all members who have borrowed more than 5 books:

      SELECT BOOKS.TITLE,MEMBERS.MEMBER_NAME ,TRANSACTIONS.TRANSACTION_TYPE 
        FROM BOOKS JOIN MEMBERS ON BOOKS.BOOK_ID=MEMBERS.MEMBER_ID JOIN TRANSACTIONS
         ON TRANSACTIONS.TRANSACTION_ID= BOOKS.BOOK_ID WHERE TRANSACTION_TYPE='BORROW' AND MEMBERS.MEMBER_NAME>=5; 

  
- List all members who have borrowed less than 5 books:

      SELECT BOOKS.TITLE,MEMBERS.MEMBER_NAME ,TRANSACTIONS.TRANSACTION_TYPE 
        FROM BOOKS JOIN MEMBERS ON BOOKS.BOOK_ID=MEMBERS.MEMBER_ID JOIN TRANSACTIONS
         ON TRANSACTIONS.TRANSACTION_ID= BOOKS.BOOK_ID WHERE TRANSACTION_TYPE='BORROW' AND MEMBERS.MEMBER_NAME<=5; 
- Retrieve the top-rated books with at least 5 reviews:

       SELECT BOOKS.TITLE,MAX(REVIEWS.RATING) FROM BOOKS JOIN REVIEWS ON 
       BOOKS.BOOK_ID=REVIEWS.REVIEW_ID GROUP BY BOOKS.TITLE HAVING COUNT(REVIEWS.REVIEW_TEXT)<=5;
  
- Calculate the total revenue generated from book purchases:

       SELECT SUM(AMOUNT_PAID) FROM TRANSACTIONS WHERE TRANSACTION_TYPE='PURCHASE';

- List all books with their respective authors and publishers:

          SELECT BOOKS.TITLE ,AUTHORS.AUTHOR_NAME,PUBLISHERS.PUBLISHER_NAME FROM BOOKS JOIN AUTHORS 
        ON AUTHORS.AUTHOR_ID=BOOKS.BOOK_ID JOIN PUBLISHERS ON PUBLISHERS.PUBLISHER_ID=BOOKS.BOOK_ID;
- Find books that are currently available for borrowing:

      SELECT BOOKS.TITLE,TRANSACTIONS.TRANSACTION_TYPE FROM BOOKS 
        JOIN TRANSACTIONS ON BOOKS.BOOK_ID=TRANSACTIONS.TRANSACTION_ID  WHERE TRANSACTION_TYPE='BORROW';
  
- Identify members who have overdue books:

      SELECT MEMBERS.MEMBER_NAME, BORROWINGS.IS_RETURNED FROM MEMBERS 
         JOIN BORROWINGS ON MEMBERS.MEMBER_ID=BORROWINGS.BORROWING_ID WHERE IS_RETURNED='FALSE';
  
- List the top 10 most borrowed books:

      SELECT BOOKS.TITLE,TRANSACTIONS.TRANSACTION_TYPE FROM BOOKS 
        JOIN TRANSACTIONS ON BOOKS.BOOK_ID=TRANSACTIONS.TRANSACTION_ID  WHERE TRANSACTION_TYPE='BORROW'  LIMIT 10;
  
- Calculate the average number of days a book is borrowed for:
      select title,
     avg(datediff(day,borrowing_date,return_date)) average_duration
     from borrowings
     join Books on Books.Book_id=Borrowings.Borrowing_Id;
- Find the total number of books published in each year:

       SELECT BOOKS.TITLE,publication_date FROM BOOKS ;

- Identify members who have borrowed books more than once:
      select members.member_name,book_copies.copy_number from members join
         book_copies on book_copies.copy_id=members.member_id where book_copies.copy_number>1;
  
- List all books with their respective authors and average ratings:

      select books.title,authors.author_name,avg(reviews.rating) from books join
         authors on authors.author_id=books.book_id join reviews on books.book_id=reviews.review_id 
         group by books.book_id;
- Calculate the total number of copies available for each book:

      select title, quantity_available from books;
- Create a view of transaction table and provide privilege to another user. The user can view only member id and transaction date and privilege should be to select id who made transaction on any specific date.

      CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `db`.`trans` AS
    SELECT 
        `db`.`transactions`.`member_id` AS `member_id`,
        `db`.`transactions`.`transaction_date` AS `transaction_date`
    FROM
        `db`.`transactions`

  select * from trans;

