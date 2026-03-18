# Wordpress-In-Docker
Installation steps of Wordpress in Docker
---------------------------------------------

WordPress in Docker – Quick Notes 2026
1. Prerequisites

Install Docker Desktop (Windows/Mac) or Docker Engine (Linux)

Verify:

docker --version
docker-compose --version
------------------------------------
2. Project Setup
mkdir wordpress-docker
cd wordpress-docker

--------------------------------------
4. Create docker-compose.yml

Services: db (MySQL), wordpress (WordPress+Apache), phpmyadmin

Example:

version: '3.9'
services:
  db:
    image: mysql:8.1
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass123
      MYSQL_ROOT_PASSWORD: rootpass123
    volumes: 
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:6.4-php8.2-apache
    depends_on: [db]
    ports: ["8000:80"]
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass123
      WORDPRESS_DB_NAME: wordpress
    volumes: 
      - wordpress_data:/var/www/html

  phpmyadmin:
    image: phpmyadmin:latest
    depends_on: [db]
    ports: ["8080:80"]
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: rootpass123

volumes:
  db_data:
  wordpress_data:
4. Start Containers
docker-compose up -d
docker ps  # check running containers

WordPress → http://localhost:8000

phpMyAdmin → http://localhost:8080
-------------------------------------------
5. Complete WordPress Setup

Open http://localhost:8000

Choose language, site title, admin username/password, email

Click Install WordPress
-----------------------------------
6. phpMyAdmin (Optional)

Login:

User: root

Password: rootpass123

Manage DB visually
-----------------------------------
7. Useful Docker Commands
docker-compose down          # stop containers
docker-compose down -v       # stop + remove volumes
docker-compose restart       # restart containers
docker-compose logs -f       # view logs
