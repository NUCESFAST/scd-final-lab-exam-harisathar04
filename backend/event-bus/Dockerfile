FROM node:14

WORKDIR /21i-1159_backend

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3363

CMD ["node", "server.js"]