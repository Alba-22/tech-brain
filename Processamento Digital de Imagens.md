## Introdução
O processamento digital de imagens(PDI) envolve um conjunto de tarefas interligadas, que começa com a captura de uma imagem(iluminação refletida na superfície de objetos) realizada através de um sistema de aquisição, seguido de etapas de processamento para transformar a imagem em uma forma apropriada para o tratamento computacional.

O primeiro passo de processamento é o *pré-processamento*, que envolve filtragem de ruídos e correção de distorções geométricas causadas pelo sensor. Após isso, há uma série de processos para *extrair caracterísiticas* como: bordas, texturas, vizinhanças, movimento. Em seguida, tenta-se distinguir o *background* da imagem por *processos de segmentação, regularização e modelagem*, que usam várias estratégias de otimização para minimizar o desvio entre os dados da imagem e um modelo que incorpora conhecimento sobre os objetos da imagem.

Pela forma goemétrica dos objetos, tenta-se extrair informações adicionais para realizar a *classificação*, que tem como objetivo reconhecer, verificar ou inferir a identidade dos objetos a partir das características obtidas pelas etapas anteriores.

Além disso, pode ser necessário mecanismos de *feedback*(ou visão ativa) entre as tarefas para ajustar parâmetros como aquisiçào, iluminação, ponto de observação para facilitar a classificação.

![](_assets/Pasted%20image%2020230129220545.png)

Tanto a Computação Gráfica(CG) quanto o Processamento Digital de Imagens atuam sobre o mesmo conhecimento, que inclui a interação entre iluminação e objetos e projeções de uma cena tridimensional em um plano de imagem. CG busca imagens foto-realísticas de cenas tridimensionais geradas por computador e PDI busca reconstruir uma cena tridimensional a partir de uma imagem real, obtida através de uma câmera.

## Conceitos Fundamentais

#### Natureza da luz
A luz é radiação eletromagnética e apresenta um comportamento ondulatório caracterizado por sua frequência e comprimento de onda. A faixa de 400 a 770nm é a *luz visível*.
![](_assets/Pasted%20image%2020230213205214.png)

#### Olho Humano
O olho humano possui um diâmetro de 2 a 2,5 cm.  A radiação de objetos penetra no olho através de uma abertura na íris, chamada de *pupila*, e de uma lente denominada *cristalino*, atingindo a *retina*, que está na parte posterior do globo ocular.
![](_assets/Pasted%20image%2020230213205727.png)
A imagem chega à retina invertida. Na retina existem:
- **cones**: sensíveis a cores, com alta resolução e operantes em cenas iluminadas.
- **bastonetes**: insensíveis a cores, com baixa resolução e operantes em baixa luminosidade.
que irão converter a *energia luminosa* em *impulsos elétricos* para que o cérebro possa interpretar.

A *pálpebra* tem função análoga ao obturador de uma câmera. O *diafragma* é semelhante à íris. A *lente* é análoga ao cristalino, com a diferença que o cristalino se deforma para focalizar a imagem e a lente possui um mecanismo manual ou automático para ajuste da distância focal.

#### Modelos Cromáticos
O processo *aditivo* é a percepção de objetos que emitem luz visível por meio da soma das cores espectrais emitidas. O processo se caracteriza pela combinação variável em proporção de componentes monocromáticas nas faixas espectrais associadas às sensações de cor *Verde, Vermelho e Azul(RGB)*, caracterizando essas como as *cores primárias*.

A combinação dessas cores, duas a duas em imgual intensidade, produz as *cores secundárias*: *Ciano, Magenta e Amarelo(CMY)*
![](_assets/Pasted%20image%2020230213211025.png)

A *cor oposta* a uma secundária é a cor primária que não entra em sua composição. A *cor branca* é gerada pela combinação balanceada de vermelho, verde e azul, assim como pela combinação de qualquer cor secundária com sua oposta(óbvio). 

A cor de um objeto é definida pela radiação eletromagnética refletida por ele. Assim um objeto amarelo, reflete o vermelho e o verde, mas absorve o azul. Assim, a cor amarela em um objeto é vista como a subtração do azul da cor branca.

As cores primárias são as cores secundárias do modelo RGB:
![](_assets/Pasted%20image%2020230213212243.png)

Imagens digitais adotam o modelo RGB. Já os dispositivos de impressão colorida adotam o CMY, com adição de um quarto pigmento(o preto), já que a combinação balanceada deles não produzem a cor preta.

Ainda há outros modelo cromáticos nos quais a caracterização da cor não se dá pelo comportamente fisiológico da retina humana. Outros modelos adotam a intensidade, matiz e tonalidade(*hue*) e a saturação ou pureza.

#### Modelo de Imagem Digital
