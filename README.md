# Criação de Banco de Dados com DynamoDB e AWS CLI
- Criação do Banco de Dados
- Incluindo arquivos JSON no BD
- Criação de Index para Queries mais complexas
- Uso de Queries para extração de dados


## Criar uma tabela
    aws dynamodb create-table \
        --table-name Musica \
        --attribute-definitions \
            AttributeName=Artista,AttributeType=S \
            AttributeName=TituloMusica,AttributeType=S \
        --key-schema \
            AttributeName=Artista,KeyType=HASH \
            AttributeName=TituloMusica,KeyType=RANGE \
        --provisioned-throughput \
            ReadCapacityUnits=10,WriteCapacityUnits=5


## Inserir um item
    aws dynamodb put-item \
        --table-name Musica \
        --item file://itemmusic.json \


## Inserir múltiplos itens
    aws dynamodb batch-write-item \
        --request-items file://batchmusic.json


