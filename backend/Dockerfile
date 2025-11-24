FROM node:18-slim

# Install dependencies needed by Sharp
RUN apt-get update && apt-get install -y \
    libvips-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

ENV NODE_ENV=production
ENV PORT=1513

EXPOSE 1513

CMD ["node", "src/server.js"]
