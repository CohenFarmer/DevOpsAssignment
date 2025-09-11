FROM node:18 AS build
#use node:18 image to build app, then set dir to app

WORKDIR /app
ARG REACT_APP_YOUR_HOSTNAME=/api
ENV REACT_APP_YOUR_HOSTNAME=${REACT_APP_YOUR_HOSTNAME}

#copy the repo
RUN git clone --depth 1 https://github.com/doananhtingithub40102/mern-app.git . \
    && rm -rf .git

WORKDIR /app/client

#install dependencies then build react app
RUN npm install && npm run build


FROM nginx:alpine
COPY --from=build /app/client/build /usr/share/nginx/html

# uses this nginx config instead of the default one
# had problems with api requests with kubernetes
RUN rm /etc/nginx/conf.d/default.conf \
    && cat > /etc/nginx/conf.d/default.conf <<'EOF'
server {
    listen 80;
    server_name _;
    root /usr/share/nginx/html;
    index index.html;

    # strip api from the path before proxy
    location /api/ {
        rewrite ^/api/(.*)$ /$1 break;
        proxy_pass http://backend:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location / {
        try_files $uri /index.html;
    }
}
EOF

EXPOSE 80
#run nginx in the container
CMD ["nginx", "-g", "daemon off;"]