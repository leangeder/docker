FROM alpine
#RUN apk -U add curl php-fpm php php-iconv php-json php-dom php-zlib php-curl nginx
RUN apk add -U php-fpm php nginx curl wget vim  php-curl php-iconv php-json php-dom php-zlib php-openssl openssl
RUN mkdir -p /var/www/rainloop && cd /var/www/rainloop && curl -s http://repository.rainloop.net/installer.php | php
ADD nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD nginx && php-fpm -F
