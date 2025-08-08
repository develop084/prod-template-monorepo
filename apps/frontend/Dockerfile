FROM node:20-alpine AS build
WORKDIR /app
COPY ../../package.json ../../pnpm-lock.yaml ./
COPY ../../pnpm-workspace.yaml ./
COPY . .
RUN corepack enable && corepack prepare pnpm@8.8.0 --activate
RUN pnpm install --frozen-lockfile
RUN pnpm --filter frontend build

FROM nginx:stable-alpine
COPY --from=build /app/apps/frontend/dist /usr/share/nginx/html
COPY apps/frontend/nginx.conf /etc/nginx/conf.d/default.conf
