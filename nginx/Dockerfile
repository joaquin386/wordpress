FROM nginx:1.15.10
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
RUN rm /etc/nginx/conf.d/default.conf
COPY wordpress.example.org.pem /cert.pem
COPY wordpress.example.org.key /cert.key
COPY dhparam2048.pem /dhparam2048.pem
COPY nginx.conf /etc/nginx/
COPY wordpress_port80.conf /etc/nginx/conf.d/
COPY wordpress.conf /etc/nginx/conf.d/
