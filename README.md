# Scaffolding Master project
Scaffolding Master is responsible for managing independent applications.

### Cloning the project
It should be a simple step, being:
```
$ git clone git@github.com:zevitagemom30/<container_name>-cocoon.git
```
### After clone the project, you will get the folder structure:
File tree :
```
├── images
│   ├── mysql
│   │   └── Dockerfile
│   ├── php
│   │   └── Dockerfile
│   ├── phpmyadmin
│   │   └── Dockerfile
│   └── wiki
├── source
│   ├── test-app
|   |   ├── public
|   |   |   └── index.html
|   |   ├── docker-compose.yml
│   │   └── .dockerignore
├── templates
|   ├── apache
|   |   └── default.conf
├── .gitignore
├── .gitmodules
├── README.md
└── run.sh
```


### Installation
```
$ chmod +x ./run.sh
$ ./run.sh
$ Selecione as aplicações a serem instaladas:
```
At this moment, all applications contained within the SOURCE folder will be displayed.
Select with the number referring to the application you want to use.
#### IMPORTANT:
You need the docker-compose.yml file inside the folder for the desired application. 
We provide a compact example of this file in test-app.
After selecting the application number, you will be asked which Docker action you want to use `(Build, Up, Down, Restar)`
```
$ Aplicação selecionada: source/test-app
$ Executando source/test-app/docker-compose.yml...
$ Selecione uma ação: (build/up/down/restart) 
```
### Running `docker-compose.yml`

After selecting the type of Docker command, the selected application will be executed.

``` 
Creating network "app-network" with the default driver
Creating network "database-network" with the default driver

Container <container_name>-postgres    ... Started
Container <container_name>-php         ... Started
Container <container_name>-mysql       ... Started
Container <container_name>-pgadmin     ... Started
Container <container_name>-phpmyadmin  ... Started
```

Access your HTTP server terminal and run the following commands:
```
$ docker-compose exec <container_name> bash
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
