FROM node:18.12.1-alpine3.17 AS builder
RUN apk add --no-cache libc6-compat
RUN apk update

WORKDIR /app
RUN yarn global add turbo
COPY . .
RUN turbo prune --scope=web --docker

# Add lockfile and package.json's of isolated subworkspace
FROM node:18.12.1-alpine3.17 AS installer
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app

# First install dependencies (as they change less often)
COPY .gitignore .gitignore
COPY --from=builder /app/out/json/ .
# COPY --from=builder /app/out/pnpm-*.yaml .
COPY --from=builder /app/out/pnpm-lock.yaml .
COPY --from=builder /app/out/pnpm-workspace.yaml .
RUN corepack enable
RUN pnpm install -r --no-frozen-lockfile

# Build the project
COPY --from=builder /app/out/full/ .
RUN pnpm turbo run build --filter=web...

FROM nginx:1.23.3-alpine as runner
COPY --from=installer /app/apps/web/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
