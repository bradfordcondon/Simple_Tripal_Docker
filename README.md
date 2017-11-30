
For managing my Docker Tripal site.

Until I set up Docker compose, the container can be launched like this:

`docker run -ti -v ~/UTK/dockers/modules:/var/www/html/sites/all/modules/tripal_extensions -v ~/UTK/dockers/data_files:/var/www/html/sites/default/files/host -v ~/UTK/dockers/tripal:/var/www/html/sites/all/modules/tripal -p 8080:80 bcondon/docker_tripal3:withExpression /bin/bash`

Alternatively I can run the base container, without elasticsearch set up/ without developer sequences loaded, like so.

`docker run -it --rm --name docker_tripal_v3 -v ~/UTK/dockers/modules:/var/www/html/sites/all/modules/tripal_extensions -v ~/UTK/dockers/data_files:/var/www/html/sites/default/files/host -v ~/UTK/dockers/tripal:/var/www/html/sites/all/modules/tripal  -p 8080:80 mingchen0919/docker-tripal-v3 /bin/bash`

The `files` and `modules` folders allow for module development and data to persist between machines.

The following modules are installed (and should therefore exist if you map a modules directory)

* tripal_analysis_expression
* tripal_ssr
* tripal_elasticsearch




[Docker container and image management cheatsheet](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)

## Login:

```
Username: admin

default password: P@55w0rd
```


```
docker commit docker-tripal-v3 docker_tripal_3_loaded

docker run -ti -v ~/UTK/dockers/modules:/var/www/html/sites/all/modules/tripal_extensions -v ~/UTK/dockers/data_files:/var/www/html/sites/default/files/host -v  ~/UTK/dockers/tripal:/var/www/html/sites/all/modules/tripal -p 8080:80 docker_tripal_3_loaded /bin/bash
```


## jackson cage container instead

docker run -i -d -p 80 -e APACHE_SERVERNAME=jacksoncage.se -e POSTGRES_HOST=localhost -e POSTGRES_PORT=5432 jacksoncage/phppgadmin





## PHPpgadmin

[Using this image:](https://hub.docker.com/r/zhajor/docker-phppgadmin/)

`docker pull zhajor/docker-phppgadmin`

Then link the image to the tripal container:

```
docker run -d -p 12345:8090 --link docker_tripal_v3:docker_tripal_v3 -e "HOST=docker_tripal_v3" -e "DB_PORT=" --name=phppg zhajor/docker-phppgadmin
```

Note that the password is set to 
```
   'database' => 'tripal_db',
      'username' => 'tripal',
      'password' => 'tripal_db_passwd',
```


Note- rather than using link, we should instead define a network.

`docker network create --driver bridge nw1`

now we specify the network with `--network=isolated_nw`


>You expose ports using the EXPOSE keyword in the Dockerfile or the --expose flag to docker run. Exposing ports is a way of documenting which ports are used, but does not actually map or open any ports. Exposing ports is optional.


>You publish ports using the PUBLISH keyword in the Dockerfile or the --publish flag (also p) to docker run. This tells Docker which ports to open on the container’s network interface. When a port is published, it is mapped to an available high-order port (higher than 30000) on the host machine, unless you specify the port to map to on the host machine at runtime. You cannot specify the port to map to on the host machine in the Dockerfile, because there is no way to guarantee that the port will be available on the host machine where you run the image.


127.0.0.1

`yum install phpPgAdmin`

  /etc/httpd/conf.d/phpPgAdmin.conf


Alias /phpPgAdmin /usr/share/phpPgAdmin

<Directory /usr/share/phpPgAdmin>
   order deny,allow
   deny from all
   allow from 192.168.1.0/24
</Directory>

service httpd restart


`http://192.168.1.100/phpPgAdmin/`



# Building my own container

Thank you to Eric Rasche who maintains an excellent Tripal container, from which I borrowed liberally.  

 wnameless/postgresql-phppgadmin info:
 username: postgres
 password: postgres
 

```
docker build --tag test_container . 
docker run --interactive --tty test_container
```


>Each container can now look up the hostname web or db and get back the appropriate container’s IP address. For example, web’s application code could connect to the URL postgres://db:5432 and start using the Postgres database.

>It is important to note the distinction between HOST_PORT and CONTAINER_PORT. In the above example, for db, the HOST_PORT is 8001 and the container port is 5432 (postgres default). Networked service-to-service communication use the CONTAINER_PORT. When HOST_PORT is defined, the service is accessible outside the swarm as well.