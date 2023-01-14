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

## Multiplexação e Demultiplexação
É a ampliação do serviço de entrega hospedeiro/hospedeiro da camada de rede para um serviço de entrega processo/processo.
Na realidade, a camada de transporte não entrega os dados diretamente a um processo, mas sim a um socket intermediário, tendo cada socket um identificador exclusivo.
![](/_assets/Pasted%20image%2020230114125738.png)
**Demultiplexação:** é realizado na extremidade receptora, entregando os dados contidos em um segmento da camada de transporte ao socket correto.
**Multiplexação:** é realizado na extremidade de origem, reunindo os partes de dados provenientes de vários sockets, encapsulando essas partes em um header para criar segmentos e enviando os segmentos para a camada de rede.
Em relação à interação segmento/socket, temos que as portas(dos sockets) tem identificadores exclusivos e que cada segmento tem campos especiais que indicam a porta para qual o segmento deve ser entregue.
![](/_assets/Pasted%20image%2020230114131525.png)
Os campos especiais são: **campo de número de porta de origem** e **campo de número de porta de destino**. Cada número de porta é um valor de 16 bits entre 0 e 65535.