1. log in as root user: `mysql -u root -p`
2. create new user and grant all permissions: `GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';`
3. exit shell: `exit()`
4. log in as new user: `mysql -u username -p`
5. 
```
CREATE DATABASE dbname CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE dbname;
```
