## Introdução
O processamento digital de imagens(PDI) envolve um conjunto de tarefas interligadas, que começa com a captura de uma imagem(iluminação refletida na superfície de objetos) realizada através de um sistema de aquisição, seguido de etapas de processamento para transformar a imagem em uma forma apropriada para o tratamento computacional.

O primeiro passo de processamento é o *pré-processamento*, que envolve filtragem de ruídos e correção de distorções geométricas causadas pelo sensor. Após isso, há uma série de processos para *extrair caracterísiticas* como: bordas, texturas, vizinhanças, movimento. Em seguida, tenta-se distinguir o *background* da imagem por *processos de segmentação, regularização e modelagem*, que usam várias estratégias de otimização para minimizar o desvio entre os dados da imagem e um modelo que incorpora conhecimento sobre os objetos da imagem.

Pela forma goemétrica dos objetos, tenta-se extrair informações adicionais para realizar a *classificação*, que tem como objetivo reconhecer, verificar ou inferir a identidade dos objetos a partir das características obtidas pelas etapas anteriores.

Além disso, pode ser necessário mecanismos de *feedback*(ou visão ativa) entre as tarefas para ajustar parâmetros como aquisiçào, iluminação, ponto de observação para facilitar a classificação.

![](_assets/Pasted%20image%2020230129220545.png)

Tanto a Computação Gráfica(CG) quanto o Processamento Digital de Imagens atuam sobre o mesmo conhecimento, que inclui a interação entre iluminação e objetos e projeções de uma cena tridimensional em um plano de imagem. CG busca imagens foto-realísticas de cenas tridimensionais geradas por computador e PDI busca reconstruir uma cena tridimensional a partir de uma imagem real, obtida através de uma câmera.

## Conceitos Fundamentais
