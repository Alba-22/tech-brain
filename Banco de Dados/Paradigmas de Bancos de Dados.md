## Key-Value
*Exemplos:* Redis, Memcache.
Armazena dados numa estrutura chave e valor, assim como um JSON, Map, Dicionário, etc.
Os dados estão na memória RAM, fazendo com que o espaço seja bem limitado, porém o acesso/escrita de dados é extremamente rápido. Além disso, não há como fazer queries, fazendo com que possam ser retornados apenas dados únicos.
Extremamente útil para **cache**, filas de mensagens, pub/sub e criação de leaderboards.
![[Pasted image 20230108140427.png]]

## Wide-Column
*Exemplos:* Cassandra, Apache HBase.
É um tipo de banco no qual os nome e formatos da colunas podem variar nas linhas, mesmo dentro de uma mesma tabela. Como os dados são armazenados em colunas, queries de um determinado valor na colunas são bem rápidos, já que a coluna inteira pode ser carregada e pesquisada rapidamente.
Nisso que vem o conceito desse tipo de banco ser de família de colunas.
Usa-se CQL(Cassandra Query Language) para fazer queries no banco. Como vantagem, não possui schema, porém também não consegue realizar joins. Além disso, é fácil de escalar e fácil de replicar os dados em múltiplos nós.
Útil para escalar uma grande quantidade de registros ordenados por tempo e também para situações de muitas escritas e poucas leituras
![[Pasted image 20230108140456.png]]
## Documentos
*Exemplos:* MongoDB, Cloud Firestore, DynamoDB.
Nesse paradigma temos documentos, e cada documento é um container para pares chave-valor. Os documentos são desestruturados e não requerem um schema.
Documentos são agrupados em coleções. Campos em uma coleção podem ser indexados e coleções podem ser organizadas em uma hierárquia. Joins não são suportados, sendo comum a desnormalização de dados. 
Com isso, leituras conseguem ser bem rápidas, já que todos os dados já estão no documento ou em uma sub-coleção. Porém, escrita e atualização de dados acaba sendo mais complexo. Por exemplo, em um caso em que há muitos dados desconectados, porém relacionados, e que são atualizados com muita frequencia, pode não ser uma boa usar um banco orientado a documentos
Costumam ser usadas para aplicações mobile e IoT, sendo muito usadas pelo fato de serem de uso geral e serem fáceis de usar do ponto de vista do desenvolvedor
![[Pasted image 20230108140518.png]]

## Relacional
