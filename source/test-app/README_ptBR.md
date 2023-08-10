## Documentação
- [Negócio] - (https://github.com/OM30/<link>/blob/main/docs/stories/main.md)
- [Técnica] - (https://github.com/OM30/<link>/blob/main/docs/api/main.md)

## Instalação
Para começar, todos os comandos devem ser executados na raiz do projeto, dentro do servidor PHP, após clonar o repositório:

```bash
composer install
composer run post-root-package-install
composer run post-create-project-cmd
npm install
```

## Configurando o banco de dados
Antes de prosseguir, é crucial configurar corretamente o arquivo `.env` na raiz do projeto para que a conexão com o banco de dados seja bem-sucedida.

Após isso, será necessário criar as tabelas no banco de dados (caso esteja usando localmente) para que o projeto funcione conforme o esperado. Execute o seguinte comando:

```bash
php artisan migrate
```

```bash
INFO  Preparando o banco de dados.  
Criando tabela de migração ................................... 137ms CONCLUÍDO

INFO  Executando as migrações.  
2023_05_05_125403_<migration> ................................ 139ms CONCLUÍDO
```

## Tecnologias e Bibliotecas
Este projeto faz uso das seguintes tecnologias e bibliotecas:
- [PHP] - https://www.php.net
- [Gerenciador de Dependências] - https://getcomposer.org
- [Laravel] - https://laravel.com/docs/9.x
- [Docker] - https://www.docker.com
- [XDebug] - https://xdebug.org
- [Git] - https://git-scm.com

## Análise Estática
Antes de qualquer commit, é altamente recomendado executar o seguinte comando dentro do servidor PHP para garantir os padrões do código:

```shell
vendor/bin/phpstan analyse app
```

```
303/303 [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓] 100%
[OK] Sem erros                                                          
```

## Testes Unitários e Integrações
Para garantir a qualidade e integridade do código, os testes unitários são essenciais. Para executá-los, utilize o seguinte comando geral ou unitário respectivamente:

```
php artisan test
php artisan test --coverage --min=80.0
php artisan test --coverage --min=80.0 --testsuite Unit
php artisan test --coverage --min=80.0 --testsuite Feature

vendor/bin/phpunit --filter 'FamiliaServiceTest'
```

```
PASS  Tests\Unit\ExampleTest
✓ that true is true                                       0.16s  

PASS  Tests\Unit\Services\FamiliaServiceTest
✓ should execute with success on get with filters         0.98s  
✓ should throw municipe is died exception on create       0.07s  

PASS  Tests\Feature\ExampleTest
✓ the application returns a successful response           5.48s  

Tests:    4 passed (11 assertions)
Duration: 8.78s
```

Sobre a cobertura mínima, caso conste algo, será exibida uma mensagem como:
```
Traits/AvailabilityWithDependencie ..................... 100.0%  
Traits/PreventBehaviorsAsService ....................... 16, 27, 39..51 / 25.0%  
Validators/AbstractValidator ........................... 0.0%  
Validators/FamiliasValidator ........................... 0.0%  
Validators/MembrosValidator ............................ 0.0%  
Validators/Municipe/GetWithFiltersValidator ............ 0.0%  
─────────────────────────────────────────────────────────────
Total: 8.1 %  
FAIL  Code coverage below expected: 8.1 %. Minimum: 80.0 %.
```

No fim da execução, estarão disponíveis arquivos de saída referente a cobertura, podendo ser localizados em: `/tests/Coverage`.
O arquivo `/tests/Coverage/html/index.html`, por exemplo, contém detalhes interessantes sobre a execução e pode ser acessado via navegador. 

## Acordos

### Padrão de Branch
Para facilitar a organização do repositório, adotamos o seguinte padrão de nomenclatura de branches:

[TIPO] / [NUMERO_HISTORIA]-[BREVE_DESCRICAO_HISTORIA]

Exemplos:
- `feat/0003-cadastro-de-familias`
- `fix/0008-pagina-nao-carrega`

### Padrão de Commit
Para garantir a clareza e rastreabilidade das mudanças realizadas no projeto, seguimos as especificações descritas no `conventional commits`:
https://www.conventionalcommits.org/en/v1.0.0

Exemplos:
- `feat`: permitir que o objeto de configuração fornecido estenda outras configurações
- `chore!`: remover suporte para o Node 6
- `docs`: corrigir erro de ortografia no CHANGELOG

### Fluxo da tarefa
Para garantir a qualidade do código e a conclusão das tarefas de forma eficiente, estabelecemos o seguinte fluxo:

```
análise + desenvolvimento + [testes + documentação] + revisão de código + revisão técnica = `CONCLUÍDO`
```

> Atenção: se existirem comportamentos "estranhos" durante a etapa de "revisão técnica", os revisadores teóricos devem ser alertados.

Para isso, foram criadas as respectivas colunas no sistema gerenciador de tarefas, **ClickUp** nesse caso, contendo os respectivos valores, sendo:

```
aberta
em andamento
testes qualidade
documentação
aguardando revisão teórica
revisão teórica
aguardando revisão prática
revisão prática
aguardando correção
finalizada
fechada
```

### Git Flow
Para simplificar o desenvolvimento e a colaboração no repositório, seguiremos o fluxo do git-flow:
https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow

Ramos principais:
- `main`
- `release/<versao>`
- `develop`
- `feat/<desenvolvedor>`

### Padrão na escrita dos testes:
A fim de manter os testes legíveis e organizados, seguimos o seguinte padrão na nomenclatura dos métodos de teste:

`test_should_<BEHAVIOR>_on_<METHOD>`

Lembrando que, foi utilizado o `snake_case` como padrão para que fique legível no terminal toda a descrição da suite de testes, exemplo:
```
PASS  Tests\Unit\Services\FamiliaServiceTest
✓ should execute with success on get with filters                  0.98s  
✓ should throw municipe is died exception on create                0.07s
```

### Outros acordos
Além disso, firmamos os seguintes acordos para manter a qualidade e a integridade do projeto:
- Serão necessárias pelo menos 2 aprovações na etapa de `code-review`.
- O código deve possuir pelo menos 80% de cobertura de testes unitários.
- Ao menos um analisador estático deve ser executado e suas mensagens, caso existam, devem ser corrigidas.
