FROM nginx:latest

RUN apt-get update && \
	apt-get install -y --no-install-recommends \
	nano

ADD ./conf.d/* /etc/nginx/conf.d/
COPY ./fastcgi.conf /etc/nginx/fastcgi.conf
ADD ./snippets/* /etc/nginx/snippets/

# SSL
ADD ./certs/*.pem /etc/ssl/certs/
ADD ./certs/*.crt /etc/ssl/certs/
ADD ./certs/*.key /etc/ssl/private/