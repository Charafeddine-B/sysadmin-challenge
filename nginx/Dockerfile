FROM nginx:1.25.5-alpine-otel




WORKDIR /var/www/html/web

RUN apk --no-cache add nginx






RUN mkdir -p /run/php

COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

RUN printf "test:$(openssl passwd -crypt test)\n" >> /etc/nginx/.htpasswd

RUN chmod 755 /var/cache/nginx
EXPOSE 80


CMD ["nginx", "-g", "daemon off;"]
