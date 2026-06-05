--What are the datatypes used for the database?--
1. Numerical datatypes
  MySQL supports various numeric data types that are used to store numerical values. These include exact numeric types such as INTEGER (INT), SMALLINT, DECIMAL, and NUMERIC, which store values with complete precision, and approximate numeric types such as FLOAT, REAL, and DOUBLE PRECISION, which may involve some rounding. In MySQL, INT is the same as INTEGER, while DEC and FIXED are synonyms for DECIMAL. Similarly, DOUBLE is treated as DOUBLE PRECISION, and REAL is also treated as DOUBLE PRECISION unless the REAL_AS_FLOAT SQL mode is enabled. These data types allow users to store both whole numbers and decimal values efficiently based on their precision requirements.

2. Date, DateTime & TimeStamp
  The date and time data types for representing temporal values are DATE, TIME, DATETIME, TIMESTAMP, and YEAR. Each temporal type has a range of valid values, as well as a “zero” value that may be used when you specify an invalid value that MySQL cannot represent. The TIMESTAMP and DATETIME types have special automatic updating behavior. TimeStamp refers to a range of dates.

3. String datatypes
  The CHAR and VARCHAR types are declared with a length that indicates the maximum number of characters you want to store. For example, CHAR(30) can hold up to 30 characters.

4. VAR & VARCHAR types
  The BINARY and VARBINARY data types are distinct from the CHAR BINARY and VARCHAR BINARY data types. For the latter types, the BINARY attribute does not cause the column to be treated as a binary string column.

5. BLOB & TEXT types
   The four BLOB types are TINYBLOB, BLOB, MEDIUMBLOB, and LONGBLOB. These differ only in the maximum length of the values they can hold. The four TEXT types are TINYTEXT, TEXT, MEDIUMTEXT, and LONGTEXT.
   BLOB values are treated as binary strings (byte strings). They have the binary character set and collation, and comparison and sorting are based on the numeric values of the bytes in column values. TEXT values are treated as nonbinary strings (character strings). They have a character set other than binary, and values are sorted and compared based on the collation of the character set.

--OLTP--
OLTP is designed to manage day-to-day business transactions such as banking transactions, online shopping orders, and ATM operations. It handles a large number of short, fast transactions involving insert, update, and delete operations. OLTP databases are highly normalized to reduce data redundancy and ensure data accuracy.

--OLAP--
OLAP, on the other hand, is designed for data analysis and decision-making. It is used in data warehouses to analyze large volumes of historical data, generate reports, identify trends, and support business intelligence. OLAP systems typically use denormalized structures such as star or snowflake schemas to improve query performance.
