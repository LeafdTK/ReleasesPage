# Stage 1: Build the Next.js app
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy the package.json and install dependencies
COPY package.json package-lock.json ./
RUN npm ci

# Copy the entire project into the container
COPY . .

# Build the Next.js app
RUN npm run build

# Stage 2: Production image, copy the build artifacts from Stage 1
FROM node:18-alpine

WORKDIR /app

# Copy only necessary files from the builder stage
COPY --from=builder /app/package.json /app/package-lock.json ./
COPY --from=builder /app/.next /app/.next
COPY --from=builder /app/next.config.js /app/

# Only copy the public folder if it exists
COPY --from=builder /app/public /app/public || true

# Install production dependencies
RUN npm ci --only=production

# Expose the port the app runs on
EXPOSE 3000

# Set environment variables for production
ENV NODE_ENV=production

# Command to run the app
CMD ["npm", "start"]
