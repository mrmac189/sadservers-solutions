https://sadservers.com/scenario/bucharest

A web application relies on the PostgreSQL 13 database present on this server. However, the connection to the database is not working. Your task is to identify and resolve the issue causing this connection failure. The application connects to a database named app1 with the user app1user and the password app1user.

Check if postgres runs:
```bash
ps -ef | grep post
```
Check error:
```bash
PGPASSWORD=app1user psql -h 127.0.0.1 -d app1 -U app1user -c '\q'
# psql: error: FATAL:  pg_hba.conf rejects connection for host "127.0.0.1", user "app1user", database "app1", SSL on
# FATAL:  pg_hba.conf rejects connection for host "127.0.0.1", user "app1user", database "app1", SSL off
```
In error tells that pg_hba.conf rejects connections. Changing this:
```bash
vim /etc/postgresql/13/main/pg_hba.conf
# local         DATABASE  USER  METHOD 
# comment deny # host   all    all        all     reject
# host   app1    app1user        all     trust
```
Restart service:
```bash
sudo systemctl restart postgresql@13-main.service
```
Checking again:
```bash
PGPASSWORD=app1user psql -h 127.0.0.1 -d app1 -U app1user -c '\q'
```