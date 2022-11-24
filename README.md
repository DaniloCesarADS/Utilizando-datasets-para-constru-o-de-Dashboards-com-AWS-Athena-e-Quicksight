# Realizando consultas em Banco de dados SQL e visualização de Dashboards com Amazon Athena e Quicksight

Projeto e implementação de banco de dados em ambiente Cloud utilizando o serviço Amazon Athena, com visualização de dados e dashboards por meio do serviço Amazon Quicksight.



## Cenário de aplicação

Dados demográficos do Brasil no ano de 2019.



### Serviços utilizados nessa atividade prática
 - Amazon S3
 - Amazon Glue
 - Amazon Athena
 - Amazon QuickSight



## Arquitetura da aplicação

* O repositório criado no Amazon S3 hospeda o csv que contém os dados brutos;
* O objeto Amazon Glue é responsável por obter os dados e gerar o esquema de dados à partir do csv contido no bucket S3, que será utilizado no serviço Athena para a criação da tabela;
* O Amazon Athena realiza as consultas SQL á partir da base de dados gerada no serviço utilizando-se do Crawler gerado via Amazon Glue;
* O serviço Amazon Quicksight pega a base de dados e gera as plotagens customizadas para dashboards.

### Etapas para desenvolvimento

### Criação do bucket (Cloud repository) no serviço de repositório Amazon S3

- Amazon S3 Console -> Buckets -> Create bucket -> Bucket name [nome_do bucket("dio-bucket-dados-athena")] - AWS Region (Usar a tua região) - Create bucket
- Create folder (Criar uma pasta chamada ```/output``` e outra com o nome do seu conjunto de dados ("population"). Este nome irá definir o nome da tabela criada no Glue)
- Upload dos arquivos de dados localizados na pasta ```/data``` (o csv) para a pasta ``/population``  do bucket.
- A pasta ``/output`` recebe as queries realizadas na utiilização do BD.

#### Criação do Glue Crawler para captura de dados do csv e geração do esquema de dados

- Amazon Glue Console -> Crawlers -> Create Crawler -> Name ("PopulationDBCrawler") -> Next
- Source type [Data Stores] -> Crawl all folders
- Data store [S3] -> Include path [caminho do diretório dos dados de entrada]
- Create IAM Role [nome da role-population]
- Frequency [Run on demand]
- Database name [seu_nome_de_db]
- Advanced options -> Group behavior [Create a single schema for each S3 path]
- Finish
- Databases -> Tables -> Visualizar dados das tabelas criadas

### Criar aplicação no Amazon Athena

- Query editor -> Settings -> Manage settings -> Query result location and encryption -> Browse S3 -> selecionar o bucket criado
- Selecionar Database -> criar queries (exemplos na pasta ```/src```)
- Verificar queries não salvas no bucket criado no S3
- Salvar queries -> Executar novamente -> Verificar no bucket criado no S3, pasta ``/output/`` .

#### Criando nova tabela

- Query Editor -> Tables -> Generate table DDL
- Copiar a query gerada
- Selecionar o DB e criar a nova tabela em uma nova query

### Visualizar dados no Amazon QuickSight

- Signup (caso não tenha conta) -> Escolher [Standard] -> Escolher a instância do S3
- Datasets -> Create new dataset -> Athena -> Name [NomeDoDataSet] -> Create
- Select database -> select table -> Edit or preview -> Save & visualize
- Criar visualizações selecionando colunas, criando filtros e parâmetros e selecionando Visual types para gráficos.

### Eliminar recursos
 - Excluir os elementos criados

