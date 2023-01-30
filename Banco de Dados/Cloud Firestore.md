#database 

## Introdução
O banco de dados do Cloud Firestore é um NoSQL de Documentos, ou seja, em vez dos dados serem guardados em tabelas que se relacionam umas com as outras por meio de chaves primárias e secundárias, os dados são armazenados em coleções de documentos, sendo um documento uma notação chave-valor.

Além disso, o Cloud Firestore não possui nenhum schema de como os dados são representados em cada documentos, o que significa que um documento pode ter uma estrutura completamente diferente de outro documento. Para evitar possiveis problemas de receber dados não esperados ou ter a falta de dados esperados, é ideal práticas de [[Programação Defensiva]].

A ideia principal do NoSQL é de não ter que acessar lugares diferentes para acessar dados. Em um modelo SQL por vezes é necessário fazer **JOIN** de várias tabelas diferentes para obter os dados, já que a ideia é que os dados estejam normalizados. Já no NoSQL de documentos, é ideia é justamente oposta. 

O *modus operandi* é ter os dados desnormalizados, ou seja, repetidos em várias coleções, para que quando for necessário acessar uma determinada informação, todos os dados já sejam retornados diretamente, sem terem que ser buscados em outro lugar. Porém, em caso de haver mudança nos dados, é necessário atualizá-lo em todos os outro lugares que o utilizam. Assim, esse modelo se mostra **bem** efetivo em situações em que a quantidade de leituras é muito maior que as de escritas.

Uma grande vantagem de bancos não relacionais é a **escalabilidade horizontal**, que permite a distribuição de dados entre múltiplas máquinas mais facilmente. Para atingir uma boa escalabilidade com bancos relacionais, seria necessário colocar o servidor em uma máquina cada vez mais poderosa, o que é chamado de **escalabilidade vertical**.
![Escalabilidade Horizontal](_assets/Pasted%20image%2020230129193636.png)

No Cloud Firestore, os dados são organizados em documentos e coleções de forma hierárquica. Os documentos são similares a objetos JSON, guardando dados na forma chave-valor. As coleções são agrupados de objetos
![](_assets/Pasted%20image%2020230129193550.png)

#### Algumas regras ao usar o Cloud Firestore:
- Coleções só podem conter documentos;
- Documentos devem ser menores que 1MB;
- Um documento não pode conter outro documento;
- A raiz do Cloud Firestore só pode conter coleções.

O acesso aos dados sempre seguirá o padrão collection-document:
```
firestore.collection(...).document(...).collection(...).document(...)
```

#### Exemplo de Estrutura NoSQL(Documentos):
![](_assets/Pasted%20image%2020230129194646.png)

## Como queries funcionam no Cloud Firestore
O processo de obter dados em um banco de dados é chamado de *querying*. As queries no Cloud Firestore seguem algumas regras:
1. As queries só podem ser usadas para encontrar documentos em uma coleção/subcoleção específica(desmentido logo a abaixo)
2. Só é possível fazer queries em campos já existentes numa coleção, e não em um cálculo feito a partir de dois campos. 
3. Os resultados do Cloud Firestore são "rasos", ou seja, ao buscar os dados de uma coleção, não é obtido automaticamente os dados das subcoleções, e sim apenas os dados dos campos dos documentos em si. Caso se deseje obter todos os dados(campos + subcoleções) seria necessário fazer múltiplas requisições ou não quebrar as informações em subcoleções.

#### Trabalhando com índices
É possível fazer buscas em várias subcoleções de uma coleção, processo esse chamado de **Collection Group Query**. Para habilitar esse tipo de query, é necessário entrar no Console do Firebase e criar um índice para indicar exatamente qual campo que se deseja buscar em uma dada coleção.

Buscando detro da collection *reviews* pelo campo *rating*: 
![](_assets/Pasted%20image%2020230129200025.png)
Isso indicará para o Cloud Firestore indexar todos os documentos em todas as coleções chamadas *reviews* como se fosse uma grande e única coleção.

No Cloud Firestore, o tempo necessário para rodar uma query é proporcional ao número de campos obtidos e não o número de documentos sendo procurados. Ou seja, o tempo pra rodar uma query em 60 documentos ou em 6000 será o mesmo.

Isso é possível por meio dos índices. Quando um documento é adicionado à uma coleção, o Cloud Firestore cria um índice para cada campo naquele documento. Cada entrada no índice guarda o valor do campo e onde o documento corresponde está no banco de dados. Esse funcionamento de índice também é válido para mapas dentro de um documento(até 20 aninhamentos). 

A criação desse índice faz com que as buscas no Firestore sejam muito rápidas e permite filtros de igualdade e inequalidade de forma fácil também. Porém, não é possível fazer buscas *full-text*, nem buscas com *ou(||)*, nem buscas com *!=*, nem buscas em documentos em que não há um determinado valor(já que não é possível indexar a inexistência de um valor).

Porém, é possível fazer queries filtrando por dois campos diferentes em casos de igualdade dos valores, o que é feito pelo algoritmo *zig-zag merge join*. Isso é possível pelo fato do Firestore ordenar os IDs dos documentos de forma ascendente para um mesmo valor do campo. Exemplo: busca em que: `address.city = "San Francisco"` e `cuisine = "Japanese"`:
![](_assets/Pasted%20image%2020230129204022.png)

Para resolver buscas filtradas em casos que algum dos campos sendo procurados seja numa relação de *maior ou igual* ou *menor ou igual* é necessária a criação de **índices compostos**, que devem ser criados manualmente no Console do Firebase. O que o Firestore fará é criar um índice que concatena os valores de uma forma que seja possível pesquisá-los por ordernação.

![](_assets/Pasted%20image%2020230129205309.png)

Porém, não é possível fazer um índice composto para duas inequalidades, já que não seria possível fazer a busca de forma adjacente no índice.
![](_assets/Pasted%20image%2020230129210321.png)
Apesar disso não funcionar para filtrar as duas inequalidades, é possível filtrar por uma delas e fazer ordenação pelas duas:
![](_assets/Pasted%20image%2020230129210504.png)

Assim, o principal ponto para a velocidade na obtenção de dados no Cloud Firestore é a criação de índices de forma que os resultados que se esteja procurando estejam adjacentes uns aos outros na tabela de índices.

## Listas e Maps vs Collections