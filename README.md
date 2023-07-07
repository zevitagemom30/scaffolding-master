# Project
Scaffolding Master is responsible for managing independent applications.

## Cloning the project
Cloning the project is a simple step. Just run the following command:
```
$ git clone https://github.com/zevitagemom30/scaffolding-master.git
```

## After cloning the project, you will have the following folder structure:
File tree:
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

## Adding sub-projects
The `source` folder is responsible for storing all the projects that this structure will handle. As this repository is not responsible for knowing the code of the projects to be cloned, we have chosen to follow the "sub-modules" approach for adding them. Here's an example of how to add a sub-module:

```
$ cd source
$ git submodule add <url>
```

## Installation
Make sure you are using a Windows or Linux terminal because there may be slight differences in the commands presented below, depending on the scenario:

```
$ chmod +x ./run.sh
$ ./run.sh
```

At this point, all applications contained within the `source` folder will be displayed. Choose the number corresponding to the application you want to use:

```
Select the applications to install:
1) source/test-app
#? 1
```

### IMPORTANT:
You need to have the `docker-compose.yml` file inside the folder for the desired application. We provide a concise example of this file in `test-app`.

After selecting the application number, you will be asked which Docker action you want to perform `(build, up, down, restart)`:
```
$ Selected Application: source/test-app
$ Running source/test-app/docker-compose.yml...
$ Select an action: (build/up/down/restart) 
```

After selecting the command, for example, `up`, the result should be similar to the following:
``` 
Creating network "app-network" with the default driver
Creating network "database-network" with the default driver

Container <container_name>-postgres    ... Started
Container <container_name>-php         ... Started
Container <container_name>-mysql       ... Started
Container <container_name>-pgadmin     ... Started
Container <container_name>-phpmyadmin  ... Started
```

If you choose, for example, `down/up`, something like this should be displayed:

```
Executing 'docker-compose down' command...
Container test-phpmyadmin  Stopping
Container test-app  Stopping
Container test-phpmyadmin  Stopped
Container test-phpmyadmin  Removing
Container test-phpmyadmin  Removed
Container test-app  Stopped
Container test-app  Removing
Container test-app  Removed
Container test-mysql  Stopping
Container test-mysql  Stopped
Container test-mysql  Removing
Container test-mysql  Removed
Network test-app_test-app-network  Removing
Network test-app_test-database-network  Removing
Network test-app_test-app-network  Removed
Network test-app_test-database-network  Removed
```

## How to use
To access the containers/services, check the port to which the desired container is configured in the `docker-compose.yml` file. In the provided examples, we have the following addresses:
- APP: http://localhost:8080
- PHPMYADMIN: http://localhost:3305
- MYSQL: http://localhost:3306

## Comments
1 - If you choose to use Apache in any of your sub-modules, it may be necessary to enable the rewriting by using a2enmod. Here are some commands to help you with that:

Access your HTTP server terminal and run the following commands:
```
$ docker-compose exec <container_name> bash
$ a2enmod rewrite
$ service apache2 restart
```

2 - If the selected application/folder is already in the `git` history, the following message will be displayed:
```
'source/app' already exists in the index
```
	
If you see the above error, confirm the "staged" state of that directory with the following command:
```
$ git ls-files --stage source/app 
100644 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0	app/.gitkeep 
```

Before proceeding, you need to "clear" this information:
```
$ git rm --cached source/app -r
rm 'source/app/.gitkeep'
```

Finally, try running the command responsible for adding a sub-module again.

3 - If you want to use the Apache configuration template file `templates/apache/default.conf` configured on port **:80**, you will likely need to ignore the default Apache file `000-default.conf` to consider your modifications.

The first step is to access the server where Apache is running, and then execute the following commands:
```
$ docker exec -it test-app bash
$ cd /etc/apache2/sites-enabled
```

If you list the files in the folder, you will see something like this:
```
$ ls
000-default.conf  default.conf
```

Since both files are listening on the same port **:80**, the first file read will override any rules declared later. Therefore, it is correct to have only one file using a specific port.

So, either you change the port in one of the files or rename one of the files to an extension other than `.conf`, as the `apache2.conf` file is responsible for reading any file with that extension. For example:
`IncludeOptional sites-enabled/*.conf`

```
$ mv 000-default.conf 000-default.conf.old
```

After performing the above steps and ensuring that we have only one `.conf` file pointing to **:80**, simply restart the Apache server and reload your page.

```
service apache2 restart
```

### Technologies and Libraries
- [Docker] - https://www.docker.com
- [Docker Compose] - https://docs.docker.com/compose
- [Apache] - https://www.apache.org
- [Git] - https://git-scm.com
