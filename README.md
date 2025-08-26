## Two-Tier Flask Application with MySQL and Docker

This project demonstrates how to **containerize a two-tier application** consisting of a **Python Flask application** and a **MySQL database** using Docker. The guide covers everything from building the Docker images to running the containers locally and pushing them to Docker Hub.

## What is a Two-Tier Application?
A two-tier application is a client-server architecture where the presentation layer (the frontend) and the data layer (the backend database) are separated. In this project, our **Flask application** serves as the presentation layer and the **MySQL database** is the data layer.

## Steps

- Dockerfile
- Image
- Run container locally
- Push to Dockerhub

Devops has:
- Code
- Build
- Deploy

We are working on Code and Build in this goal.
### Step 1: Code
  ```bash
git clone https://github.com/<your-username>/Two-Tier-Python-Flask-App-with-MySQL-Docker
.git
cd Two-Tier-Python-Flask-App-with-MySQL-Docker
  ```

### Step 2: Build
- **Dockerfile**

We have Dockerfile
- **Build**

(Before build, please create a network because we have 2 tier application, so both container can talk to each other.)
```bash
docker network create twotier
docker build -t flaskapp .
```
- **Run container locally**
  - MySQL
```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```
  - Python Flask App
```bash
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest
```


> Let's check weather our data has done into MySQL database or not.
```bash
docker exec -it mysql bash
mysql -u root -p
# Enter password: admin
show databases;
use mydb;
show tables;
select * from messages;

# Exit MySQL shell and then container bash
exit
exit
```


- **Push**
```bash
docker login
docker tag flaskapp YOUR-DOCKERHUB-USERNAME/flaskapp:latest
docker push YOUR-DOCKERHUB-USERNAME/flaskapp:latest
```
