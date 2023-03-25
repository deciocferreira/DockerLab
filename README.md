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
