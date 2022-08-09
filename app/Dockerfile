FROM node:14-alpine

WORKDIR /bootcamp-app
COPY . .
RUN npm install
# RUN npm run initdb

ENV port=80
EXPOSE 80

CMD [ "npm", "run", "initdb" ]
ENTRYPOINT [ "npm", "run", "dev" ]
