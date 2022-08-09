FROM node:fermium-alpine3.16

WORKDIR /usr/app/

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 80

CMD ["npm", "start"]

