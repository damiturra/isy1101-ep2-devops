FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN if [ -f package-lock.json ]; then npm ci; else npm install; fi

COPY . .

RUN npm run build


FROM nginxinc/nginx-unprivileged:1.27-alpine

COPY --from=build --chown=101:101 /app/dist /usr/share/nginx/html

COPY --chown=101:101 nginx.conf.template /etc/nginx/templates/default.conf.template

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]