FROM nginx:1.25.5-alpine-otel

RUN apk --no-cache add nginx

RUN mkdir -p /run/php

COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

RUN printf "test:$(openssl passwd -crypt test)\n" >> /etc/nginx/.htpasswd

EXPOSE 80

USER www-data

CMD ["nginx", "-g", "daemon off;"]
