# README: Ingestão de Dados e Upload para S3 de Solicitações de Serviço 311 de Boston

## Visão Geral
Este notebook demonstra um pipeline de ingestão de dados utilizando dados de solicitações de serviço 311 da cidade de Boston. 
O processo envolve o download de arquivos CSV do portal de dados abertos da Boston.gov, o processamento desses arquivos com Pandas e, posteriormente, o upload dos dados processados em formato Parquet para um bucket AWS S3.

## Etapas:

### 1. Configuração do Ambiente
- **Criação do Diretório de Dados**: Um diretório local chamado `data` é criado para armazenar os arquivos CSV baixados.
  ```python
  !mkdir -p data
  ```

- **Importação de Bibliotecas**: Bibliotecas essenciais como `requests`, `urllib.request`, `pandas`, `boto3` e `io.BytesIO` são importadas para manipulação de dados, requisições web e interação com o S3.

### 2. Extração de Dados
- **Função `extrair_dados`**: Uma função Python chamada `extrair_dados` é definida para realizar o download de arquivos CSV a partir de URLs específicas. A função gerencia requisições HTTP utilizando cabeçalhos apropriados e grava o conteúdo em arquivos locais.

- **Download dos Dados**: A função `extrair_dados` é chamada repetidamente para baixar os dados de solicitações de serviço 311 referentes aos anos de 2015 a 2020, salvando os arquivos como `dados_YYYY.csv` dentro do diretório `data`.

### 3. Carregamento e Consulta Inicial dos Dados
- **Lista de Arquivos**: É criada uma lista contendo os caminhos dos arquivos CSV baixados.

- **Carregamento em DataFrames**: Cada arquivo CSV é carregado em um DataFrame do pandas. Esses DataFrames são armazenados em um dicionário chamado `dfs`, utilizando o ano como chave (por exemplo, `dfs['2018']`).

- **Consulta Inicial dos Dados**: O método `.head()` é utilizado para exibir as primeiras linhas de um DataFrame de exemplo (como `dfs['2018']`) para inspecionar a estrutura e o conteúdo dos dados.

### 4. Integração com AWS S3
- **Instalação do `boto3`**: A biblioteca `boto3`, SDK da AWS para Python, é instalada para permitir a interação com o Amazon S3.

- **Configuração das Credenciais AWS**: Variáveis de exemplo para chave de acesso AWS, chave secreta e região são definidas. Essas variáveis devem ser substituídas por credenciais reais.

  ```python
  chave_acesso_aws = 'sua_chave'
  chave_acesso_secreta_aws = 'sua_chave_secreta'
  regiao_nome = 'us-east-1'
  ```

- **Inicialização do Cliente S3**: Um cliente S3 do `boto3` é inicializado utilizando as credenciais fornecidas.

### 5. Teste de Conexão com o S3
- **Criação de Arquivo de Teste**: Um arquivo de texto simples chamado `oi-s3.txt` é criado localmente contendo um conteúdo de teste.

- **Upload do Arquivo de Teste**: Esse arquivo é enviado para o bucket S3 especificado (`alura-data-lake-aws-1`) utilizando o prefixo `bronze/`.

- **Operações no S3 (Demonstração)**: São demonstradas operações básicas no S3:
    - Listagem de buckets.
    - Listagem de objetos dentro de um bucket específico.
    - Exclusão de objetos.
    - Download de objetos.

### 6. Transformação dos Dados e Upload para o S3 (Formato Parquet)
- **Conversão para Parquet**: O notebook percorre cada DataFrame armazenado no dicionário `dfs`.

- **Uso do `BytesIO`**: Para cada DataFrame, um objeto `BytesIO` é utilizado para armazenar temporariamente o DataFrame em formato Parquet na memória.

- **Upload dos Arquivos Parquet para o S3**: Os dados em formato Parquet armazenados em memória são enviados para o bucket S3 (`alura-data-lake-aws-1`) utilizando a chave `bronze/dados_YYYY.parquet`.

O uso do formato Parquet foi escolhido devido à sua eficiência em armazenamento e desempenho em consultas, especialmente em arquiteturas de Data Lake.

### 7. Verificação dos Uploads no S3
- **Listagem de Objetos**: Os objetos presentes no bucket S3 são listados novamente para confirmar que os novos arquivos Parquet foram enviados corretamente, juntamente com o arquivo de teste.

Este notebook fornece um exemplo fundamental de extração de datasets distintos, padronização e armazenamento em uma camada de Data Lake baseada em nuvem (zona `bronze`), utilizando um formato colunar (Parquet) para otimizar o processamento analítico posterior.
