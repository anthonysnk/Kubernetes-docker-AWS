FROM ubuntu:20.04 #obteniendo una imagen base

RUN apt-get update
RUN apt-get install nginx -y
RUN echo 'hello world' > /var/www/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"] 