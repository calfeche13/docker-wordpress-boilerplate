# WordPress Docker development setup

Docker setup for wordpress development environment using docker-compose.

## How to use

### Clone this repository
```sh
# via HTTPS
git clone https://github.com/calfeche13/docker-wordpress-boilerplate.git

# via SSH
git clone git@github.com:calfeche13/docker-wordpress-boilerplate.git

# via GitHub CLI
gh repo clone calfeche13/docker-wordpress-boilerplate
```

### Update remote repository
```sh
git remote set-url origin <remote_repo_url>
```

### Moving existing docker files (If you are starting fresh I suggest go to step 4)
- Put WordPress files inside wordpress folder, you should have a directory that looks like below.
```
<project root>
│   README.md
│   docker-compose.yml
└───wordpress
    │   wp-admin
    │   wp-content
    │   wp-includes
    │   ...
    └───index.php
```

### Adjust the docker-compose.yml file if needed
```yml
services:

  wordpress:
    depends_on:
      - db
    image: wordpress:latest # adjust wordpress version
    ports:
      - '8080:80' # adjust the exposed port accordingly

    ⋮

  php_myadmin:
    image: phpmyadmin:latest
    restart: no
    ports:
      - '8081:80' # adjust the exposed port accordingly
```

### Run docker-compose

This docker-compose has 3 profiles.
1. dev - used when developing the app on your local pc
2. debug - prod like but enables php-myadmin
3. prod - enables nginx and certbot for ssl generation

**_Note:_** If you skipped step 3, running below will generate the wordpress folder.
```sh
# run docker compose in detach mode
docker-compose up -d

# run docker compose without detach mode
docker-compose up

# run docker compose with profile dev in detached mode
docker-compose --profile dev up -d

# run docker compose with profile debug in detached mode
docker-compose --profile debug up -d

# run docker compose with profile prod in detached mode
docker-compose --profile prod up -d
```