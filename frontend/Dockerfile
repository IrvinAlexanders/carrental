# Build 1 ----- Node modules
FROM node:lts-alpine as build
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json ./
RUN npm i --silent
COPY . ./

RUN npm run build

# Build 2 ----- Deploy build on NGINX
FROM nginx:1.21
RUN  apt-get update \
  && app-get install -y wget \
  && rm -rf /var/lib/apt/lists/*
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/nginx/nginx.conf /etc/nginx/conf.d/default.conf:ro
COPY --from=build /app/dist /usr/share/nginx/html