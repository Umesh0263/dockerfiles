# 3 Tier Architecture with Docker

1. create 1 docker instance that enabled 80,8080,3306 port.
2. docker install and start and enable docker.
3. first of all I have created 1 container for mysql.
  docker run -d -p 3306:3306 --name mysql-container -e MYSQL_ROOT_PASSWORD=1234 mysql:8.0
4. To see container ip with (docker inspect mysql-container-id)
5. then,create database script in mysql.
   apt install mysql-client
   mysql -h container-ip -u username -p and enter paste the script of database.
6. create docker images for backend.

  FROM ubuntu:latest
  RUN apt update
  RUN apt install openjdk-11-jre-headless -y
  ADD https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.tar.gz .
  RUN tar -xvf apache-tomcat-8.5.99.tar.gz -C /opt
  WORKDIR /opt/apache-tomcat-8.5.99
  COPY context.xml conf/context.xml
  COPY student.war webapps/student.war
  COPY mysql-connector.jar lib/mysql-connector.jar
  EXPOSE 8080
  ENTRYPOINT [ "bin/catalina.sh" ]
  CMD [ "run"]

7. build & run docker image and expose in 8080 port
   docker run -d -p 8080:8080 image-id
8. then, go to context.xml file of java, and this-
   username=(),password=(),and url="http://mysql-container-ip:3306/databasename"
9. lets create another docker file for frontend.
  ex:
   FROM ubuntu:latest
   RUN apt update
   RUN apt install apache2 -y
   COPY index.html /var/www/html/index.html
   RUN chmod 777 /var/www/html
   RUN chmod 666 /var/www/html/index.html
   EXPOSE 80
   CMD ["apache2ctl", "-D", "FOREGROUND"]
10. then,create index.html for redirect to backend page.
    Ex:
    <!DOCTYPE html>
    <html>
    <head>
    </head>
    <body>
        <h1>Three Tier Architecture Project...<h1>
    <a href="http://54.144.195.78:8080/student/">Click here to fill form<a/>
    </body>
    </html>
11. build & run frontend docker file and expose to 80 port.
    ex: docker build .
        docker run -d -p 80:80 imageid
12.then, hits the instance ip to see the registration page.


