### Global args
ARG PORT 3000

### Builder
FROM node:alpine3.16@sha256:dda50fd533b57096bfb959afa8bfd290dc3c127207907177b578d44ee14d7119 AS build

# Build static files
WORKDIR /app
COPY . .
RUN npm ci && \
    npm run build

### Deploy
FROM caddy:2.4.6-alpine@sha256:15e576e7d00b1f41a648c5295a03677c24da5fede09131edcb1e6d809c7dc8aa
ARG PORT

# Copy Caddyfile and static files
COPY --from=build /app/Caddyfile /etc/caddy/Caddyfile
COPY --from=build /app/dist /srv

# Port, Healthcheck and user
EXPOSE ${PORT}
HEALTHCHECK --interval=30s --timeout=3s \
    CMD curl -f http://localhost/:${PORT} || exit 1
USER app
