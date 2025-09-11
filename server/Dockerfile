FROM node:18 AS build
WORKDIR /app
RUN git clone --depth 1 https://github.com/doananhtingithub40102/mern-app.git . \
    && rm -rf .git
WORKDIR /app/server
RUN npm install
#app listens on port 5000
EXPOSE 5000
ENV PORT=5000

#launch server
CMD ["npm", "start"]