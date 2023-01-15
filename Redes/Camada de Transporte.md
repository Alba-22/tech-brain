#network
## Introdução e Serviços
A camada de transporte fornece serviços de comunicação diretamente aos processos de aplicação que rodam em diferentes hospedeiros. Os protocolos da camada de transporte fornecem **comunicação lógica** entre os processos dos hospedeiros que estão se comunicando. Ou seja, ele faz com que na visão da camada de aplicação, esses hospedeiros estejam diretamente conectados.
A camada de transporte é implementada nos sistemas finais, de forma que ela converte as mensagens recebidas da aplicação em **segmentos da camada de transporte**. Na criação desses segmentos, divide-se as mensagens da camada de aplicação e adiciona-se um header a cada um deles.

![](/_assets/Pasted%20image%2020230114115723.png)

A camada de transporte, então, passa o segmento pra camada de rede do remetente, que encapsula o segmento em um datagrama e envia ao destinatário.
*OBS:* nesse processo, os roteadores que estiverem levando a mensagem não possuem a camada de transporte para examinar os campos do segmento.

### Relação Transporte e Rede
A camada de transporte fornece comunicação lógica entre **processos** que rodam nos hospedeiros e a camada de rede fornece comunicação lógica entre os **hospedeiros** em si.
Além disso, os serviços que um protocolo de transporte pode oferecer são muitas vezes limitados pelo modelo de serviço do protocolo da camada de rede. Se este não puder dar garantias contra atraso ou garantias de largura de banda, então o protocolo de camada de transporte também não poderá dar essas garantias para mensagens de aplicação enviadas entre processos. Porém, há alguns serviços que a camada de rede não provém que podem ser oferecidos pela camada de transporte.

### Visão Geral da Camada de Transporte
Existem 2 principais protocolos:
- **UDP:** User Datagram Protocol - serviço não confiável, não orientado a conexão.
- **TCP:** Transmission Control Protocol - serviço confiável, orientado a conexão.
A responsabilidade fundamental do UDP e TCP é ampliar o serviço de entre IP entre 2 sistemas finais para um serviço de entrega entre 2 **processos** que rodam nos sistemas finais. Essa ampliação é a **multiplexação/demultiplexação**.
Além disso, os dois protocolos fornecem **verificação de integridade** por incluirem campos de detecção de erros nos cabeçalhos de seus segmentos.
Adicionalmente, o TCP fornece a **transferência confiável de dados**(para os processos dos sistemas finais) e o **controle de congestionamento**(que serve a Internet como um todo).


---
## Multiplexação e Demultiplexação
É a ampliação do serviço de entrega hospedeiro/hospedeiro da camada de rede para um serviço de entrega processo/processo.
Na realidade, a camada de transporte não entrega os dados diretamente a um processo, mas sim a um socket intermediário, tendo cada socket um identificador exclusivo.

![](/_assets/Pasted%20image%2020230114125738.png)

**Demultiplexação:** é realizado na extremidade receptora, entregando os dados contidos em um segmento da camada de transporte ao socket correto.

**Multiplexação:** é realizado na extremidade de origem, reunindo os partes de dados provenientes de vários sockets, encapsulando essas partes em um header para criar segmentos e enviando os segmentos para a camada de rede.
Em relação à interação segmento/socket, temos que as portas(dos sockets) tem identificadores exclusivos e que cada segmento tem campos especiais que indicam a porta para qual o segmento deve ser entregue.

![](/_assets/Pasted%20image%2020230114131525.png)

Os campos especiais são: **campo de número de porta de origem** e **campo de número de porta de destino**. Cada número de porta é um valor de 16 bits entre 0 e 65535.
### Multiplexação/Demultiplexação não orientada a conexão
O socket UDP é identificado por uma tupla de 2 elementos:
```
(IP de destino, porta de destino)
```
Assim, se 2 segmentos UDP tiverem IP de origem e/ou porta de origem diferentes, mas o mesmo IP de destino e mesma porta de destino, eles serão direcionados para o mesmo processo de destino por meio do socket.

*Exemplo:* Enviando de um processo do hospedeiro A(cuja porta UDP é 19157) para um processo do hospedeiro B(cuja porta é 46428). No hospedeiro A, a camada de transporte cria o segmento com os dados, a porta de origem e a porta de destino e envia esse segmento pra camada de rede, que encapsulará isso em um datagrama IP e fará o melhor esforço possível pra entregar ao hospedeiro B.
Se o segmento chegar em B, a camada de transporte dele irá analisar a porta de destino(46428) e entregará ao socket com aquela porta.
O motivo de ser enviado junto a porta de origem, é para que se possa fazer uma comunicação "reversa" de B para A.

![](/_assets/Pasted%20image%2020230114133852.png)

### Multiplexação/Demultiplexação orientada a conexão
O socket TCP é identificado por uma tupla de quatro elementos:
```
(IP de origem, porta de origem, IP de destino, porta de destino)
```
Dessa forma, quando um segmento TCP vindo da rede chega a um hospedeiro, o hospedeiro usa os 4 valores para demultiplexar o segmento para o socket correto. Com isso, 2 segmentos TCP chegando com IPs de origem diferentes e/ou portas diferentes, serão direcionados para 2 sockets diferentes.

O hospedeiro servidor pode suportar vários sockets TCP simultâneos, sendo cada um ligado a um processo e identificado pela tupla de quatro elementos.

*Exemplo:* Um hospedeiro C inicia duas sessões HTTP com o servidor B e o hospedeiro A inicia uma sessão HTTP com o servidor B. Todos os elementos possuem seus próprios IPs. O hospedeiro C atribui 2 números diferentes a porta de origem de suas conexões HTTP. Caso A escolha um número de porta de origem igual a de C, o servidor B ainda será capaz de demultiplexar as duas conexões com mesmo número de porta de origem, já que elas tem endereço IP de origem diferentes.

![](/_assets/Pasted%20image%2020230114145446.png)

### Varredura de Portas
Uso do comando `nmap` para listar portas UDP e TCP em um dispositivo

---
## UDP: Transporte não orientado para conexão
O transporte não ser orientado para conexão quer dizer que não há apresentação entre as entidades remetente e destinatária da camada de transporte antes de enviar um segmento.
Algumas motivos para se preferir UDP em vez de TCP:
- Melhor controle sobre quais dados são enviados e quando: UDP é mais simples e mais enxuto. Se a aplicação precisar de algo além do serviço de entrega de segmentos simples do UDP, ela pode implementar.
- Não há estabelecimento de conexão: o UDP envia mensagens sem nenhuma preliminar formal, não possuindo atraso algum para estabelecer um conexão.
- Não há estados de conexão: TCP possui buffers de envio e recebimento, parâmetros de controle de congestionamento e parâmetros de sequência e reconhecimento. UDP não tem isso, assim consegue suporta um número muito maior de clientes ativos.
- Cabeçalho é menor

### Estrutura do segmento UDP
O UDP possui 4 campos, cada um possuindo 2 bytes:
- Os *números de porta* permitem que o hospedeiro destinatário passe os dados ao processo correto.
- O *comprimento* especifica o número de bytes total do segmento UDP
- O *checksum* é usada pelo hospedeiro receptor para verificação de erros.

![](/_assets/Pasted%20image%2020230115091018.png)

### Checksum do segmento UDP
É usada para determinar se bits dentro do segmento UDP foram alterados durante o trajeto da origem ao destino. O UDP no lado remetente realiza o complemento de 1 da soma de todas as palavras de 16 bits do segmento levando em conta o "vai um" em toda soma e coloca o resultado no campo *checksum*.

No lado do destinatário, todas as palavras de 16 bits são somadas(incluindo o checksum). Se não houverem errros, todos os bits do resultado serão 1. Se algum erro aconteceu, então haverá algum bit 0 no resultado.

Apesar de muitos protocolos da camada de enlace fazerem verificação de erros, o UDP também fornece esse serviço por não haver garantia de que todos os enlaces entre a origem e destino façam essa verificação. Além disso, quando um segmento é armazenado em um roteador, pode ocorrer a introdução de erros.

---
## Princípios da transferência confiável de dados
- Resumo dos mecanismo de transferência confiável de dados e sua utilização:
![](/_assets/Pasted%20image%2020230115093701.png)

---
## TCP: Transporte orientado para conexão
### Serviços do TCP
O TCP é **orientado para conexão** pois, antes que um processo de aplicação consiga enviar dados a outros, os dois processos precisam enviar alguns segmentos preliminares um a outro para estabelecer os parâmetros da transferência de dados. Nesse estabelecimento de conexão, ambas as partes iniciam muitas variáveis de estado. Além disso, a conexão TCP não é um circuito fim a fim, rodando apenas nos sistemas finais e não nos elementos intermediários(roteadores).

Uma conexão TCP também provê um **serviço full-duplex**, fazendo com que um processo A de um hospedeiro converse com o processo B de outro hospedeiro e de B para A ao mesmo tempo.

A conexão TCP sempre é **ponto a ponto**: sempre um único remetente e um único destinatário. Durante o estabelecimento de conexão, o cliente envia um segmento TCP especial para o servidor; o servidor responde com um segundo segmento TCP especial e, por fim, o cliente responde com um terceiro segmento(que pode conter dados). Esse procedimento é chamado de **3-way handshake**.

Após o estabelecimento da conexão, os dois processo começam a enviar dados um para o outro. Ao enviar os dados pelo socket, o TCP do lado do cliente irá direcionar os dados para o **buffer de envio**. Aos poucos, os dados vão sendo retirados do buffer e passados à camada de rede. A quantidade máxima de dados que pode ser manipulada em um segmento é limitado pelo **tamanho máximo do segmento(MSS)**, que é definido de acordo com o tamanhho do maior quadro de camada de enlace que pode ser enviado pelo hospedeiro remetente. No recebimento do lado do servidor, os dados do segmento são colocados no **buffer de recepção**.

![](/_assets/Pasted%20image%2020230115102632.png)

### Estrutura do segmento TCP
O segmento do TCP possui vários campos:
- Os *números de porta* usados para multiplexação/demultiplexação
- Campo de *checksum* para verificação de erros
- Campos de *número de sequência* e *número de reconhecimento* de 32 bits, udados para execução do serviço confiável de transferência de dados.
- Campo de *janela de recepção* de 16 bits para controle de fluxo.
- Comprimento do cabeçalho de 4 bits
- Campo de opções, usado quando remetente e destinatário negociam o MSS
- Campo de flag de 6 bits

![](/_assets/Pasted%20image%2020230115102911.png)

### Número de sequência e reconhecimento
O TCP vê os dados como uma cadeia de bytes não estrutura, mas ordenada. Assim, quando é necessário enviar uma série de segmentos, cada segmento recebe um número de sequência de acordo com o tamanho do conteúdo a ser enviado e do MSS.

*Exemplo*: enviar um arquivo de 500.000 bytes, com MSS de 1.000 bytes. Serão criados 500 segmentos, o primeiro segmento terá o número 0, o segundo, 1.000, o terceiro 2.000, etc.

O número de reconhecimento que um hospedeiro atribui a seu segmento é o número de sequência do próximo byte que ele estiver aguardando do outro hospedeiro.