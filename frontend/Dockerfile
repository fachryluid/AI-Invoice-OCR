# Stage 1 — Build Vite App
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci --prefer-offline --no-audit --progress=false

COPY . .

# build args for Vite envs (set by docker-compose build.args)
ARG VITE_API_BASE_URL
ARG VITE_API_OCR_PROCESS_FILE
ARG VITE_API_OCR_AUTH_TOKEN
ARG VITE_API_SAVE_ENDPOINT
ARG VITE_USE_MOCK_DATA

# export as env for the build step so Vite can pick them up (Vite reads process.env at build time)
ENV VITE_API_BASE_URL=${VITE_API_BASE_URL}
ENV VITE_API_OCR_PROCESS_FILE=${VITE_API_OCR_PROCESS_FILE}
ENV VITE_API_OCR_AUTH_TOKEN=${VITE_API_OCR_AUTH_TOKEN}
ENV VITE_API_SAVE_ENDPOINT=${VITE_API_SAVE_ENDPOINT}
ENV VITE_USE_MOCK_DATA=${VITE_USE_MOCK_DATA}

RUN npm run build


# Stage 2 — Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

# Replace default nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose container port 80 (internal)
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]