# Stage 1 — Build Vite App
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build


# Stage 2 — Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

# Replace default nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 5173

CMD ["nginx", "-g", "daemon off;"]
