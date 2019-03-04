#Set base image to node to support npm install. 8.15.1-alpine is the latest version of alpine and reduces image size by ~900MB
FROM node:8.15.1-alpine
#Add labels
LABEL title="Best Buy API IoT MACE Query" description="Dockerization of BestBuy CLI and specific query for IoT devices and characteristics relevant to project MACE" query-output="JSON"
#Create app directory
WORKDIR /usr/src/app
#Install Best Buy CLI
RUN npm install --global bestbuy cli
#Bundle app within the Docker container
COPY . .












