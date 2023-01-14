#network
## Introdução
A camada de transporte fornece serviços de comunicação diretamente aos processos de aplicação que rodam em diferentes hospedeiros. Os protocolos da camada de transporte fornecem **comunicação lógica** entre os processos dos hospedeiros que estão se comunicando. Ou seja, ele faz com que na visão da camada de aplicação, esses hospedeiros estejam diretamente conectados.
A camada de transporte é implementada nos sistemas finais, de forma que ela converte as mensagens recebidas da aplicação em **segmentos da camada de transporte**. Na criação desses segmentos, divide-se as mensagens da camada de aplicação e adiciona-se um header a cada um deles.
![](_assets/Pasted%20image%2020230114115723.png)
A camada de transporte, então, passa o segmento pra camada de rede do remetente, que encapsula o segmento em um datagrama e envia ao destinatário.
*OBS:* nesse processo, os roteadores que estiverem levando a mensagem não possuem a camada de transporte para examinar os campos do segmento.

### Relação Transporte e Rede
A camada de transporte fornece comunicação lógica entre **processos** que rodam nos hospedeiros e a camada de rede fornece comunicação lógica entre os **hospedeiros** em si.