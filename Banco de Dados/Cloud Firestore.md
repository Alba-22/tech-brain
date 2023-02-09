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
1. Documentos tem limites:
	- Um documento pode ter até 1MB de tamanho.
	- No máximo 20.000 campos(o que inclui campos dentro de maps).
	- Limite de uma escrita por segundo em um único documento(podem ocorrer falhas na escrita).
2. Não é possível retornar um documento parcialmente, todos os campos dele serão retornados numa leitura.
3. Queries são "rasas", ou seja, subcoleções de um documento não são retornadas na leitura de um documento.
4. A cobrança é feita pelo número de leituras e escritas feitas.
5. Arrays podem ser problemáticos em casos de alterações em simultâneo. Portanto, não é possível modificar, adicionar ou deletar um elemento de um array em posições específicas e nem buscar um valor em uma posição específica. O uso de array deve ser por meio de "contains".

*Dica:* se o esperado é buscar por registros individuais de um grupo de dados, é melhor colocados em uma coleção, seja ela uma coleção raiz ou subcoleção de outra.

## Como estruturar os dados
Há sempre algumas abordagens a se considerar na modelagem:
1. Colocar tudo em um único documento
	- Pode chegar no limite de 1MB/20.000 campos se tiver listas/maps grandes
	- Traz mais dados que o necessário de uma vez
	- Útil quando se deseja ter alguns dados prontos para serem acessados com o documento 
2. Criar uma subcoleção
	- Divide melhor os dados de forma hierárquica
	- Evita requisições muito grandes
	- Podem ser mais custosos para atualizar com Cloud Functions
	- Não necessita de indíce composto para algumas queries
	- Mais fácil de escrever regras de segurança
	- Melhor em casos que uma coleção depende muito de outra(uso de collectionGroup)
3. Criar uma nova coleção raiz
	- Irá ter os IDs de outras coleções que referencia
	- Bom para busca de leitura e busca de atualização com Cloud Functions
	- Pior em casos de queries de múltiplos campos, que requerem a criação de índices compostos
	- Melhor em casos que duas coleções não se relacionam tanto
4. Uso de arrays
	- Bom para casos de flags em que se precisa obter se um valor está contido na lista
	- Ruim em casos de flags se for necessário obter mais de um valor da lista
5. Gerenciamento de permissões:
	- Como não é possível retornar um documento parcial, não é ideal colocar num documento que vai ser lido pela aplicação cliente 
	- Associar ID com a permissão ajuda na hora de escrever as regras de segurança
	- Pode-se criar subcoleções para permissões(mas muito trabalhoso)
	- Criar uma subcoleção privada com alguns campos que escondam permissões, relatórios, etc.
6. No geral, se perguntar se será possível fazer queries para tudo que é necessário com aquela modelagem, já que o uso de indíces do NoSQL traz algumas limitações quando às queries que podem ser feitas.
7. Sempre se perguntar se será possível fazer leituras boas pelas Cloud Functions para manter as desnormalizações sincronizadas.
Em suma, veja o vídeo abaixo para vários exemplos:
https://youtu.be/haMOUb3KVSo?list=PLl-K7zZEsYLluG5MCVEzXAQ7ACZBCuZgZ

## Regras de Segurança
Filosofia: nunca confiar que a aplicação cliente está fazendo as coisas certo.
As regras de segurança se aplicam aos documentos de coleções.
Como a hierárquia de coleções pode ser muito grande, é possível fazer regras aninhadas. 
As regras de segurança de uma coleção pai não se aplicam à coleção filho
#### Wildcards
- Em {}: wildcard de elemento único
![](_assets/Pasted%20image%2020230205200450.png)
- \*\*: resto do caminho do documento, ou seja, aplica a regra a todos os documentos da coleção e a todos os documentos das subcoleções. Regras escritas com essa wildcard sobrepoem a outras regras que afetam as subcoleções
![](_assets/Pasted%20image%2020230205200744.png)

#### Ações
É necessário prover um valor booleano para cada ação específica. As ações são:
- **GET**: quando o usuário requisita o documento específico
- **LIST**: quando o usuário requisita uma query que pode retornar um documento específico
- **CREATE**: criar um novo documento
- **DELETE**: apagar um documento específico
- **UPDATE**: atualizar um documento que já existe
- **READ**: agrupa *GET* e *LIST*
- **WRITE**: agrupa *CREATE*, *DELETE* e *UPDATE*

#### Tomando decisão
A lógica para decidir a se um conjunto de documentos vai estar visível ao usuário requisitante depende de 3 fatores:
- A requisição de dados
- O documento no banco que está sendo lido/modificado
- Algum outro documento localizado no banco
Na requisição, pode-se olhar 2 propriedades:
- **Auth Object**: obtêm-se informações sobre o usuário logado(considerando o uso de FirebaseAuth). Geralmente pode-se confiar no usuário logado. Verificações:
```
request.auth != null
request.auth.uid
request.auth.token.email(verificar que apenas usuário de um determinado domínio conseguem acessar os dados)
request.auth.token.email.matches('.*google[.]com$') && request.auth.toekn.email_verified;
```
- **Resource Object**: o campo `data` possui todos os campos do documento que o usuário está tentando escrever
```
request.resource.data.score
request.resource.data["score"]
```
