# ==========================
# Build Stage
# =========================
FROM node:20-alpine AS build

WORKDIR /usr/src/app

COPY package*.json ./
RUN yarn install --frozen-lockfile

COPY . .
RUN yarn build


# =========================
# Runtime Stage
# =========================
FROM node:20-alpine

WORKDIR /app

COPY --from=build /usr/src/app/node_modules ./node_modules
COPY --from=build /usr/src/app/dist ./dist

EXPOSE 3000

ENTRYPOINT ["node", "dist/main.js"]