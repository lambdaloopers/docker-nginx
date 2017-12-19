FROM library/nginx:1.13

ARG USER_ID=1000
ARG GROUP_ID=1000

COPY ./app/build /var/www/html

RUN groupmod -g ${GROUP_ID:-1000} www-data \
  && usermod -u ${USER_ID:-1000} www-data

RUN chown -R www-data:www-data /var/www/html
