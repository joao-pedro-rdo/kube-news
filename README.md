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