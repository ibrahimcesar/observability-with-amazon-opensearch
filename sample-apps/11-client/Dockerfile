# Build step #1: build the React front end
FROM node:18-alpine as build-step
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
RUN npm install npm@9.8.1 --location=global
COPY *.json ./
COPY ./src ./src
COPY ./public ./public
RUN npm install --loglevel "silent"
RUN npm run build

# Build step #2: build the API with the client as static files
FROM python:3.11-slim
WORKDIR /app
COPY --from=build-step /app/build ./build

# Make sure we use the virtualenv:
ENV PATH="/app/env/bin:$PATH"

COPY api/requirements.txt api/api.py api/.flaskenv ./
RUN pip install -r requirements.txt


EXPOSE 5000
ENTRYPOINT [ "python3", "api.py" ]
