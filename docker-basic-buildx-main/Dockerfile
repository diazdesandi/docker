# First stage: development dependencies
FROM node:19.2-alpine3.16 AS deps
WORKDIR /app
COPY package.json ./
RUN npm install

# New stage: testing
FROM node:19.2-alpine3.16 AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
# Everything that is not in .dockerignore will be copied
COPY . .
RUN npm run test

# There could be a build step here if needed, for example:
# RUN npm run build

# Production stage: only production dependencies
FROM node:19.2-alpine3.16 AS prod-deps
WORKDIR /app
COPY package.json ./
RUN npm install --prod

# Final stage: run the application
FROM node:19.2-alpine3.16 AS runner
WORKDIR /app
COPY --from=prod-deps /app/node_modules ./node_modules
COPY app.js ./
COPY tasks/ ./tasks
CMD [ "node", "app.js" ]
