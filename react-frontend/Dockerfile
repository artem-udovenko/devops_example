# Build stage
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./

RUN npm cache clean --force && \
    npm ci --no-optional --silent

COPY . .

RUN npm run build && \
    npm cache clean --force

# Production stage
FROM nginx:alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup && \
    chown -R appuser:appgroup /var/cache/nginx && \
    chown -R appuser:appgroup /var/run && \
    chown -R appuser:appgroup /var/log/nginx && \
    touch /var/run/nginx.pid && \
    chown -R appuser:appgroup /var/run/nginx.pid

COPY --chown=appuser:appgroup --from=build /app/build /usr/share/nginx/html
COPY --chown=appuser:appgroup nginx.conf /etc/nginx/conf.d/default.conf

USER appuser

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
