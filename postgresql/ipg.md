# in the name of ALLAH


# Install & First Login

```
# sudo -i postgres
$ psql
```
# Create User

# Create Database, User, Grant

- to grant privileges to users: "GRANT ALL PRIVILEGES ON DATABASE <database> TO <username>;"
  * mysql: "GRANT ALL PRIVILEGES ON <database>.* ON <username>@<localhost>;"

- to change a password of an exiting user: "ALTER ROLE <username> WITH PASSWORD '<new-password>';"
  * mysql: ?

- to grant access to create databases: "ALTER USER <username> CREATEDB;"
  * mysql: ?

- creata user that can create databases: "> CREATE ROLE username WITH CREATEDB LOGIN PASSWORD '<password'>;"
  * mysql: "CREATE USER <username> IDENTIFIED BY '<password>';"


# Commands of PGSQL-SHELL:

- to show databases: "> \l" or "> \list"

- to change database: "> \c"

- to show tables in the public schema: "> \dt" or "> \dt public.*"
  * trick: "> SELECT * FROM information_schema.tables WHERE table_schema = 'public';"
  or
  * trick: "> select * from pg_tables where schemaname='public';"
  
- to show all tables: "> \dt *.*" or in pacticular schema: "> \dt <name>.*"

- to describe a table strcuture: "> \d+ <table-name>"

- to show all indexes of the current schema: "> \di" and in all schemas "> \di *.*"

- to quit: "> \q"