
# Azure  <image src="https://user-images.githubusercontent.com/12403699/230789670-469903ac-85e4-4d05-9549-1a2e0f9028d6.png" width="110" height="90">

## Deploy de um container Docker no Azure

Neste exemplo, iremos fazer o deploy da aplicação do Prometheus, através do serviço *App Service* no Azure. 

O *App Services* do Azure é um serviço com base em *HTTP* para hospedagem de aplicativos Web, APIs REST e back-end. Você pode desenvolver usando linguagem .NET, .NET Core, Java, Ruby, Node.js, PHP ou Python. Os aplicativos são executados e escalados com facilidade em ambientes baseados no Windows e no Linux. 

Com isso, temos ganho de tempo nas implantações, portabilidade e escalabilidade, além de reduzir custos de infraestrutura. 

Segue passo a passo para realização da implantação:  

## 1. Construção do Dockerfile localmente
 
Temos que construir uma imagem do Docker que queremos executar na nuvem. Neste caso, vamos usar uma imagem pro Prometheus, que é um dos serviços de monitoramento mais populares do mercado. 
 
<image src="https://user-images.githubusercontent.com/12403699/230739453-0a9faf60-27d4-4f41-92b2-acce0c938906.png" width="900" height="500">

## 2. Upload da imagem para dentro do DockerHub

Com uma conta no DockerHub, faça o upload da imagem anterior para o repositório:
  
<image src="https://user-images.githubusercontent.com/12403699/230790349-68d237ae-903f-4033-bc29-b2e1dffd2dac.png" width="900" height="500">  

## 3. Criação do Azure App service
  
Nesta etapa, é necessária ter uma conta no Azure. Há a possibilidade de se criar uma conta gratuita para experimentar os serviços da nuvem da Microsoft e após a expiração desse período, ela passa a ser uma conta Pay-as-you-go, onde você é cobrado somente pelo que consome.

Com a devida subscription e Resource Group criado, vá para a página de configuração. 

Selecione o Resource Group onde o recurso será criado, de um nome para a aplicação, selecione a opção *Docker Container* e o devido Princing Plan. O restante das opções optei deixar default.
 
<image src="https://user-images.githubusercontent.com/12403699/230795315-98bd34d0-a036-46aa-97d0-306f26362961.png" width="1000" height="600">    
  
Na próxima página de configuração do Docker, selecione *Single Container* para fazer deploy de um container simples, *Docker Hub* como origem da imagem e passe o caminho do repositório anterior do DockerHub:
  
<image src="https://user-images.githubusercontent.com/12403699/230796741-9ed283ab-2e7b-4c17-ae71-477f93fc8725.png" width="1000" height="600">  
  
Revise todas as opções para fazer o deploy corretamente nos passos seguintes. No caso das opções de rede, optei por deixar default. Clique em "Create" ao fim:

<image src="https://user-images.githubusercontent.com/12403699/230796807-5b276324-8825-44af-b708-af5738670550.png" width="1000" height="600">  

<image src="https://user-images.githubusercontent.com/12403699/230796565-caa58fd4-abf4-42bf-a696-d4575f95a12b.png" width="1000" height="600"> 

Clique no endereço disponibilizado e você irá para a aplicação do Prometheus sendo executada em nuvem (Pode ser que demore um tempo, pois o Azure está fazendo todo o trabalho de por a aplicação no ar):
 
<image src="https://user-images.githubusercontent.com/12403699/230796548-5464d7ce-98cf-4de4-a15a-a84eea87bad8.png" width="900" height="500"> 
  
Vimos como é simples fazer deploy de aplicações em curto tempo e sem usar caminhos complexos.

Em um cenário de produção, podemos criar um CDN e regras de acesso para a aplicação.

**Recomendo que toda a infra em nuvem criada no laboratório seja excluída para que não haja cobrança indesejadas.**  

No caso de utilização de outros cloud providers, consultar a devida documentação.  
  
## Referências
  
*Consultar arquivos docker compose e yaml do prometheus na pasta Prometheus deste repo.*  
  
*https://learn.microsoft.com/en-us/azure/app-service/*

*https://learn.microsoft.com/pt-br/azure/app-service/overview*
 
*https://learn.microsoft.com/en-us/azure/docker/*

