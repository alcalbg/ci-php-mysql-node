# Bitbucket Pipelines Docker image based on andmetoo/bitbucket-pipelines-php7

### Packages installed by andmetoo/bitbucket-pipelines-php7

- Ubuntu 16.04
- `php7.0-fpm`, `php7.0-mcrypt`, `mongod`, `xdebug`, `php7.0-zip`, `php7.0-xml`, `php7.0-mbstring`, `php7.0-curl`, `php7.0-json`, `php7.0-imap`, `php7.0-mysql` and `php7.0-tokenizer`
- [Composer](https://getcomposer.org/)
- [Deployer](https://github.com/deployphp/deployer)
- Node / NPM / Gulp / Yarn
- Mysql 5.7

### Packages added for Cypress.io (dependencies)

- libgtk2.0-0
- libnotify-dev
- libgconf-2-4
- libnss3
- libxss1
- libasound2

### Sample `bitbucket-pipelines.yml`

```YAML
image: alcalbg/ci-php-mysql-node
pipelines:
  default:
    caches:
      - composer
      - node
      - cypress-bin
    - step:
        script:
          - service mysql start
          - mysql -h localhost -u root -proot -e "CREATE DATABASE test;"
          - composer install --no-interaction --no-progress --prefer-dist
          - npm install --no-spin
          - gulp
          - ./vendor/phpunit/phpunit/phpunit -v --coverage-text --colors=never --stderr
          - npm run tests

definitions:
  caches:
    cypress-bin: $HOME/.cache/Cypress
```

