#Stage 1: Build React App
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm install


ARG REACT_APP_API_BASE_URL
ENV REACT_APP_API_BASE_URL=$REACT_APP_API_BASE_URL

COPY . .
RUN npm run build

# Stage 2: NGINX for production serve
FROM nginx:alpine

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set temp dirs writable for nginx
RUN mkdir -p /tmp/client_temp /tmp/proxy_temp /tmp/fastcgi_temp /tmp/scgi_temp /tmp/uwsgi_temp && \
    chown -R appuser:appgroup /tmp

# Replace nginx full config with our own
COPY nginx.conf /etc/nginx/nginx.conf

# Copy built app to nginx html folder
COPY --from=builder /app/build /usr/share/nginx/html

# Switch to non-root user
USER appuser

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]