# MySQL Notes

Created: September 12, 2024 1:48 PM
Labels & Language: Database, SQL

## Description

MySQL is a widely used open-source relational database management system that provides an efficient and scalable way to store and manage data. 

Having been taught conclusively in school, reviewing MySQL can still be a challenging but rewarding experience.

Understanding the basics of MySQL, such as the installation process, data types, querying data, and optimizing database performance, is crucial for developing robust and scalable applications.

## MySQL Notes

### Insert basics

Here’s how to insert rows using SQL scripts.

```sql
INSERT INTO `mysite`.`user` (`user_id`, `user_account`, `user_password`, `user_auth_level`) VALUES ('1', 'admin', '******', '1');
INSERT INTO `mysite`.`user` (`user_id`, `user_account`, `user_password`, `user_auth_level`) VALUES ('2', 'manager1', '******1', '2');
INSERT INTO `mysite`.`user` (`user_id`, `user_account`, `user_password`, `user_auth_level`) VALUES ('3', 'manager2', '******2', '2');
INSERT INTO `mysite`.`user` (`user_id`, `user_account`, `user_password`, `user_auth_level`) VALUES ('4', 'manager3', '******3', '2');
```

![Untitled](MySQL%20Notes%2017f6bc05752849828af1b03d005075e6/Untitled.png)

## Other sources & Reference