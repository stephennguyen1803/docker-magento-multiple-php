FROM arm64v8/ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Install necessary packages
RUN apt-get update && \
    apt-get install -y nginx tini tzdata && \
    apt-get clean

# Timezone configuration
RUN ln -fs /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# Add your custom nginx config
ADD conf/nginx.conf /etc/nginx/nginx.conf

# Create directories and set permissions
RUN mkdir -p /home/public_html /run/php && \
    groupadd -g 1000 nginx && \
    useradd -u 1000 -g nginx -M -d /home/nginx -s /bin/false nginx && \
    chown -R nginx:nginx /home/public_html/

WORKDIR /home/public_html

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["nginx", "-g", "daemon off;"]
