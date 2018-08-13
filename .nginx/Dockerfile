FROM nginx:latest

# Uncomment these 2 lines below if you want to manipulate files from your running container
# RUN apt-get update
# RUN apt-get install vim -y

# Copy the line below and replace the {{SERVICE_NAME}} placeholder with the defined service from your docker compose file
# COPY .nginx/{{SERVICE_NAME}}-vhost.conf /etc/nginx/sites-enabled/{{SERVICE_NAME}}-vhost.conf

RUN echo "include /etc/nginx/sites-enabled/*.conf;" >> /etc/nginx/conf.d/default.conf
RUN echo "server_names_hash_bucket_size 64;" >> /etc/nginx/conf.d/default.conf

WORKDIR /