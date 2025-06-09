# Stage 1: Build the React app
FROM node:18-alpine AS builder

WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --silent
COPY . .
RUN npm run build

# Stage 2: Serve the built app
FROM nginx:alpine

# Copy built assets from builder (changed from /app/build to /app/dist)
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy custom nginx config (create this file or remove this line)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
