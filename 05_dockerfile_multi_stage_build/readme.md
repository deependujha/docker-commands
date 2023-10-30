# Multi-Stage Dockerfile

- Typically, a Dockerfile builds a single image. However, a Dockerfile can contain multiple FROM instructions. This is called a multi-stage build.

- Many frontend applications are built using Node.js and React. The frontend application is built using Node.js and the resulting static files are served by a web server such as Nginx. The following Dockerfile builds a multi-stage image for a frontend application.

- Here's the multi-stage Dockerfile for Next.Js:

```Dockerfile
FROM node:18-alpine AS Build_Image

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# remove dev dependencies
RUN npm prune --production

# ====================

FROM node:18-alpine


# copy from build image
COPY --from=BUILD_IMAGE /app/package.json ./package.json
COPY --from=BUILD_IMAGE /app/node_modules ./node_modules
COPY --from=BUILD_IMAGE /app/.next ./.next
COPY --from=BUILD_IMAGE /app/public ./public
EXPOSE 3000
CMD ["npm","run", "start"]
```