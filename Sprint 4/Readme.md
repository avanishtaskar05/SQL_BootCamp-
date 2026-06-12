**Fincore Database**

create database if not exists fincore_db;

use fincore_db;


create table fact_transactions (

transaction_id int primary key unique,

account_id int,

transaction_type enum("Withdraw","Loan","Deposit","New account","Close account","Check deposit"),

transaction_amount bigint);


create table dim_customers (

customer_id int primary key unique,

customer_name varchar(20) not null,

customer_age int check(customer_age between 18 and 100),

customer_gender enum("Male","Female","Other") default "Male",

cusromer_city varchar(20)not null,

customer_emailid varchar(30),

customer_phoneno bigint not null,

transaction_id int,

foreign key (transaction_id) references fact_transactions(transaction_id));


create table dim_accounts (

account_id int primary key unique,

customer_id int,

foreign key(customer_id) references dim_customers(customer_id),

account_type enum("Saving account","Recurrent account","Fixed Deppsit account","Pensioner account") default "Saving account",

transaction_id int,

foreign key (transaction_id) references fact_transactions(transaction_id));


create table dim_saving_account (

account_id int,

foreign key(account_id) references dim_accounts(account_id),

customer_id int,

foreign key(customer_id) references dim_customers(customer_id));


create table dim_recurrent_account (

account_id int,

foreign key(account_id) references dim_accounts(account_id),

customer_id int,

foreign key(customer_id) references dim_customers(customer_id));


create table dim_fixed_deposit_account (

account_id int,

foreign key(account_id) references dim_accounts(account_id),

customer_id int,

foreign key(customer_id) references dim_customers(customer_id));


create table dim_pensioner_account (

account_id int,

foreign key(account_id) references dim_accounts(account_id),

customer_id int,

foreign key(customer_id) references dim_customers(customer_id));


create table dim_services (

account_id int,

foreign key(account_id) references dim_accounts(account_id),

customer_id int,

foreign key(customer_id) references dim_customers(customer_id),

transaction_id int,

foreign key (transaction_id) references fact_transactions(transaction_id),

service_type enum("Withdraw","Loan","Deposit","New account","Close account","Check deposit"));


create table dim_employee(

employee_id int primary key unique,

employee_name varchar(20),

employee_department_type enum("HR","IT","Strategic Planning","General Banking","Inspection","Risk Management","Cash & Treasury Managemnt") default "IT",

employee_city varchar(20),

employee_salary double precision,

employee_emailid varchar(30),

employee_phoneno bigint not null,

transaction_id int,

foreign key (transaction_id) references fact_transactions(transaction_id));
