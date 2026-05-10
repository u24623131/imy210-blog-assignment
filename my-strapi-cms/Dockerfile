FROM node:20-bullseye

RUN apt-get update && apt-get install -y python3 make g++ && rm -rf /var/lib/apt/lists/*

WORKDIR /opt/strapi

COPY package*.json ./
RUN npm install
RUN npm rebuild better-sqlite3 --build-from-source

COPY . .

RUN npm run build

EXPOSE 1337

CMD ["npm", "start"]