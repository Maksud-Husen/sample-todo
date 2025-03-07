FROM nginx:latest

# Copy custom Nginx configuration
COPY ./default.conf /etc/nginx/conf.d/default.conf

# Expose port
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
