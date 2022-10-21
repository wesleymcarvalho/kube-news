#especifica a versao da imagem node do para não quebrar o projeto caso haja atualização
FROM node:18.11.0
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . . 
EXPOSE 8080
CMD ["node", "server.js"]