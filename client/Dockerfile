FROM node:18 AS build
WORKDIR /app
ARG REACT_APP_YOUR_HOSTNAME=/api
ENV REACT_APP_YOUR_HOSTNAME=${REACT_APP_YOUR_HOSTNAME}
RUN git clone --depth 1 https://github.com/doananhtingithub40102/mern-app.git . \
    && rm -rf .git
WORKDIR /app/client
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=build /app/client/build /usr/share/nginx/html
COPY client/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]