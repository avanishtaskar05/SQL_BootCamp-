--What is DBMS?--
DBMS (Database Management System) is software that helps users create, store, organize, retrieve, and manage data in a database efficiently.

--Difference between DBMS and RDBMS?--
 Feature             | DBMS                                                 | RDBMS                                                    
 ------------------- | ---------------------------------------------------- | --------------------------------------------------------
 **Full Form**       | Database Management System                           | Relational Database Management System                    
 **Data Storage**    | Stores data as files or simple structures            | Stores data in tables (rows and columns)                 
 **Relationships**   | Does not strictly support relationships between data | Supports relationships using primary and foreign keys    
 **Normalization**   | Limited or no normalization                          | Supports normalization to reduce redundancy              
 **Data Redundancy** | Higher redundancy                                    | Lower redundancy                                         
 **Security**        | Basic security features                              | Advanced security and access control                     
 **Users**           | Usually suitable for single-user applications        | Suitable for multi-user applications                    
 **Data Integrity**  | Less emphasis on integrity constraints               | Enforces integrity constraints                           
 **Scalability**     | Less scalable                                        | Highly scalable                                          
 **Examples**        | dBase, FoxPro                                        | MySQL, Oracle Database, Microsoft SQL Server, PostgreSQL 

--Principles to Design a Database--
1. Understand Business Requirements
   → Identify entities, attributes, and relationships clearly
   → Know what data to store and why

2. Identify Proper Entities (Tables)
   → One table = one real-world object (Student, Course, Order, etc.)
   → Avoid mixing multiple concepts in one table

3. Choose Correct Datatypes
   → INT for IDs, VARCHAR for text, DATE/TIMESTAMP for dates
   → Avoid using TEXT when VARCHAR is sufficient

4. Define Primary Keys
   → Every table must have a PRIMARY KEY
   → Use surrogate keys (id) where natural keys are complex

5. Normalize the Database
   → Remove redundant data
   → Follow 1NF, 2NF, and 3NF
   → Split repeating or dependent columns into separate tables

6. Plan Relationships
   → Identify One-to-One, One-to-Many, Many-to-Many relationships
   → Use FOREIGN KEYS to maintain data integrity

7. Avoid Redundancy
   → Do not duplicate columns across tables unnecessarily
   → Store data only once and reference using keys

8. Use Meaningful Naming Conventions
   → Table names: plural nouns (Students, Orders)
   → Column names: snake_case and self-explanatory
   → Avoid reserved keywords

9. Handle NULL Values Carefully
   → Allow NULL only when data is truly optional
   → Use NOT NULL where data is mandatory

10. Think About Scalability & Performance
    → Index columns used in JOIN, WHERE, GROUP BY
    → Avoid over-indexing

11. Security & Data Sensitivity
    → Separate sensitive data (passwords, PII)
    → Use proper access control and encryption where required

12. Plan for Future Changes
    → Design flexible schema
    → Avoid hard-coded or derived data storage
