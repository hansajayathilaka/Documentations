
install postgresql (9.3)
```
sudo apt-get install postgresql postgresql-contrib
```
    
install python-psycopg2
 ```
 sudo apt-get install python-psycopg2
 sudo apt-get install libpq-dev
 sudo pip install psycopg2==2.7.5 --ignore-installed
 ```

edit `/etc/postgresql/9.3/main/pg_hba.conf` like this.`local   all    postgres      peer` line's `peer` to `md5`    

restart postgresql
```
sudo service postgresql restart
```
    
create indivo user and the indivo database following instructions
```
sudo su - postgres
createuser --superuser indivo
psql
postgres=# \password indivo
postgres=# \q
logout
createdb -U indivo -O indivo indivo
```

## If Error Occoured when login to postrges or creating db,

* Edit `pg_hba.conf`, setting the auth mode to `trust` instead of the `md5`
* In the Services control panel restart the PostgreSQL service
* Connect with `psql` or PgAdmin or whatever
* `ALTER USER postgres PASSWORD 'mynewpassword';`
* Edit `pg_hba.conf` again and set the auth mode back to `md5`
* Restart PostgreSQL again

```console
sudo su - postgres
psql
# when asking password type mynewpassword
password indivo
# give indivo as the password
\q
createdb -U indivo -O indivo indivo
```

## If Stil Error Comes,

* Add `/etc/postgresql/9.3/main/pg_hba.conf` to `local   all    indivo      md5` after `local   all    postgres      peer` line. <br>
* Restart PostgreSQL again

```console
sudo su -postgres
createdb -U indivo -O indivo indivo
# when asking password type indivo as the password
```
