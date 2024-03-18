Fonte de dados: https://www.kaggle.com/code/andremitri/pcd3-t1-ecommerce/notebook

# ENTREVISTA PTCGROUP

1. Arquitetura e Configuração:
Descreva a arquitetura geral de uma implementação típica do Databricks. Como você configuraria um ambiente Databricks para otimizar o desempenho e a escalabilidade?
    
    
    Na implementação do Databricks, a arquitetura geral consiste em três principais componentes: 
    
    - O Workspace, onde os usuários interagem com a plataforma;
    - O Clustering, onde o processamento efetivo ocorre em clusters Spark configurados e gerenciados pelo Databricks;
    - E o Serviço de Metadados e Gerenciamento, que gerencia automaticamente os serviços de metadados e fornece uma camada de gerenciamento para os clusters Spark.
    
    Para otimizar o desempenho e a escalabilidade, é crucial considerar o tamanho e o tipo adequado do cluster, configurar o particionamento dos dados de forma eficiente, ajustar as configurações de shuffle, utilizar o cache de dados quando apropriado, monitorar e otimizar continuamente o uso dos recursos, e utilizar bibliotecas e frameworks otimizados para o Spark, como o Delta Lake.
    
2. Apache Spark e Databricks:
Como o Databricks se integra ao Apache Spark? Quais são as principais vantagens do uso do Databricks em comparação com uma instalação padrão do Apache Spark?
    
    
    O Databricks se integra ao Apache Spark como uma plataforma de análise unificada que simplifica o uso e a administração do Spark. 
    
    Suas principais vantagens incluem uma interface de usuário amigável, gerenciamento automático de clusters, integração com serviços de nuvem e otimizações de desempenho, tornando-o mais fácil de usar e mais eficiente do que uma instalação padrão do Apache Spark.
    
3. Notebooks e Linguagens de Programação:
Explique como você usaria Notebooks no Databricks para criar e executar código. Além disso, como o Databricks suporta várias linguagens de programação, e como você decidiria qual linguagem usar em um projeto específico?
    
    Para usar Notebooks no Databricks, eu abriria um novo Notebook no ambiente Databricks e escolheria a linguagem de programação desejada (como Python, Scala, SQL, R, ou Java) para escrever o código. Em seguida, eu poderia executar o código em células individuais ou em todo o Notebook para analisar, visualizar ou processar dados.
    
    O Databricks suporta várias linguagens de programação, então a escolha da linguagem depende do objetivo do projeto e da familiaridade da equipe com a linguagem. 
    
    Por exemplo, Python é frequentemente usado para análise de dados devido à sua ampla gama de bibliotecas, enquanto Scala pode ser preferido para processamento de dados em larga escala devido à sua eficiência no Spark. 
    
    A escolha da linguagem também pode depender da integração com outros sistemas ou serviços da nuvem. Em um projeto específico, eu decidiria a linguagem com base nessas considerações, buscando a que melhor atenda aos requisitos e às habilidades da equipe.
    
4. Integração de Fontes de Dados:
Como o Databricks facilita a integração com diferentes fontes de dados, como Data Lakes, bancos de dados relacionais e fontes externas? Você pode fornecer um exemplo prático de como lidar com essas integrações?
    
    O Databricks facilita a integração com diferentes fontes de dados, como Data Lakes, bancos de dados relacionais e fontes externas, por meio de suas bibliotecas e APIs integradas. Por exemplo, ele oferece integração nativa com serviços de armazenamento em nuvem, suporte a drivers JDBC/ODBC para bancos de dados relacionais e APIs para integração com serviços de terceiros. Um exemplo prático seria a leitura de dados de um Data Lake (como AWS S3) para realizar análises no Databricks, usando a API do Spark para ler os dados do S3 em um DataFrame e realizar transformações e análises nos dados.
    
    - LEITURA DE UM BUCKET S3
        
        ```python
        AWS_BUCKET_NAME = "fia-datasets"
        MOUNT_NAME = "datasets"
        
        dbutils.fs.mount(f"s3a://{AWS_BUCKET_NAME}", f"/mnt/{MOUNT_NAME}")
        ```
        
    - LEITUA DE UM STORAGE AZURE
        
        ```python
        dbutils.fs.mount(
          source = "wasbs://bs-stg-files@batista.blob.core.windows.net",
          mount_point = "/mnt/bs-stg-files",
          extra_configs = {"fs.azure.account.key.batista.blob.core.windows.net":dbutils.secrets.get(scope = "az-blob-storage-batista", key = "key-az-blob-storage")})
        ```
        
    - Via Streaming
    
    ```python
    df = (spark.readStream.format("cloudFiles")
          .option("cloudFiles.format", "json")
          .load(input_data_path))
    
    (df.writeStream.format("delta")
       .trigger(once=True)
       .option("checkpointLocation", chkpt_path)
       .table("iot_stream"))
    ```
    
5. Machine Learning no Databricks:
Descreva a abordagem que você seguiria para desenvolver e treinar modelos de machine learning no Databricks. Quais são as principais ferramentas e bibliotecas que você usaria para esse fim?
    
    Para desenvolver e treinar modelos de machine learning no Databricks, eu utilizaria a plataforma para explorar e preparar os dados, escolheria algoritmos adequados e treinaria os modelos usando bibliotecas como **`pyspark.ml`**. 
    
    Em seguida, avaliaria e otimizaria os modelos antes de implantá-los. As principais ferramentas seriam o Apache Spark, MLlib e **`pyspark.ml`**, além do uso opcional do scikit-learn para desenvolvimento local.
    
6. Segurança e Controle de Acesso:
Como o Databricks aborda questões de segurança e controle de acesso? Quais são as práticas recomendadas para garantir a proteção dos dados e ambientes de desenvolvimento?
    
    O Databricks aborda questões de segurança e controle de acesso por meio de recursos como autenticação e autorização granular, criptografia de dados em repouso e em movimento, gerenciamento de chaves, monitoramento e auditoria, e isolamento de ambientes. É importante seguir práticas recomendadas, como o princípio do menor privilégio e o uso de senhas fortes, para garantir a segurança dos dados e ambientes de desenvolvimento.
    
7. Desafios e Soluções:
Pergunta: Conte-nos sobre um desafio específico que você enfrentou ao trabalhar com Databricks e como o resolveu. Qual foi a solução implementada e quais foram os resultados alcançados?
    
    
    Desafio iniciar um projeto na metade o qual não estava progredindo bem, esta sem goernança e auditoria dos dados.
    
    Muitas vezes para ter um ambiente de trabalho para desenvolver era necessario fazer export do Banco produtivo para sua propria maquina.
    
    Com isso comprometendo a segurança e informação dos dados.
    
    Criei um ambiente de desenvolvimento no proprio databricks utilizando o REPOS com controle de versão dos notebooks onde todos os devs iam ate um repositorio padrão coletar os dados e realizar as atividades.
    
    Desafio: Iniciar um projeto na metade que estava com dificuldades, sem governança e auditoria dos dados. Frequentemente, era necessário exportar dados do banco de produção para a máquina local, comprometendo a segurança e integridade dos dados.
    
    Solução: Para resolver esses problemas, criei um ambiente de desenvolvimento no Databricks utilizando o REPOS com controle de versão dos notebooks. Todos os desenvolvedores acessavam um repositório padrão para coletar os dados e realizar as atividades, garantindo a segurança e integridade dos dados.
    
    Resultado: Com a implementação do ambiente de desenvolvimento no Databricks, foi possível melhorar a governança e auditoria dos dados, evitando a necessidade de exportar dados sensíveis para máquinas locais. Isso aumentou a segurança e eficiência do projeto, permitindo que os desenvolvedores trabalhassem de forma mais colaborativa e segura.
