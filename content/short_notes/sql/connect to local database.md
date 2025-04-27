#### **Connect to the local database**
```sh
sudo mysql -u root
```
#####  Check users
```sql
SELECT user, host FROM mysql.user;
```

#### Create user
```sql
CREATE USER 'user'@'localhost' IDENTIFIED BY 'pasword123';
```

#### Grant privileges to the user
```sql
GRANT ALL PRIVILEGES ON csrf_lab.* TO 'csrf_user'@'localhost';
FLUSH PRIVILEGES;
```
#### Log as user
```sh
sudo mysql --host=localhost --user=user --password=pasword123
```
#### Update table
```sql
ALTER TABLE users ADD COLUMN cookie VARCHAR(255) NULL;
```
