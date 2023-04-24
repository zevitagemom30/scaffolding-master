# Cocoon project
Simple cocoon responsible for managing independent applications.

## Cloning the project
In theory, it should be a simple step, being:
```
$ git clone --recurse-submodules git@github.com:zevitagemom30/consolidacao-cocoon.git
```

## Installation
There are two ways to set up your environment, with or without `docker-compose.yml` (container manager).

### Using `docker-compose.yml`
In the root of the project, run:

```
$ docker-compose up -d --build

Creating network "app-network" with the default driver
Creating network "database-network" with the default driver

Creating mysql      ... done
Creating phpmyadmin ... done
Creating php        ... done
```

### Manually without `docker-compose.yml`
Still in the root of the project, follow these steps:

Creating the images based on the `Dockerfile` files located in images/`<local>`:
```
$ docker build -t php-image -f images/php/Dockerfile .
$ docker build -t phpmyadmin-image -f images/phpmyadmin/Dockerfile .
$ docker build -t mysql-image -f images/mysql/Dockerfile .
```

Creation of networks for communication between containers:
```
$ docker network create --driver=bridge database-network
$ docker network create --driver=bridge app-network
```

Up `MYSQL SERVER` as the first container, since the others depend on it for connection:
```
$ docker run -d --name mysql mysql-image
```

Add specific networks to `MYSQL SERVER`:
```
$ docker network connect app-network mysql
$ docker network connect database-network mysql
```

Up `PHPMYADMIN SERVER` on the same network (`database`) as MYSQL and with access through port **3305**:
```
$ docker run -d \
	--name phpmyadmin \
	--network database-network -p 3305:80 \
phpmyadmin-image
```

Up `PHP SERVER` on the same network (`app`) as MYSQL and with access through port **8080**:
```
$ docker run -d --name php -p 8080:80 \
	--network app-network \
	--mount src="$(pwd)/src",target=/var/www/html,type=bind \
php-image
```

After that, regardless of whether you used compose or not, it will be necessary to enable `a2enmod rewrite`.
Access your HTTP server terminal and run the following commands:
```
$ a2enmod rewrite
$ service apache2 restart
```

## Accessing the application
All applications will be located in `source/*`, just access the path directly:

> http://localhost:8080/`<app_name>/<public_folder|public_file>`
> phpMyAdmin: http://localhost:3305/index.php

Example of an application using the `Laravel` framework:
- Login route: http://localhost:8080/app/public/login
- README: https://github.com/zevitagem/evolke-test/blob/main/README.md

## Configuring new applications
If you want to create a new application inside `source/` and make it a sub-module of `Git`, here are some commands to help you:

Access the `source/` folder, try adding:
```
$ git submodule add git@github.com:zevitagem/evolke-test.git app
```

If the selected application/folder is already in the `Git` history, the following message will be displayed:
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

## Technologies and Libraries
- [Docker] - https://www.docker.com
- [Docker Compose] - https://docs.docker.com/compose
- [Apache] - https://www.apache.org
- [Git] - https://git-scm.com
