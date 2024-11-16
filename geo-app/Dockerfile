# ============================
# Stage 1: Build the Application
# ============================
FROM node:18 AS builder

# Set working directory
WORKDIR /app

# Copy package.json, package-lock.json, and tsconfig.json
COPY package.json package-lock.json tsconfig.json ./

# Install dependencies
RUN npm ci

# Copy the entire project, excluding files listed in .dockerignore
COPY . .

# Build the Next.js application (using the TypeScript configuration)
RUN npm run build

# ============================
# Stage 2: Production Image
# ============================
FROM node:18-alpine AS runner

# Set working directory
WORKDIR /app

# Install only production dependencies
COPY package.json package-lock.json ./
RUN npm ci --only=production

# Copy the built application files
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/next.config.ts ./next.config.ts
COPY --from=builder /app/package.json ./package.json

# Expose the application port
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
