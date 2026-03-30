# FROM ubuntu

# RUN apt-get update && apt-get install -y curl

# RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && apt-get install -y nodejs

# #Here we using ubuntu as base image, we are installing curl and nodejs in the container. This is a simple Dockerfile that can be used to create a container with nodejs installed. You can build this Dockerfile using the command `docker build -t my-node-app .` and then run it using `docker run -p 3000:3000 my-node-app`.

# This Dockerfile is not secure because it uses the latest version of Ubuntu, which may have vulnerabilities. It also does not specify a user to run the application, which can lead to security issues. Additionally, it does not use a specific version of nodejs, which can also lead to security issues. To make this Dockerfile more secure, you should specify a specific version and nodejs, and also create a non-root user to run the application.


# FROM node:25
# WORKDIR /app
# RUN groupadd -r appgroup && useradd -r -g appgroup appuser
# COPY . .
# RUN npm install
# USER appuser
# EXPOSE 3000
# CMD ["npm", "start"]

FROM node:25 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN echo "appuser:x:10001:10001::/home/appuser:/sbin/nologin" > /tmp/passwd && \
    echo "appgroup:x:10001:" > /tmp/group


FROM gcr.io/distroless/nodejs22-debian13
WORKDIR /app
# RUN groupadd -r appgroup && useradd -r -g appgroup appuser
COPY --from=builder --chown=10001:10001 /app/app.js ./app.js
COPY --from=builder --chown=10001:10001 /app/node_modules ./node_modules
# USER appuser
USER 10001
EXPOSE 3000
CMD ["app.js"]