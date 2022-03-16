#2.6 Working with Docker 
#2.6.1 Create project directory and go into it:
	
	mkdir data
	cd data
	
#2.6.2 Install docker on the VM using official tutorial https://docs.docker.com/engine/install/ubuntu/

	![Alt text](/screenshot/docker_test.png?raw=true "Docker test")

#2.6.3 Run docker container with nginx:
	
	docker run -it --rm -d -p 8080:80 --name web nginx
	
#2.6.4 Run docker container with MySQL:

	docker run -d --rm --name mysql-web -v /data -e MYSQL_ROOT_PASSWORD=Pass1234 mysql:latest
	
#2.6.5 Working with MySQL:
	
	docker exec -it mysql-web bash

	mysql -u root -p 
	
	use mysql
	
	CREATE USER 'ZhukAV'@'%' identified by 'Pass12345'
	
	CREATE DATABASE Itransition
	
	![Alt text](/screenshot/mysql_db.png?raw=true "Working with MySQL")
	
#2.7 Working with Dockerfile
#2.7.1 Create Dockerfile:
	
	nano Dockerfile
	
#2.7.2 Fill Dockerfile instruction to install ruby:

	![Alt text](/screenshot/Dockerfile.png?raw=true "Dockerfile")
	
#2.7.3 Run container from Dockerfile image:

	docker build ~/data -t ubuntu-ruby
	
	docker run -d -it --name ubuntu2004ruby ubuntu-ruby
	
	docker exec -it ubuntu2004ruby bash
	
	ruby -v 
	
	![Alt text](/screenshot/ruby_-v.png?raw=true "Installed Ruby")

#2.8 Working with Docker Compose
#2.8.1 Install docker-compose using official tutorial https://docs.docker.com/compose/install/
#2.8.2 Create docker-compose.yml file:
	
	![Alt text](/screenshot/docker-compose_yml.png?raw=true "docker-compose.yml")
	
	docker-compose up -d
	
	![Alt text](/screenshot/docker-compose_up.png?raw=true "docker-compose up")

	

