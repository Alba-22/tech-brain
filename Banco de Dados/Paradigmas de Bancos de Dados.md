#database
## Bancos Chave-Valor
*Exemplos:* Redis, Memcache.
Armazena dados numa estrutura chave e valor, assim como um JSON, Map, Dicionário, etc.
Os dados estão na memória RAM, fazendo com que o espaço seja bem limitado, porém o acesso/escrita de dados é extremamente rápido. Além disso, não há como fazer queries, fazendo com que possam ser retornados apenas dados únicos.
Extremamente útil para **cache**, filas de mensagens, pub/sub e criação de leaderboards.

![](/_assets/Pasted%20image%2020230108224638.png)

## Bancos Wide-Column
*Exemplos:* Cassandra, Apache HBase.
É um tipo de banco no qual os nome e formatos da colunas podem variar nas linhas, mesmo dentro de uma mesma tabela. Como os dados são armazenados em colunas, queries de um determinado valor na colunas são bem rápidos, já que a coluna inteira pode ser carregada e pesquisada rapidamente.
Nisso que vem o conceito desse tipo de banco ser de família de colunas.
Usa-se CQL(Cassandra Query Language) para fazer queries no banco. Como vantagem, não possui schema, porém também não consegue realizar joins. Além disso, é fácil de escalar e fácil de replicar os dados em múltiplos nós.
Útil para escalar uma grande quantidade de registros ordenados por tempo e também para situações de muitas escritas e poucas leituras.

![](/_assets/Pasted%20image%2020230108224656.png)

## Bancos de Documentos
*Exemplos:* MongoDB, Cloud Firestore, DynamoDB.
Nesse paradigma temos documentos, e cada documento é um container para pares chave-valor. Os documentos são desestruturados e não requerem um schema.
Documentos são agrupados em coleções. Campos em uma coleção podem ser indexados e coleções podem ser organizadas em uma hierárquia. Joins não são suportados, sendo comum a desnormalização de dados. 
Com isso, leituras conseguem ser bem rápidas, já que todos os dados já estão no documento ou em uma sub-coleção. Porém, escrita e atualização de dados acaba sendo mais complexo. Por exemplo, em um caso em que há muitos dados desconectados, porém relacionados, e que são atualizados com muita frequencia, pode não ser uma boa usar um banco orientado a documentos.
Costumam ser usadas para aplicações mobile e IoT, sendo muito usadas pelo fato de serem de uso geral e serem fáceis de usar do ponto de vista do desenvolvedor.

![](/_assets/Pasted%20image%2020230108224706.png)

## Bancos Relacionais
*Exemplos:* MySQL, PostgreSQL, SQLite, SQL Server.
A organização dos dados é feita em tabelas, em que cada linha é um registro e cada coluna representa um dados diferente de um registro. Em toda tabela, uma das colunas será a *Primary Key*, que será responsável por identificar unicamente um registro. 
Além disso, outras coluna podem ser marcadas como *Foreign Keys*, que irão referenciar Primary Keys de outras tabelas. Com isso é possível fazer Join de duas ou mais tabela e unir dados diferentes em novos registros.
Utilizando o SQL(Structured Query Language) para manipulação dos dados, os bancos relacionais exigem um schema para cada tabela, de forma que as colunas e seus tipos devem ser definidos a priori.
Os bancos relacionais também tem como base o conceito de ACID:
- Atomicidade
- Consistência
- Isolação
- Durabilidade

![](/_assets/Pasted%20image%2020230108224718.png)

## Bancos de Grafos
*Exemplo:* neo4j
Nesse tipo de banco, os dados são representados como nós e as relações entre os dados são representados como arestas. Assim, em vez de criar-se uma tabela intermediária(em bancos relacionais), basta criar uma aresta ligando 2 dados que se relacionam.
As queries são feitas por meio do Cypher, que possui statements mais concisos e legiveis, além de ter mais performance(especialmente em grandes conjuntos de dados).
São utilizados em aplicaçõeas de detecção de fraudes, para grafos de conhecimento e engines de recomendação de conteúdo.

![](/_assets/Pasted%20image%2020230108224728.png)

## Bancos de Busca
*Exemplos:* Elastic Search, Algolia, MeiliSearch.
Engines de busca de texto. Do ponto de vista de uso, elas são bem semelhantes aos bancos de documentos, em que a partir de um índice são adicionados vários objetos de dados.
A diferença é que um banco de busca vai analisar todo o texto do documento e criar um índice de termos buscáveis, fazendo com que uma busca seja extremamente rápida, mesmo pra um banco com muitos registros. 
O banco também pode executar vários algoritmos para ordernar os resultados das buscas. Porém isso pode fazer com que seja um pouco mais difícil de escalar, apesar de adicionar muito valor à experiência do usuário.

![](/_assets/Pasted%20image%2020230108224455.png)


## Banco Multi-Modelo
*Exemplos:* FaunaDB, CosmosDB.
Usa-se GraphQL para descrever-se como se deseja acessar os dados. Cria collections que armazenam dados em um indice para que se possar fazer queries. Por de baixo dos panos, determina como usar melhor os diversos paradigmas a partir do código GraphQL provido.
![](_assets/Pasted%20image%2020230108224820.png)

## Data Warehouses


## Time-series