# Byggfas
FROM node:14 AS build
WORKDIR /app

# Kopiera package-filer och installera beroenden
COPY package.json package-lock.json ./
RUN npm install

# Kopiera hela projektet och bygg det
COPY . .
RUN npm run build

# Felsök: kontrollera om byggfilerna existerar
RUN ls -l /app/.svelte-kit/output/client

# Produktionsfas
FROM nginx:alpine

# Kopiera byggda filer till Nginx standardrot
COPY --from=build /app/.svelte-kit/output/client /usr/share/nginx/html

# Kopiera din Nginx-konfigurationsfil
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Exponera port 80
EXPOSE 80

# Starta Nginx
CMD ["nginx", "-g", "daemon off;"]
