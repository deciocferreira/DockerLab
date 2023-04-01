# DockerLab <image src="https://user-images.githubusercontent.com/12403699/227597435-511fd8ae-c873-4fa4-b06f-a6fbe9bc1667.png" width="100" height="80">

## Container 

É uma forma de realizar isolamento de recursos computacionais, *imutável*(não se pode alterar) e *efêmero*(só existe naquele momento da execução).
- lógico: Processos, network 
- Físico: Computação, memória

O Kernel do host faz a limitação dos recursos físicos(isolamento).

Imagem do container tem várias camadas(Instrução no Dockerfile). Camada de Read e Write as demais são read (ReadOnly). Se houver um arquivo RO e precisar altera-lo faz-se uma cópia dele para o a camada de read e write. Informações de execução do container, efêmeras (arquivos de log).

Todos os container utilizam a mesma imagem RO. Container utilizam o kernel do host, para um melhor gerenciamento de recursos.

*IPtable* - responsável por regras de Network.

## Principais formas de operação do *Docker*

**Docker Cliente** Executa os comandos que digitamos.
**Docker Deamon** Servidor que gerencia os containers.

## Instalação da ferramenta(Linux-Ubuntu)

*curl -fsSL https://get.docker.com | bash*

Comandos mais utilizados no dia a dia:

- *docker container ls* **Lista os atuais containers.** 
- *docker container ls -a* **Lista todos em execução e que já foram executados.**
- *docker container run -ti* **Execução com interatividade no container.**
- *docker container attach + id* **Retorna para container em execução.**
- *docker container  run -d* **Executa container em modo daemon e não ficar em primeiro plano travando a tela.**
- *docker container exec -ti id* **Executa determinado processo no container, como bash ou vim em Linux por exemplo.**

**ctrl+p+q sai do container sem encerrá-lo**

- *docker container stop* **Para determinado container.**
- *docker container start* **Iniciar determinado container.**
- *docker container restart* **Reinicia determinado container.**
- *docker container inspect* **Lista detalhes do container.**

## Volumes

É uma forma de inserir um File system em um container para persistência de dados de uma determinada aplicação.

Tipos de volumes:

Bind - É quando existe um diretório local e quero inseri-lo no container.
*docker container run -ti --mount type=bind,src=/diretorio,dst=diretorio image*

Volume - É quando inserimos um volume criado anteriormente listado no Docker dentro do container. (No Linux: /var/lib/docker/volumes)
*docker container run -ti --mount type=volume,src=NomeVolume,dst=diretorio image*

*docker volume prune* - deleta volumes que não estão sendo utilizados

**Diferença entre **docker container create** e **docker container run** é que o create apenas criar o container e não executa-o.**

## Backup de dados
- Criação de um container para realizar de backup de determinada aplicação, no caso de um banco Postgres
*docker container run -ti --mount type=volume,src=dbpostgrees,dst=/backup --mount type=bind,src=/opt/backup,dst=backup ubuntu tar -cvf /backup/bkp-postgres.tar /backup*

## Gerenciamento de imagens - Dockerfile
</figure>
<image src="https://user-images.githubusercontent.com/12403699/227609571-c979e282-dd4e-41ca-bf45-ff4512019149.png" width="700" height="350">
<figcaption align = "center"><b>Fig.1 - Exemplo de build e execução de uma imagem baseada em Debian que realiza um processo do Apache server.</b></figcaption>
</figure>

&nbsp;

*docker image build -t .* **Build de uma imagem a partir de um dockerfile no diretório com tag de versão.**

*docker container run -ti -P* analisa se há um expose de portas no dockerfile, porém ele faz o bind com uma porta aleatória, sendo que o recomendado é fazer o bind manualmente com o parametro*-p*.

Não é possível acrescentar um volume do tipo Bind em um Dockerfile. Ele cria um volume automaticamente para o container com o parâmetro *VOLUME /diretorio*.

*EXEC FORM* ***ENTRYPOINT["python", "start.py"]***

*SHELL FORM* ***ENTRYPOINT python star.py***

*ENTRYPOINT* é o principal processo do container, tudo o que é declarado nele.

*LABEL* **Adição de informações como descrição, versão e etc para ser visualizadas na inspeção do container.**

Por padrão o docker utiliza cache ao fazer o build de uma imagem que já foi criada anteriormente, caso não queira utilizar basta usar o parâmetro *--no-cache*

*COPY* **Copiar arquivos de diretório ou diretórios e adiciona no container.**

*ADD* **Copia arquivos de diretório ou remotos e arquivos *.Tar* (conteúdo), sendo mais completo que o *Copy*.**

*Multistage* **se assemelha a uma pipeline dentro do dockerfile, onde uma camada copia conteúdo de outra acima utilizado o parâmetro *COPY --from=parametrodoprimeiroFROM.***

*.dockerignore* **Parâmetro que não permite que arquivos desejados sejam incluídos ao fazer uma cópia.**

*HEALTHCHECK* **Parâmetro dentro da imagem que faz checagem da saúde de tempos em tempos**

*docker commit -m "descrição" imageID* **Adiciona um commit a uma imagem.**

## Docker Hub e Registry

Para fazer upload de uma imagem para um hub ou registry recomenda-se aplicar o devido Tag e utilizar os seguintes comandos como práticas.

*docker image tag imageID nomeimagem* **Adiciona uma Tag e nome a imagem no repositório.**

*docker push nomeimagem* **Faz upload da imagem local para o DockerHub ou repositório online.**

*docker container run -d -p 5000:5000 --restart=always --name registry registry:2* **Cria um registry local na versão recomendada para subir imagens.**

*docker imagem push enderecoregistry/nomeimagemtag* **Faz o upload de imagem local para o registry.**

## Docker Swarm

É um orquestrador *builtin* do docker com dois principais papéis de atuação dentro do cluster, que são o **Manager** e o **Worker**.
O *Worker* é o responsável pela execução dos containers, enquanto que o *Manager* fica encarregado da administração de todo o cluster, cuidando de toda a informação sensível.

Pensando em alta disponibilidade e resiliência do Cluster com Swarm, é necessário que 51% dos *Manager ou Workers node* estejam ativos para garantir a saúde do mesmo.
Sempre que um *Mandager node* cai, ocorre uma eleição para que um próximo node assuma a carga de trabalho.

*docker swarm init* **Inicia o cluster swarm do Docker.** 
- O parâmetro *--advertise-addr* **especifica o endereço IP que irá centralizar o cluster.**

*docker node promote nodename* **Promove o node a managar para garantir resiliência.**

*docker swarm join-token worker/manager* **Retorna comando para adicionar um worker/manager ao cluster.**

*docker swarm join-token --rotate worker/manager **Retorna comando com uma nova chave token para adicionar nós no cluster.**

*docker node update* **Atualiza informações como disponibilidade e Tag no correspondente nó.**
 - Parâmetros *--availability* irá determinar o estado do nó, com as possibilidade de valor: drain, pause, active.

## Service

Garante resiliência dos nós do Swarm. Configuração de diversos containers respondendo/suportando a um único serviço/aplicação.
 - *Load Balancer* das aplicações.

*docker service create --name nomeaplicacao --replicas numeroreplicas -p 8080:8080 nginx* **Criação de service nginx com bind de porta em todos os nós.**

*docker service ls* **Lista os serviços criados.**

*docker service ps nomeservice* **Especifica os status do service que foi passado no comando.**

*docer service inspect nomeservice* **Lista os detalhes do serviço.** 
 - Parâmetro *--pretty* lista os detalhes de forma mais resumida e amigável.

*docker service scale nomeservice=numerodesejadodeescala* **Realiza um scale no cluster de acordo com o número que foi passado no comando.**
 - Recomenda-se como boa prática, fazer um *downscaling* no cluster e depois um *upscale* ao se adicionar novos nodes, para a devida disrtibuição.
 
*docker service logs -f nomeservice* **Disponibiliza logs de todos os containers para realização de análises.** 

## Network

*docker network create -d overlay giropops* **Cria uma rede do tipo overlay, que é utilizada em swarms para comunicação dos containers.**

*docker network ls* **Lista as redes criadas.**

*docker network inspect nomedarede* **Retorna informações sobre a rede.**
 
*Tipo de de rede *ingress* é a que possibilita a comunicação entre todos os containers da mesma rede*

## Secrets

Gerenciamento de informação sensíveis como usuário e senhas. Podem ser criados diretamente por parâmetros ou por meio de arquivos.
Utilizados somente dentro do cluster swarm.

*echo -n "USER/PASSWORD@2023" | docker secret create secret1 -* **Criação de um secret por parâmetros(linux).**

*docker service create --name app --detach=false --secret db_pass  minha_app:1.0* **Criação de service passando um secret.**

*docker service create --detach=false --name app --secret source=db_pass,target=password,uid=2000,gid=3000,mode=0400 minha_app:1.0* **Criação de service passando secret com permissionamento.

## Docker Compose

O compose é uma forma de você conseguir escrever em um único arquivo todos os detalhes e necessidade de sua aplicação. 

Uma Stack é a união de services, que por sua vez é a união de containers. 

*docker stack deploy -c arquivocompose.yml nomeda stack* **Criação de um stack passando um docker compose file.**

*docker stack ps nomedastack* **Lista detalhes sobre a execução da stack.**

Modes de deploy:

- Replicated: você escolhe a quantidade de réplicas do seu service, 
- Global você não escolhe a quantidade de réplicas, ele irá subir uma réplica por node de seu cluster (uma réplica em cada node de seu cluster).

</figure>
<image src="https://user-images.githubusercontent.com/12403699/229312724-588e3c6b-6b40-4105-8f0b-677f43a6cd78.png" width="700" height="350">
<figcaption align = "center"><b>Fig.1 - Exemplo de deploy de uma stack e os serviços criados.</b></figcaption>
</figure>

&nbsp;

## Referências

Documentação oficial do Docker: *https://docs.docker.com/reference/*

Livro descomplicando Docker do Linuxtips: *https://livro.descomplicandodocker.com.br/*

Site para poder praticar e fazer laboratórios: *https://labs.play-with-docker.com/*
