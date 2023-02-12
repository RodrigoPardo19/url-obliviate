FROM node:18.12.1-alpine3.17 AS builder
RUN apk add --no-cache libc6-compat
RUN apk update

WORKDIR /app
RUN yarn global add turbo
COPY . .
RUN turbo prune --scope=api --docker

FROM node:18.12.1-alpine3.17 AS installer 
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app

# install dependencies
COPY .gitignore .gitignore
COPY --from=builder /app/out/json/ .
COPY --from=builder /app/out/pnpm-lock.yaml .
COPY --from=builder /app/out/pnpm-workspace.yaml .
RUN corepack enable
RUN pnpm install -r --no-frozen-lockfile

#build the project
COPY --from=builder /app/out/full/ .
RUN pnpm turbo run build --filter=api...

FROM node:18.12.1-alpine3.17 AS runner
WORKDIR /app

COPY --from=installer /app .

EXPOSE 3000
CMD node apps/api/dist/main.js
