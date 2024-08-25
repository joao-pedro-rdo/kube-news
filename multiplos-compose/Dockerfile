FROM node:20.10.0-alpine3.18
# opção -m no comando useradd cria automaticamente o diretório home para o novo usuário.
RUN adduser -D -h /home/admin admin


WORKDIR /app
# para copiar o package.json e o package-lock.json
# que sao as dependencias do projeto
COPY --chown=admin:admin --chmod=777 /src/package*.json .
RUN npm install
COPY --chown=admin:admin --chmod=777 /src .
EXPOSE 8080
CMD ["node", "server.js"]
USER admin

