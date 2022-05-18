## JdbcUserDetailsManager
1. expects three columns in the users table: a username, a password, and enabled, which you can use to deactivate the user

```sql

CREATE TABLE IF NOT EXISTS 'spring'.'users' (
    'id' INT NOT NULL AUTO_INCREMENT,
    'username' VARCHAR(45) NOT NULL,
    'password' VARCHAR(45) NOT NULL,
    'enabled' INT NOT NULL,
    PRIMARY KEY ('id')
);

```
