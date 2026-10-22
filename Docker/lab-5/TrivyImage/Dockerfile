FROM node:24-trixie-slim


WORKDIR /app

COPY package.json ./

RUN npm install

COPY app.js ./

EXPOSE 3000

CMD ["npm", "start"]
