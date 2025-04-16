# Étape 1 : Build
FROM node:20-alpine AS builder

WORKDIR /app

COPY package.json ./
RUN npm install

COPY . .
RUN NODE_OPTIONS=--openssl-legacy-provider npm run build

# Étape 2 : Image de production
FROM node:20-alpine

WORKDIR /app

COPY package.json ./
RUN npm install --omit=dev

COPY --from=builder /app/.next .next
COPY --from=builder /app/pages pages
COPY --from=builder /app/node_modules node_modules
COPY --from=builder /app/package.json ./

EXPOSE 9090

CMD ["npx", "next", "start", "-p", "9090", "-H", "0.0.0.0"]
