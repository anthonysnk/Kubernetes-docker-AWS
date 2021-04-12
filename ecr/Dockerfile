FROM ubuntu

RUN apt update
RUN apt install nginx -y
RUN echo hola_mundo > /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]