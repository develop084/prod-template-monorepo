# apps/backend/Dockerfile

FROM node:20-alpine AS build
WORKDIR /app

# Copy root-level files
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/backend/package.json apps/backend/package.json

RUN corepack enable && corepack prepare pnpm@8.8.0 --activate
# Install ALL dependencies (including dev) for build stage
RUN pnpm install --frozen-lockfile

COPY . .

# Now build backend
RUN pnpm --filter backend build


# ===== Production image =====
FROM node:20-alpine AS prod
WORKDIR /app

COPY --from=build /app/apps/backend/dist ./dist
COPY --from=build /app/apps/backend/package.json .
COPY --from=build /app/node_modules ./node_modules

CMD ["node", "dist/main.js"]
