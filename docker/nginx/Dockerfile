FROM nginx:alpine

# Default environment variables
ENV NGINX_HOST=localhost
ENV NGINX_PORT=80

# Adds nginx configuration template
# and "compile" it with env variables
ADD default.template /etc/nginx/conf.d/default.template
RUN envsubst '${NGINX_PORT},${NGINX_HOST}' \
    < /etc/nginx/conf.d/default.template \
    > /etc/nginx/conf.d/default.conf

# Creates and set working directory
RUN mkdir -p /var/www/html
WORKDIR /var/www/html
