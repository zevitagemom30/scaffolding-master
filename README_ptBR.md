# Projeto
O Scaffolding Master é responsável por gerenciar aplicações independentes.

## Clonando o projeto
É um passo simples, basta executar o seguinte comando:
```
$ git clone https://github.com/zevitagemom30/scaffolding-master.git
```

## Após clonar o projeto, você terá a seguinte estrutura de pastas:
Árvore de arquivos:
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

## Adicionando subprojetos
A pasta `source` é responsável por armazenar todos os projetos que essa estrutura irá gerenciar. Como este repositório não é responsável por conhecer o código dos projetos a serem clonados, optamos por seguir a abordagem de "submódulos" para essa adição, por exemplo:

```
$ cd source
$ git submodule add <url>
```

## Instalação
Certifique-se de estar usando um terminal Windows ou Linux, pois dependendo do cenário, há pequenas diferenças nos comandos apresentados abaixo:

```
$ chmod +x ./run.sh
$ ./run.sh
```

Neste momento, todas as aplicações contidas na pasta `source` serão exibidas. Em seguida, selecione o número referente à aplicação que deseja usar.

```
Selecione as aplicações a serem instaladas:
1) source/test-app
#? 1
```

### IMPORTANTE:
Você precisa ter o arquivo `docker-compose.yml` dentro da pasta da aplicação desejada. Fornecemos um exemplo compacto desse arquivo em `test-app`.

Após selecionar o número da aplicação, você será questionado sobre qual ação Docker deseja executar `(build, up, down, restart)`:
```
$ Aplicação selecionada: source/test-app
$ Executando source/test-app/docker-compose.yml...
$ Selecione uma ação: (build/up/down/restart) 
```

Após selecionar o tipo de comando, por exemplo, `up`, o resultado deve ser semelhante ao seguinte:
``` 
Criando a rede "app-network" com o driver padrão
Criando a rede "database-network" com o driver padrão

Container <nome_do_container>-postgres    ... Iniciado
Container <nome_do_container>-php         ... Iniciado
Container <nome_do_container>-mysql       ... Iniciado
Container <nome_do_container>-pgadmin     ... Iniciado
Container <nome_do_container>-phpmyadmin  ... Iniciado
```

Se você escolher, por exemplo, `down/up`, algo parecido com isso será exibido:

```
Executando o comando 'docker-compose down'...
Container test-phpmyadmin  Parando
Container test-app  Parando


Container test-phpmyadmin  Parado
Container test-phpmyadmin  Removendo
Container test-phpmyadmin  Removido
Container test-app  Parado
Container test-app  Removendo
Container test-app  Removido
Container test-mysql  Parando
Container test-mysql  Parado
Container test-mysql  Removendo
Container test-mysql  Removido
Rede test-app_test-app-network  Removendo
Rede test-app_test-database-network  Removendo
Rede test-app_test-app-network  Removida
Rede test-app_test-database-network  Removida
```

## Como usar
Para acessar os contêineres/serviços, verifique a porta em que o contêiner desejado está configurado no arquivo `docker-compose.yml`. Nos exemplos configurados, temos os seguintes endereços:
- APP: http://localhost:8080
- PHPMYADMIN: http://localhost:3305
- MYSQL: http://localhost:3306

## Comentários
1 - Se você escolheu usar o Apache em algum dos seus submódulos, pode ser necessário habilitar a reescrita do a2enmod. Abaixo estão alguns comandos para acelerar o processo:

Acesse o terminal do seu servidor HTTP e execute os seguintes comandos:
```
$ docker-compose exec <nome_do_container> bash
$ a2enmod rewrite
$ service apache2 restart
```

2 - Se a aplicação/pasta selecionada já estiver no histórico do `git`, a seguinte mensagem será exibida:
```
'source/app' já existe no índice
```

Se o erro acima for exibido, confirme o estado "staged" daquele diretório com o seguinte comando:
```
$ git ls-files --stage source/app 
100644 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0	app/.gitkeep 
```

Antes de continuar, você deve "limpar" essa informação:
```
$ git rm --cached source/app -r
rm 'source/app/.gitkeep'
```

Por fim, tente executar novamente o comando responsável por adicionar um submódulo.

### Tecnologias e Bibliotecas
- [Docker] - https://www.docker.com
- [Docker Compose] - https://docs.docker.com/compose
- [Apache] - https://www.apache.org
- [Git] - https://git-scm.com
