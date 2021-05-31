# Setting up a simple local webserver
* In one Docker container we have:
 - Nginx
 - phpMyAdmin
 - wordpress
 - mariaDB

- To start it:
 docker build -t server_name .
- To run it:
 docker run --name server_name -it -p 443:443 -p 80:80