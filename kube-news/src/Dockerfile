# docker/Dockerfile (exemplo)
FROM node:18.12.1

WORKDIR /app

# 1) Fixar registry público e remover eventual token herdado (evita 403 indevido)
RUN npm config set registry https://registry.npmjs.org/ \
 && npm config delete '//registry.npmjs.org/:_authToken' || true

# 2) Mostrar configuração e versões ANTES de instalar
RUN echo "==== NPM CONFIG LIST ====" \
 && npm config list \
 && echo "==== NODE/NPM VERSIONS ====" \
 && node -v && npm -v

# 3) Instalar toolchain caso exista dependência nativa (bcrypt/sharp/etc.)
#    Você pode comentar este bloco se tiver certeza de que não precisa.
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 make g++ \
 && rm -rf /var/lib/apt/lists/*

# 4) Copiar apenas manifestos para maximizar cache
COPY package*.json ./

# 5) Rodar npm ci verboso (se não houver package-lock.json, troque para npm install)
#    Opções: --legacy-peer-deps para resolver conflitos de peer deps (se necessário)
RUN npm ci --verbose --no-audit --no-fund

# 6) Copiar o restante do projeto
COPY . .

EXPOSE 8080
CMD ["node", "server.js"]
