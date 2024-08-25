# Multiplos Compose

## Descrição

Este diretorio é uma replica do diretorio acima, porém com a utilização do docker compose sera diferente, para testar os multiplos composes 

### Comandos uteis

```bash
ocker compose up -d
docker compose down             # Finaliza os contêineres gerenciados pelo Compose
docker compose stop             # Para os contêineres sem removê-los
docker compose ls               # Lista os projetos Docker Compose ativos
docker compose ps               # Lista os contêineres que fazem parte do Compose em uso
docker compose logs             # Exibe os logs de todos os serviços
docker compose logs <nome-do-serviço>  # Exibe os logs de um serviço específico
docker compose exec <nome-ct> <comando> # Executa um comando em um contêiner específico
docker compose up -d --remove-orphans   # Remove contêineres órfãos ao iniciar
docker compose -f <arquivo>.yaml        # Especifica um arquivo Compose para usar
docker compose build            # Constrói as imagens definidas no Compose
docker compose push             # Envia as imagens para um registry
docker compose pull             # Baixa as imagens necessárias para o Compose
docker compose config           # Exibe as configurações do Compose no terminal
docker compose --env-file <nome>.env up -d  # Executa o Compose com um arquivo de variáveis de ambiente
```

### Build de imagem no compose

```bash
image: joaoprdo/kube-news
build: # buildar a imagem, especificamos que vamos buildar a imagem para levantar o container
  context: . # caminho do dockerfile
```

Nesse trecho acima estamos definindo que para nossa imagem nos vamos builder ela apartir do dockerfile, se estivermos em desenvolvimento podemos passar docker compose up -d --build para forçar o build da imagem a partir do dockerfile, se nao passar a flag de --build se a imagem já existir ele não vai buildar novamente, somente se não existir a imagem

### Parametrizar do compose

Podemos usar variáveis de ambiente para ganhar mais flexibilidade no ambiente 

`usamos o ${<name>}` 

```bash
 kube-news-db:
    image: postgres:${POSTGRES_TAG}    #postgres:12.17
    container_name: kube-news-db
 
  kube-news-app:
    image: joaoprdo/kube-news:${KUBE_NEWS_TAG}
```

para passarmos no compose, passamos as variáveis e depois o compose, `<VARIAVEL> <VARIAVEL> docker compose up -d`    e para conferir as config podemos verificar com `docker compose config` 

> PODEMOS USAR UM **.env** e ter cada um dos tipos do arquivo para cada coisa, desenvolvimento, homolog, produção ….
> 

```bash
#Arquivo .env
POSTGRES_TAG=12.17
KUBE_NEWS_TAG=v1
```

e para executar usando um .env usamos docker compose --env-file <nome>.env up -d   

Tambem podemos setar uma configuração de default pra caso nao seja passado .env 

```bash
kube-news-app:
    image: joaoprdo/kube-news:${KUBE_NEWS_TAG:-latest} # tag da imagem
    build: # buildar a imagem, especificamos que vamos buildar a imagem para levantar o container
```

podemos usar essa opção para buildar a imagem com o  `<VARIAVEL> docker compose build `

## Múltiplos Compose

### Extends

Suponhamos que temos um arquivo chamado de compose.yaml para definir nossas configurações do docker copose para usarmos o merge basicamente teremos um arquivo que sobrescreve as configura;coes no nosso compose base o nome padrão é compose.overide.yaml caso nao  use o nome original pode usar docker compose -f compose.yaml -f <nome-alternativo>.yaml  ai dps usar up ou config, desse modo passamos mais de um arquivo para fazer merge e sobrescrever 

### Include 
Incluir todo um arquivo, como se nós tivéssemos definido ele naquele mesmo arquivo.

### Profile 

Permite que criemos perfis de execução para... diferentes situações e possamos alterná-la entre elas. Como por exemplo.: se nós usamos o postgree para desenvolvimento, mas não vamos usar ele para produção. Podemos fazer isso com o profile.

<aside>
💡 Se usar o profile voce deve tomar cuidando com ouso o depends_on , Com o profiles, nem sempre, dependendo do perfil que escolhermos, todos os serviços serão criados, então, se tiver alguma dependência entre os serviços o depends_on vai travar dependendo do perfil que a gente escolher.

</aside>

```bash
#exemplo de execuçao 
docker compose --profile <dev> up -d
```