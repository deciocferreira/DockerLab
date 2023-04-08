
# LabGCP <image src="https://user-images.githubusercontent.com/12403699/230738690-db1e14db-d96f-4e39-ae32-e976b6430fde.png" width="110" height="90">


## Utilizando Docker em Cloud

Utilizando do Docker aliado a computação em nuvem, no exemplo GCP, nos permite acelerar as implantações, aumentando a portabilidade e escalabilidade, além de reduzir custos de infraestrutura. 
  
Vamos subir um serviço do Prometheus em um container na nuvem, usando os serviços CloudShell, Container Registry e CloudRun do GCP.

  Segue passo a passo para realização da implantação.  

## 1. Construção do Dockerfile localmente
 
<image src="https://user-images.githubusercontent.com/12403699/230739453-0a9faf60-27d4-4f41-92b2-acce0c938906.png" width="900" height="500">


## 2. Upload do projeto para dentro do GCP Registry

Com uma conta do Google Cloud previamente criada, faça o upload para o cloudShell da seguinte forma: 
  
<image src="https://user-images.githubusercontent.com/12403699/230739671-0c7b1c09-a189-4efb-b078-c6015582852e.png" width="900" height="500">  
  
Após upload feito, faça o unzip da pasta com os arquivos necessários para executar o Prometheus.
 
<image src="https://user-images.githubusercontent.com/12403699/230739820-c7198fb5-c445-4a2d-b909-55ad2ea18178.png" width="500" height="200">  
 
 
  
 
## 3. Teste do container no CloudShell


## 4. Deploy da aplicação no CLoudRun
