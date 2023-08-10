## Documentation
- [Business] - (https://github.com/OM30/<link>/blob/main/docs/stories/main.md)
- [Technical] - (https://github.com/OM30/<link>/blob/main/docs/api/main.md)

## Installation
To get started, all commands must be executed at the root of the project, inside the PHP server, after cloning the repository:

```bash
composer install
composer run post-root-package-install
composer run post-create-project-cmd
npm install
```

## Configuring the database
Before proceeding, it is crucial to correctly configure the `.env` file at the root of the project to ensure a successful database connection.

After that, you will need to create the tables in the database (if you are using it locally) so that the project works as expected. Execute the following command:

```bash
php artisan migrate
```

```bash
INFO  Preparing the database.  
Creating migration table ................................... 137ms DONE

INFO  Running migrations.  
2023_05_05_125403_<migration> ................................. 139ms DONE
```

## Technologies and Libraries
This project makes use of the following technologies and libraries:
- [PHP] - https://www.php.net
- [Dependency Manager] - https://getcomposer.org
- [Laravel] - https://laravel.com/docs/9.x
- [Docker] - https://www.docker.com
- [XDebug] - https://xdebug.org
- [Git] - https://git-scm.com

## Static Analysis
Before any commit, it is highly recommended to run the following command within the PHP server to ensure code standards:

```shell
vendor/bin/phpstan analyse app
```

```
303/303 [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓] 100%
[OK] No errors                                                          
```

## Unit Tests and Integrations
To ensure the quality and integrity of the code, unit tests are essential. To run them, use the following general or unit-specific command:

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

Regarding the minimum coverage, if there is anything, a message will be displayed like:
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

At the end of the execution, output files referring to the coverage will be available, and can be located at: `/tests/Coverage`.
The `/tests/Coverage/html/index.html` file, for example, contains interesting details about the run and can be accessed via browser.

## Agreements

### Branching Pattern
To facilitate the organization of the repository, we have adopted the following branch naming pattern:

[TIPO] / [NUMERO_HISTORIA]-[BREVE_DESCRICAO_HISTORIA]

Examples:
- `feat/0003-cadastro-de-familias`
- `fix/0008-pagina-nao-carrega`

### Commit Pattern
To ensure clarity and traceability of changes made to the project, we follow the specifications described in `conventional commits`:
https://www.conventionalcommits.org/en/v1.0.0

Examples:
- `feat`: allow the provided config object to extend other configs
- `chore!`: remove support for Node 6
- `docs`: fix spelling error in CHANGELOG

### Task Flow
To ensure code quality and efficient task completion, we have established the following flow:

```
analysis + development + [tests + documentation] + code review + technical review = `DONE`
```

> Note: if there are "strange" behaviors during the "technical review" stage, the theoretical reviewers must be alerted.

For this, the respective columns were created in the task manager system, **ClickUp** in this case, containing the respective values, being:

```
open
in progress
quality tests
documentation
awaiting theoretical review
theoretical review
awaiting practical review
practical review
waiting for correction
finished
closed
```

### Git Flow
To simplify development and collaboration in the repository, we follow the git-flow workflow:
https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow

Main branches:
- `main`
- `release/<version>`
- `develop`
- `feat/<developer>`

### Test Naming Standard:
To keep tests readable and organized, we follow the following standard for naming test methods:

`test_should_<BEHAVIOR>_on_<METHOD>`

Remember that `snake_case` was used as the standard to make the description of the test suite readable in the terminal, for example:
```
PASS  Tests\Unit\Services\FamiliaServiceTest
✓ should execute with success on get with filters                  0.98s  
✓ should throw municipe is died exception on create                0.07s
```

### Other Agreements
Additionally, we have established the following agreements to maintain the quality and integrity of the project:
- At least 2 approvals are required in the `code-review` stage.
- The code must have at least 80% unit test coverage.
- At least one static analyzer must be executed, and its messages, if any, must be fixed.
