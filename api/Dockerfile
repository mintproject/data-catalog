
FROM node:16 AS build-env
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
WORKDIR /home/node/app
COPY frontend/package.json .
COPY --chown=node:node frontend/ .
RUN yarn install --network-timeout 1000000000
USER node
ARG MAPGL_ACCESS_TOKEN
RUN yarn build

FROM python:3.7.15-alpine3.15
RUN apk update && apk add postgresql-dev gcc python3-dev musl-dev g++ nodejs npm
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
COPY --from=build-env /home/node/app/public/ frontend/public/
EXPOSE 7000
ENTRYPOINT [ "python" ]
CMD [ "app.py" ]
