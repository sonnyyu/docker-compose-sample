## WordPress with External MySQL
This example defines one of the basic setups for WordPress. More details on how this works can be found on the official [WordPress image page](https://hub.docker.com/_/wordpress).


Project structure:
```
.
├── docker-compose.yaml
└── README.md
```

[_docker-compose.yaml_](docker-compose.yaml)
```
services:
  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    restart: always
    ...
```

When deploying this setup, docker-compose maps the WordPress container port 80 to
port 80 of the host as specified in the compose file.

> ℹ️ **_INFO_**  
> For compatibility purpose between `AMD64` and `ARM64` architecture, we use a MariaDB as database instead of MySQL.  
> You still can use the MySQL image by uncommenting the following line in the Compose file   
> `#image: mysql:8.0.27`

# Test mysql login from docker host
```bash
mysql -uroot -h127.0.0.1 -ppassword
```
# Create DB, User and grant access 
```bash
CREATE DATABASE wordpress;
CREATE USER 'wordpress'@'%' IDENTIFIED BY 'wordpress';
GRANT ALL ON wordpress.* TO 'wordpress'@'%';
FLUSH PRIVILEGES;
```
# Edit docker-compose.yaml WORDPRESS_DB_HOST as mysql IP
```bash
    environment:
      - WORDPRESS_DB_HOST=192.168.1.204
```

## Deploy with docker-compose

```
$ docker-compose up -d
Creating network "wordpress_default" with the default driver
Creating volume "wordpress_db_data" with default driver
Pulling wordpress (wordpress:latest)...
latest: Pulling from library/wordpress
f7a1c6dad281: Pull complete
```


## Expected result

Check containers are running and the port mapping:
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
5fbb4181a069        wordpress:latest    "docker-entrypoint.s…"   35 seconds ago      Up 34 seconds       0.0.0.0:80->80/tcp    wordpress-mysql_wordpress_1
```

Navigate to `http://localhost:80` in your web browser to access WordPress.

![page](output.jpg)

Stop and remove the containers

```
$ docker-compose down
```

To remove all WordPress data, delete the named volumes by passing the `-v` parameter:
```
$ docker-compose down -v
```
