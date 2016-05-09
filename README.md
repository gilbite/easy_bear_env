### What's this?

Easy environment for BEAR.Sunday using docker.

Providing:
  - php 5.6 with composer
  - MySQL 5.6

### Requirements

* docker
* docker-compose(>=1.6)
* docker-machine(>=0.6)
* Virtualbox
* bash

disclaimer: I don't know this works in Windows.


### Usage

* Define environment variables
  * `BEAR_DOCKER_PROJECT_SRC`: the path to bear project dir
  * `BEAR_DOCKER_COMPOSER_CACHE`: composer cache dir
  * `BEAR_DOCKER_WEB_PORT`: http port for the project where php built-in web server listens(var/www/index.php)
* Add `bin` dir to `$PATH` environment variable  
* Use services
  * Before using... keep in mind followings.
    * Any of annoying docker procedures are hided in  `bear_docker_run` command.
    * The very first `bear_docker_run` will take time to execute with building docker images etc
  * Shell commands
    * Just prepend `bear_docker_run` to the command you want to execute.
    * Examples:
      * `bear_docker_run composer install`
      * `bear_docker_run php bootstrap/api.php get "/todo?id=1"`
      * or `bear_docker_run bash` starts interactive shell
    * `bear_docker_run` command always works right under `$BEAR_DOCKER_PROJECT_SRC`. This means ...
      * You cannot use your local machine's absolute path.
      * You have to use relative path based on `$BEAR_DOCKER_PROJECT_SRC`.
  * MySQL
    * data source infos.
      * user: `bear`
      * password: `koriym`
      * db name: `bear_sunday`
      * port: 3306
      * host:
        * from container: `db`
        * from local: `$(docker-machine ip bear)`
    * Examples:
      * [local] `mysql -u bear -pkoriym -h $(docker-machine ip bear) bear_sunday`
      * [container]
      ```php
      $con = Doctrine\DBAL\DriverManager::getConnection([
          'driver'   => 'pdo_mysql',
          'user'     => 'bear',
          'host'     => 'db',
          'password' => 'koriym',
          'dbname'   => 'bear_sunday',
      ]);
      ```
  * Web server
    * After you execute any command via `bear_docker_run`, the web server is already servicing. Nothing to do.
    * `open http://$(docker-machine ip bear):$BEAR_DOCKER_WEB_PORT/` will open the browser(mac)
