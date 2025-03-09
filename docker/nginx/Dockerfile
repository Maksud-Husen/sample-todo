FROM nginx:latest

# Set working directory
WORKDIR /var/www/html/Todo

# Copy custom Nginx configuration
COPY ./docker/nginx/default.conf /etc/nginx/conf.d/

# Set correct permissions
RUN chmod -R 755 /var/www/html

# Expose ports for HTTP and HTTPS
EXPOSE 80 443

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
