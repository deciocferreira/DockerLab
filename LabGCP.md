
# GCP    <image src="https://user-images.githubusercontent.com/12403699/230738690-db1e14db-d96f-4e39-ae32-e976b6430fde.png" width="110" height="90">


## Utilizando Docker em Cloud

Docker aliado a computação em nuvem, no exemplo GCP, nos permite acelerar as implantações, aumentando a portabilidade e escalabilidade, além de reduzir custos de infraestrutura. 
  
Vamos subir um serviço do Prometheus em um container na nuvem, usando os serviços *CloudShell*, *Container Registry* e *CloudRun do GCP*.

Segue passo a passo para realização da implantação.  

## 1. Construção do Dockerfile localmente
 
<image src="https://user-images.githubusercontent.com/12403699/230739453-0a9faf60-27d4-4f41-92b2-acce0c938906.png" width="900" height="500">

## 2. Upload do projeto para dentro do GCP Registry

Com uma conta do Google Cloud previamente criada, faça o upload para o *CloudShell* da seguinte forma: 
  
<image src="https://user-images.githubusercontent.com/12403699/230739671-0c7b1c09-a189-4efb-b078-c6015582852e.png" width="900" height="500">  
  
Após upload feito, faça o unzip da pasta com os arquivos necessários para executar o Prometheus.
 
<image src="https://user-images.githubusercontent.com/12403699/230739820-c7198fb5-c445-4a2d-b909-55ad2ea18178.png" width="500" height="200">  
 
## 3. Teste do container no CloudShell
  
Depois de subir o Docker compose e garantir o funcionamento dos serviços, vamos testar o funcionamento do Prometheus:
  
<image src="https://user-images.githubusercontent.com/12403699/230741519-5aaae182-2ef9-4b4f-8b35-f35e1e5b7df1.png" width="900" height="500">    
  
Verificando a aba do Prometheus:
  
<image src="https://user-images.githubusercontent.com/12403699/230741611-354c5973-05cf-4792-9f7d-4d261e52fd66.png" width="900" height="500">    

## 4. Deploy da aplicação no CloudRun
  
Para fazer o deploy do Prometheus, é necessário fazer o push da imagem do serviço criada anteriormente para o repositório do GCP:
  
<image src="https://user-images.githubusercontent.com/12403699/230742508-811547f9-ee85-4dc5-994e-3347fd18df88.png" width="900" height="500">  

Para fazer o deploy, podemos clicar nesta opção simples "Deploy to Cloud Run":

<image src="https://user-images.githubusercontent.com/12403699/230742545-34dfcc79-cceb-4c43-80e6-3ea8771ab091.png" width="900" height="500">

Na página de configuração do Deploy, vamos optar para que não haja autenticação no Container, já que é uma aplicação Simples. &nbsp;
 
Em termos de produção recomenda-se utlizar autenticação.
  
<image src="https://user-images.githubusercontent.com/12403699/230742637-c0426794-d8d4-4c20-bc48-6caec4d40844.png" width="900" height="500">  

Agora, vamos alterar a porta para 9090, já que foi configurada anteriormente:

<image src="https://user-images.githubusercontent.com/12403699/230742674-371c286a-3206-4c6a-8fa3-55394307b245.png" width="900" height="500">    
  
Revise todas as opções para fazer o deploy corretamente. No caso optei por deixar default. Clique em "Create" ao fim:

<image src="https://user-images.githubusercontent.com/12403699/230742981-eaaa8e33-f502-45df-8719-a6685c2a0b81.png" width="900" height="500"> 
  
Clique no endereço disponibilizado e você irá para a aplicação do Prometheus sendo executada em nuvem:
  
<image src="https://user-images.githubusercontent.com/12403699/230743016-ac428d4c-755d-4edc-8eb4-21a0dfff6c35.png" width="900" height="500"> 
  
Vimos como é simples fazer deploy de aplicações em curto tempo e sem usar caminhos complexos. 

**Recomendo que toda a infra em nuvem criada no laboratório seja excluída para que não haja cobrança indesejadas.**  

No caso de utilização de outros cloud providers, consultar a devida documentação.  
  
## Referências
  
*Consultar arquivos docker compose e yaml do prometheus na pasta Prometheus deste repo.*  
  
*https://cloud.google.com/shell/docs*

*https://cloud.google.com/run/docs*

*https://cloud.google.com/container-registry/docs*
