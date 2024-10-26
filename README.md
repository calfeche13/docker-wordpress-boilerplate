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
sudo docker-compose up -d

# run docker compose without detach mode
sudo docker-compose up

# run docker compose with profile dev in detached mode
sudo docker-compose --profile dev up -d

# run docker compose with profile debug in detached mode
sudo docker-compose --profile debug up -d

# run docker compose with profile prod in detached mode
sudo docker-compose --profile prod up -d
```

### Prod create cert with Certbot
To check if certbot will be able to create a cert run --dry-run first.

```sh
sudo docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.org --dry-run
```

Once it shows a successful message you should now be able to run without the --dry-run flag to generate your ssl certificates.

```sh
sudo docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.org
```

To renew your certificate run command below.

```sh
sudo docker-compose run --rm certbot renew
```

### Prod setup

First disable `./nginx/conf/wordpress.conf`.

```sh
mv ./nginx/conf/wordpress.conf ./nginx/conf/wordpress.conf.disabled
```

Run docker prod setup.

```sh
sudo docker-compose --profile prod up -d
```

Once everything is now running, run certbot with `--dry-run` flag.

```sh
sudo docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.org --dry-run
```

__*Note: if first run fails wait for a few minutes and re run above again.*__

Once you get the successful message you can then run it without the `--dry-run` flag.

```sh
sudo docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.org
```

After that enable `./nginx/conf/wordpress.conf`.

```sh
mv ./nginx/conf/wordpress.conf.disabled ./nginx/conf/wordpress.conf
```

Rerun docker-compose again.

```sh
sudo docker-compose down && sudo docker-compose up --profile prod up -d
```

Access your website via the domain then you should be able to be redirected to the wordpress setup page.