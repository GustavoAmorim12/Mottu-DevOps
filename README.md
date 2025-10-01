"# Mottu-DevOps" 

#CRIAR RESOURCE GROUP

az group create --name rg-sprint3 --location eastus

#CRIAR CONTAINER

az acr create --resource-group rg-sprint3 --name acrsprint3rm556999 --sku Standard --location eastus --public-network-enabled true --admin-enabled true

#PEGAR ENDEREÇO DO ACR

az acr show --name acrsprint3rm556999 --resource-group rg-sprint3 --query loginServer --output tsv

#PEGAR CREDENCIAIS DO ACR

az acr credential show --name acrsprint3rm556999 --resource-group rg-sprint3

#LOGAR

az acr login --name acrsprint3rm556999

#CLONAR

git clone https://github.com/cahAmaral/Moto-Flow.git
cd Moto-flow-main

#BUILDAR API

docker build -t moto-flow-main .

cd ..

git clone https://github.com/GustavoAmorim12/sql-motoflow.git
cd sql-motoflow

docker build -t sql-sprint3 .

cd ..

#PREPARANDO AS IMAGENS PARA O ACR

docker tag moto-flow-main acrsprint3rm556999.azurecr.io/moto-flow-main:v1
docker tag sql-motoflow acrsprint3rm556999.azurecr.io/sql-motoflow:v1

#ENVIANDO AS IMAGENS PARA O ACR

docker push acrsprint3rm556999.azurecr.io/moto-flow-main:v1
docker push acrsprint3rm556999.azurecr.io/sql-motoflow:v1
s
#CONFERIR IMAGENS NO ACR

az acr repository list --name acrsprint3rm556999 --output table

#CRIANDO CONTAINER NO BANCO

az container create --resource-group rg-sprint3 --name sql-motoflow --image acrsprint3rm556999.azurecr.io/sql-motoflow:v1 --cpu 1 --memory 1.5 --ports 3306 --os-type Linux --environment-variables MYSQL_ROOT_PASSWORD=senha123 MYSQL_DATABASE=sprint3db

"username": "acrsprint3rm556999"
"q2PcHsrvVkLx0HDnDWfqjBXWmQZsCGiHma69mqJugu+ACRDjCjsu" ou "7KTgEYkQ7o4XzNFB8jPbfPPTM3PuyGCzg3DP2oUpnh+ACRDfAruX"

#CRIANDO CONTAINER DA API

az container create --resource-group rg-sprint3 --name moto-flow-main --image acrsprint3rm556999.azurecr.io/moto-flow-main:v1 --cpu 1 --memory 1.5 --ports 8080 --os-type Linux --ip-address Public --environment-variables DB_HOST=4.156.197.7 DB_USER=root DB_PASSWORD=senha123 DB_NAME=sprint3db --registry-login-server acrsprint3rm556999.azurecr.io --registry-username acrsprint3rm556999 --registry-password q2PcHsrvVkLx0HDnDWfqjBXWmQZsCGiHma69mqJugu+ACRDjCjsu

http://20.241.140.250:8080/api/motos

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
