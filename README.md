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


## Criar um index global secundário baeado no título do álbum
    aws dynamodb update-table \
        --table-name Musica \
        --attribute-definitions AttributeName=TituloAlbum,AttributeType=S \
        --global-secondary-index-updates \
            "[{\"Create\":{\"IndexName\": \"TituloAlbum-index\",\"KeySchema\":[{\"AttributeName\":\"TituloAlbum\",\"KeyType\":\"HASH\"}], \
            \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"


## Criar um index global secundário baseado no nome do artista e no título do álbum
    aws dynamodb update-table \
        --table-name Musica \
        --attribute-definitions\
            AttributeName=Artista,AttributeType=S \
            AttributeName=TituloAlbum,AttributeType=S \
        --global-secondary-index-updates \
            "[{\"Create\":{\"IndexName\": \"ArtistaTituloAlbum-index\",\"KeySchema\":[{\"AttributeName\":\"Artista\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"TituloAlbum\",\"KeyType\":\"RANGE\"}], \
            \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"


## Criar um index global secundário baseado no título da música e no ano
    aws dynamodb update-table \
        --table-name Musica \
        --attribute-definitions\
            AttributeName=TituloMusica,AttributeType=S \
            AttributeName=AnoMusica,AttributeType=S \
        --global-secondary-index-updates \
            "[{\"Create\":{\"IndexName\": \"TituloMusicaAno-index\",\"KeySchema\":[{\"AttributeName\":\"TituloMusica\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"AnoMusica\",\"KeyType\":\"RANGE\"}], \
            \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"


