# Dockerfile
FROM node:18.12.1

WORKDIR /app

# Garante que vamos falar com o registry público e sem auth herdada
RUN npm config set registry https://registry.npmjs.org/ \
 && npm config delete '//registry.npmjs.org/:_authToken' || true

# Copia apenas manifestos para maximizar cache
COPY package*.json ./

# Instala dependências com logs mais verbosos (ajuda depuração)
# Se você tem package-lock.json versionado, prefira npm ci
RUN npm ci --verbose --no-audit --no-fund

# Copia o restante do código
COPY . .

EXPOSE 8080
CMD ["node", "server.js"]

    
