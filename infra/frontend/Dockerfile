FROM node:18 as build-stage

WORKDIR /app
COPY package.json ./
COPY pnpm-lock.yaml ./
COPY nginx.conf ./
RUN npm install -g pnpm
RUN pnpm install
COPY . .

# Ajouter des variables d'environnement dynamiques
ARG API_URL
ENV VITE_IMPORT_API_URL=${API_URL}

RUN pnpm run build

FROM nginx:alpine
WORKDIR /usr/share/nginx/html
RUN rm -rf *
COPY --from=build-stage /app/nginx.conf /etc/nginx/nginx.conf
COPY --from=build-stage /app/dist .
EXPOSE 80
ENTRYPOINT [ "nginx", "-g", "daemon off;" ]