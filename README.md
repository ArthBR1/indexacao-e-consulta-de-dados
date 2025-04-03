# Azure Cognitive Search: Utilizando AI Search para Indexação e Consulta de Dados

Este projeto demonstra como configurar e utilizar o Azure AI Search para indexar e consultar dados de forma eficiente. O Azure AI Search é um serviço de pesquisa em nuvem que fornece infraestrutura e APIs para criar experiências de pesquisa avançadas.

## Pré-requisitos

- Conta do Azure
- Conhecimento básico de JSON e APIs REST

## Passo a Passo para Configuração

### 1. Criar um serviço de Azure AI Search

Exemplo:

```bash
# Login no Azure
az login

# Criar grupo de recursos
az group create --name meu-grupo-recursos --location eastus

# Criar serviço de Azure AI Search
az search service create --name meu-servico-search \
                         --resource-group meu-grupo-recursos \
                         --location eastus \
                         --sku basic
```

### 2. Criar um índice de pesquisa

Exemplo:

Crie um arquivo `index-definition.json` com a seguinte estrutura:

```json
{
  "name": "meu-indice-produtos",
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": false
    },
    {
      "name": "nome",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "sortable": true
    },
    {
      "name": "descricao",
      "type": "Edm.String",
      "searchable": true,
      "analyzer": "pt.microsoft"
    },
    {
      "name": "preco",
      "type": "Edm.Double",
      "filterable": true,
      "sortable": true
    },
    {
      "name": "categoria",
      "type": "Edm.String",
      "searchable": true,
      "filterable": true,
      "facetable": true
    }
  ]
}
```

Criando o índice usando a API REST:

```bash
curl -X POST 'https://meu-servico-search.search.windows.net/indexes?api-version=2023-11-01' \
-H 'Content-Type: application/json' \
-H 'api-key: SUA_CHAVE_API' \
-d '@index-definition.json'
```

### 3. Carregar dados no índice

Exemplo:

Criando um arquivo `documents.json` com os dados a serem indexados:

```json
{
  "value": [
    {
      "id": "1",
      "nome": "Smartphone Premium",
      "descricao": "Smartphone com câmera de alta resolução e bateria de longa duração",
      "preco": 2999.99,
      "categoria": "eletronicos"
    },
    {
      "id": "2",
      "nome": "Notebook Ultrafino",
      "descricao": "Notebook leve com processador rápido e SSD de 512GB",
      "preco": 4599.99,
      "categoria": "eletronicos"
    }
  ]
}
```

Carregando os documentos no índice:

```bash
curl -X POST 'https://meu-servico-search.search.windows.net/indexes/meu-indice-produtos/docs/index?api-version=2023-11-01' \
-H 'Content-Type: application/json' \
-H 'api-key: SUA_CHAVE_API' \
-d '@documents.json'
```

### 4. Realizar consultas no índice

Exemplo de consulta simples:

```bash
curl -X GET 'https://meu-servico-search.search.windows.net/indexes/meu-indice-produtos/docs?search=notebook&api-version=2023-11-01' \
-H 'api-key: SUA_CHAVE_API'
```

## Insights e Aprendizados

1. **Personalização**: O Azure AI Search permite configurar analisadores de linguagem específicos (como "pt.microsoft" para português), melhorando a qualidade dos resultados para idiomas específicos.

2. **Facetas**: O uso de facetas (como a categoria no exemplo) permite criar experiências de filtragem avançadas em aplicações de e-commerce.

3. **Performance**: Para grandes volumes de dados, é importante considerar a estratégia de indexação em lotes e o uso de indexadores para fontes de dados como Azure SQL, Cosmos DB ou Blob Storage.

4. **Segurança**: O serviço oferece integração com Azure Active Directory e controle de acesso em nível de documento.

## Possibilidades de Aplicação

1. **E-commerce**: Busca de produtos com filtros avançados e sugestões de pesquisa.
2. **Sistemas de Conteúdo**: Pesquisa em grandes volumes de documentos e artigos.
3. **Aplicações Empresariais**: Busca unificada em múltiplas fontes de dados corporativos.
4. **Bancos de Conhecimento**: Sistemas de perguntas e respostas baseados em documentos indexados.

## Melhorias Futuras

1. Integrar habilidades cognitivas para enriquecimento de dados (extração de entidades, análise de sentimentos).
2. Implementar autocomplete/sugestões de pesquisa.
3. Adicionar pontuação personalizada para priorizar certos resultados.
4. Configurar sinônimos para melhorar a recall das buscas.

## Conclusão

O Azure AI Search oferece uma plataforma poderosa para implementar funcionalidades de busca em aplicações, com diversas opções de personalização e integração. Este projeto demonstra o fluxo básico de criação de índice, carregamento de dados e consulta, servindo como ponto de partida para implementações mais complexas.
