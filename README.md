Symfony Docker
==============

## Docker

- Configure host's local ip address (docker/engine/php.ini line 6):
   * native: default local network.
   * virtual(box): virtualbox local network.

## PhpStorm

### Server configuration
![PhpStorm Server configuration](docker/doc/phpstorm-server-config.gif)
- Configuration name must be the same as docker-compose.yml line 29 (PHP_IDE_CONFIG).
- path mappings:
    - / => /home/docker
    - /wev/app_dev.php => /home/docker/web/app_dev.php

### Php interpreter configuration
![PhpStorm Interpreter configuration](docker/doc/phpstorm-interpreter-config.gif)

### XDebug configuration
![PhpStorm XDebug configuration](docker/doc/phpstorm-xdebug-config.gif)
- Pass required configuration options through command line (...) unchecked. 
- Detect path mappings from deployment configuration.
- Debug port : 9090 
- "Can accept external connections" checked.

### PhpUnit configuration
![PhpStorm PhpUnit configuration](docker/doc/phpstorm-phpunit-config.gif)


## Start

- ```docker-compose up -d```