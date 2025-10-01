"# Mottu-DevOps" 

🏍️ Moto-Flow – Sistema de Gestão de Motos API
📋 Descrição da Solução
--
O Moto-Flow é um sistema inteligente de controle e rastreamento de motos em pátios, desenvolvido para otimizar a entrada, saída, localização e gestão operacional.
A solução é modular e escalável, podendo ser aplicada em diferentes tipos de pátios, desde operações logísticas até concessionárias.

Funcionalidades

Gestão de Motos: cadastro, atualização, listagem e exclusão

Gestão de Operações: registro de operações de motos

Banco MySQL: persistência de dados rodando em container no ACI
--
💼 Benefícios para o Negócio
--
Automação: elimina planilhas manuais e centraliza os dados

Visibilidade: status das motos em tempo real

Escalabilidade: possibilidade de subir múltiplas instâncias no Azure

Integração: API pronta para se integrar com sistemas externos
--
🏗️ Arquitetura da Solução
--
Fluxo de funcionamento:

O código-fonte é versionado no GitHub.

Build de imagens Docker (API e Banco).

Push das imagens para o Azure Container Registry (ACR).

Deploy dos containers no Azure Container Instances (ACI).

Um container roda o MySQL.

Outro container roda a API Moto-Flow e se conecta ao banco.

O usuário acessa a API por meio do IP público do ACI.

<img width="1536" height="1024" alt="devops" src="https://github.com/user-attachments/assets/f771a597-c411-4a31-a6ef-d332437e2e4c" />

--
📌 Recursos criados via script:
--
Resource Group

Azure Container Registry (ACR)

Azure Container Instance (ACI) para MySQL

Azure Container Instance (ACI) para API
--
🚀 Passo a Passo para Deploy
--
Pré-requisitos

Azure CLI instalado

Docker instalado

Conta no Azure
--
🔗 Acesso à API
--
http://20.241.140.250:8080/api/motos
--
🎥 Demonstração
--
Deploy do banco (ACI – MySQL)

Deploy da API (ACI – Spring Boot)

Teste do CRUD no endpoint /api/motos
--
✅ Checklist de Entrega
--
 API Dockerizada

 Imagens publicadas no Azure Container Registry

 Containers rodando no Azure Container Instances

 Banco MySQL funcional no ACI

 API acessível por IP público

 CRUD funcionando

#CRIAR RESOURCE GROUP
```
az group create --name rg-sprint3 --location eastus
```
#CRIAR CONTAINER
```
az acr create --resource-group rg-sprint3 --name acrsprint3rm556999 --sku Standard --location eastus --public-network-enabled true --admin-enabled true
```
#PEGAR ENDEREÇO DO ACR
```
az acr show --name acrsprint3rm556999 --resource-group rg-sprint3 --query loginServer --output tsv
```
#PEGAR CREDENCIAIS DO ACR
```
az acr credential show --name acrsprint3rm556999 --resource-group rg-sprint3
```
#LOGAR
```
az acr login --name acrsprint3rm556999
```
#CLONAR
```
git clone https://github.com/cahAmaral/Moto-Flow.git
cd Moto-flow-main
```
#BUILDAR API
```
docker build -t moto-flow-main .

cd ..

git clone https://github.com/GustavoAmorim12/sql-motoflow.git
cd sql-motoflow

docker build -t sql-sprint3 .

cd ..
```
#PREPARANDO AS IMAGENS PARA O ACR
```
docker tag moto-flow-main acrsprint3rm556999.azurecr.io/moto-flow-main:v1
docker tag sql-motoflow acrsprint3rm556999.azurecr.io/sql-motoflow:v1
```
#ENVIANDO AS IMAGENS PARA O ACR
```
docker push acrsprint3rm556999.azurecr.io/moto-flow-main:v1
docker push acrsprint3rm556999.azurecr.io/sql-motoflow:v1
```
#CONFERIR IMAGENS NO ACR
```
az acr repository list --name acrsprint3rm556999 --output table
```
#CRIANDO CONTAINER NO BANCO
```
az container create --resource-group rg-sprint3 --name sql-motoflow --image acrsprint3rm556999.azurecr.io/sql-motoflow:v1 --cpu 1 --memory 1.5 --ports 3306 --os-type Linux --environment-variables MYSQL_ROOT_PASSWORD=senha123 MYSQL_DATABASE=sprint3db

"username": "acrsprint3rm556999"
"q2PcHsrvVkLx0HDnDWfqjBXWmQZsCGiHma69mqJugu+ACRDjCjsu" ou "7KTgEYkQ7o4XzNFB8jPbfPPTM3PuyGCzg3DP2oUpnh+ACRDfAruX"
```
#CRIANDO CONTAINER DA API
```
az container create --resource-group rg-sprint3 --name moto-flow-main --image acrsprint3rm556999.azurecr.io/moto-flow-main:v1 --cpu 1 --memory 1.5 --ports 8080 --os-type Linux --ip-address Public --environment-variables DB_HOST=4.156.197.7 DB_USER=root DB_PASSWORD=senha123 DB_NAME=sprint3db --registry-login-server acrsprint3rm556999.azurecr.io --registry-username acrsprint3rm556999 --registry-password q2PcHsrvVkLx0HDnDWfqjBXWmQZsCGiHma69mqJugu+ACRDjCjsu
```


CREATE

{
    "id": 6,
    "placa": "ABC1A28",
    "modelo": {
      "id": 2,
      "nome": "Mottu-Express",
      "tipo": "Scooter"
    }
}

{
    "id": 7,
    "placa": "ABC1A76",
    "modelo": {
      "id": 2,
      "nome": "Mottu-Express",
      "tipo": "Scooter"
    }
}
