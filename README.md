# Projeto kube-news

### Objetivo
O projeto Kube-news é uma aplicação escrita em NodeJS e tem como objetivo ser uma aplicação de exemplo pra trabalhar com o uso de containers.

### Configuração
Pra configurar a aplicação, é preciso ter um banco de dados Postgre e pra definir o acesso ao banco, configure as variáveis de ambiente abaixo:

DB_DATABASE => Nome do banco de dados que vai ser usado.

DB_USERNAME => Usuário do banco de dados.

DB_PASSWORD => Senha do usuário do banco de dados.

DB_HOST => Endereço do banco de dados.

---

## Minhas inserções

- Comando para iniciar o container do Postgres no shell:

```shell
docker container run -d -p 5432:5432 -e POSTGRES_PASSWORD=pg123 -e POSTGRES_USER =kubenews -e POSTGRES_DB=kubenews -v kubenews_vol:/var/lib/postgresql/data postgres:14.10
```

 `-d` => Executa o container em background.

 `-p` -> Mapeia a porta do container para a porta do host.

 `-e` -> Define variáveis de ambiente. [Postgre Docker Hu]( https://hub.docker.com/_/postgres)

 `-v` -> Mapeia um volume para o container.
 - Nesse caso mapeamos o volume `kubenews_vol`(gerenciado pelo docker) para o diretório `/var/lib/postgresql/data` do container.

# Para criar o projeto foi usado
```shell
#Crianndo a build da aplicação
docker build -t joaoprdo/kube-news:v1 -f Dockerfile .

#Criando a rede
docker network create kube-news-brige

#Criando o volume
docker volume create kube-news-vol

#Ciando o container do banco
docker container run -d -p 5432:5432 --name kube-news-db -e POSTGRES_PASSWORD=pg
123 -e POSTGRES_USER=kubenews -e POSTGRES_DB=kubenews --network kube-news-brige -v kube-news-vol:/vat/lib/postgresql/dat
a postgres:12.17

#Criando o container da aplicação:
docker container run -d -p 8080:8080 --name kube-news-app -e DB_DATABASE=kubenews -e DB_USERNAME=kubenews -e DB_PASSWORD=pg123 -e DB_HOST=kube-news-db --network kube-news-brige joaoprdo/kube-news:v1
```
