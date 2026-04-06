# ---- Build stage ----
FROM node:20-alpine AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci --omit=dev

# ---- Production stage ----
FROM node:20-alpine AS production

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Install dumb-init
RUN apk add --no-cache dumb-init

WORKDIR /app

# Copy production dependencies
COPY --from=build /app/node_modules ./node_modules

# Copy app files
COPY package.json ./
COPY src/ ./src/

# Change ownership
RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 5000

ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "src/index.js"]
