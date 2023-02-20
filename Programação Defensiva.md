#software-engineering 

Programação defensiva é uma forma de desenvolvimento de programas capaz de detectar potenciais anormalidades de comportamento e realizar respostas pré-determinadas, garantindo o funcionamento do software em circunstâncias imprevistas durante o desenvolvimento. É muito útil quando se necessita de alta disponibilidade e de segurança.

A programação defensiva é uma abordagem para melhorar o software e o código fonte em termos de:
- *Qualidade* Geral
- Tornar o *código* mais *compreensível*
- Fazer o código se *comportar* de maneira *predizível* independente de entradas inesperadas ou ações do usuário.

O principal conceito quando programando defensivamente é considerar os problemas antes que eles surjam, criando código que é feito para lidar com qualquer cenário possível. A programação defensiva também sugere a eliminação de qualquer código desnecessário e a simplificação do código quando possível, justamente para diminuir as oportunidades de introdução de bugs.

Apesar disso, a existência de checagens e tratamento de erros por si só não categoriza estar programando defensivamente. Assim, condições para checar se um valor é nulo, tratar falta de conexão de rede, tratar se um arquivo não foi encontrado por ter sido deletado e outras verificações de erros esperados não necessariamente são programar defensivamente.

## Categorias da Programação Defensiva
#### Programação Segura
É o subset da programação defensiva focado na segurança do software, de forma que, além de visar evitar bugs, também se motiva a reduzir a superfície de ataque. O programador deve assumir quer o software pode ser usado de forma indevida para encontrar bugs que podem ser explorados maliciosamente, como buffer overflow, SQL injection, dentre outros.

#### Programação Ofensiva
É uma categoria que enfatiza que certos erros não devem ser tratados defensivamente, ou seja, deve haver tolerância a falhas. Assim, apenas erros externos(como input do usuário) ao programa devem ser tratados e o software em si é tratado como algo mais confiável.

Dessa forma, a programação ofensiva coloca que erros internos ao programa devem fazê-lo reportar os detalhes do erro e terminar de forma abrupta para o usuário, impedindo que o programa continue a funcionar após o dado erro.

## Técnicas de Programação Defensiva
#### Design By Contract(DBC)
O DBC estabelece que um programa deve fazer apenas o que ele se propõe a fazer, conforme definido anteriormente. As organizações envolvidas devem, então, definir  como os módulos do software interagem, de forma com que cada vez que o programa falha em aderir ao contrato, pode-se surgir um novo bug.

#### Dead programs tell no lies
Problemas no funcionamento do software devem ser detectados o mais rápido possível, fazendo-o crashar em vez de continuar a rodar com os problemas, visto que a partir desse ponto, as informações podem não ser mais confiáveis.

#### Assertive Programming
