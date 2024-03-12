FROM ubuntu:latest
RUN apt update
RUN apt install apache2 -y
COPY index.html /var/www/html/index.html
RUN chmod 777 /var/www/html
RUN chmod 666 /var/www/html/index.html
EXPOSE 80
CMD ["apache2ctl", "-D", "FOREGROUND"]
