# =============================================================================
# Multi-stage build : permet de séparer l'environnement de build/test de
# l'environnement de production. Avantages :
# - Sécurité : les outils de dev n'arrivent pas en prod
# - Taille : l'image finale est beaucoup plus légère (alpine = ~5Mo de base)
# - Les secrets de build ne sont pas inclus dans l'image finale
# =============================================================================

# Stage 1 – Build & Tests
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm test

# Stage 2 – Production
FROM node:20-alpine AS production

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY --from=builder /app/server.js .

EXPOSE 3000

CMD ["node", "server.js"]