FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm i
COPY . .
RUN npm run build

FROM node:16-alpine AS production
WORKDIR /app
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/ecosystem.config.js ./
RUN npm i --omit=dev && npm i pm2 -g
COPY --from=builder /app/dist ./dist/
EXPOSE 3000
CMD [ "pm2-runtime", "start", "ecosystem.config.js" ]