# Cocoon project
Simple cocoon responsible for managing independent applications.

### Cloning the project
In theory, it should be a simple step, being:
```
$ git clone --recurse-submodules git@github.com:zevitagemom30/consolidacao-cocoon.git
```

### Installation
There are two ways to set up your environment, with or without `docker-compose.yml` (container manager).

### Using `docker-compose.yml`
In the root of the project, run:

```
$ docker-compose up -d --build

Creating network "app-network" with the default driver
Creating network "database-network" with the default driver

Container consolidacao-postgres    ... Started
Container consolidacao-php         ... Started
Container consolidacao-mysql       ... Started
Container consolidacao-pgadmin     ... Started
Container consolidacao-phpmyadmin  ... Started
```

After that, regardless of whether you used compose or not, it will be necessary to enable `a2enmod rewrite`.
Access your HTTP server terminal and run the following commands:
```
$ docker-compose exec consolidacao-php bash
$ a2enmod rewrite
$ service apache2 restart
```

If the selected application/folder is already in the `git` history, the following message will be displayed:
```
'source/app' already exists in the index
```
	
If the above error is displayed, confirm the `staged` state of that directory with the following command:
```
$ git ls-files --stage source/app 
100644 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0	app/.gitkeep
```

Before continuing, you must "clear" this information:
```
$ git rm --cached source/app -r
rm 'source/app/.gitkeep'
```

Finally, try running the command responsible for adding a sub-module again.

To access the containers check the port that the desired container is configured in the docker-compose.yml file.
In the configured examples we have the following addresses:
http://localhost:8080
  and/or
http://localhost:9000

### Technologies and Libraries
- [Docker] - https://www.docker.com
- [Docker Compose] - https://docs.docker.com/compose
- [Apache] - https://www.apache.org
- [Git] - https://git-scm.com
- [PgAdmin] - https://www.pgadmin.org

---

From here on down, there will be shortcut commands for quick reference ...

### Connect to Postgres database via terminal:
`psql -U postgres -h localhost`

### Choose Postgres database:
`\c <database_name>`

### List Postgres tables (after choosing the database):
`\dt`

### Disable pagination in Postgres to display all records:
`\pset pager off`

### Running a query inside Postgres:
`SELECT * FROM <table>;`
