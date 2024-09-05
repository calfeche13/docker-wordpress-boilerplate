# WordPress Docker development setup

Docker setup for wordpress development environment using docker-compose.

## How to use

1. Clone this repository
```sh
# via HTTPS
git clone https://github.com/calfeche13/docker-wordpress-boilerplate.git

# via SSH
git clone git@github.com:calfeche13/docker-wordpress-boilerplate.git

# via GitHub CLI
gh repo clone calfeche13/docker-wordpress-boilerplate
```

2. Update remote repository
```sh
git remote set-url origin <remote_repo_url>
```

3. Installation (If you are starting fresh I suggest go to step 4)
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

4. Adjust the docker-compose.yml file if needed
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

5. Run docker-compose

**_Note:_** If you skipped step 3, running below will generate the wordpress folder.
```sh
# run docker compose in detach mode
docker-compose up -d

# run docker compose without detach mode
docker-compose up
```