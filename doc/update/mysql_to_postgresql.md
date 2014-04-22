# Use the shell commands below to convert a MySQL GitLab database to a PostgreSQL one.

1. [Install and configure PostgreSQL](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md#5-database)
2. [Configure database.yml](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md#configure-gitlab-db-settings)
3. 

    ```
    cd /home/git/gitlab
    sudo -u git -H bundle install --without development test mysql --deployment
    sudo -u git -H bundle exec rake gitlab:env:info RAILS_ENV=production
    ```
4. 

    ```
    git clone https://github.com/lanyrd/mysql-postgresql-converter.git
    cd mysql-postgresql-converter
    mysqldump --compatible=postgresql --default-character-set=utf8 -r databasename.mysql -u root -p gitlabhq_production
    python db_converter.py databasename.mysql databasename.psql
    sudo -u git -H psql -f databasename.psql -d gitlabhq_production
    ```
5. ```service gitlab restart```
