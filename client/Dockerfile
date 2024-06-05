# Use lightweight Node.js image as the base image
FROM node:14-alpine

WORKDIR /21i-1159_frontend

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 1159

CMD ["npm", "start"]