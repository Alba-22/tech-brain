#compilers
## O que é um Compilador?
Um compilador é um programa que recebe como entrada um programa em uma linguagem de programação(*linguagem fonte*) e o traduz para um programa equivalente em outra linguagem(*linguagem objeto*).
![](_assets/Pasted%20image%2020230305130306.png)

Assim, um compilador tem como objetivo traduzir um código em outros. Geralmente o código **fonte** é escrito em linguagem de **alto nível**, sendo influenciado pela linguagem adotada, e o código **objeto** em linguagem **simbólica** ou linguagem de máquina, sendo dependente da arquitetura alvo.

Além disso, é importante notar que os códigos fonte e objeto serão sintaticamente diferentes, porém **devem** ser **semanticamente equivalentes**. Dessa forma, o compilador tem como papel fazer a **abstração** entre o **software** e o **hardware**, mantendo a corretude do programa e até otimizando o programa original.

## Processadores de Linguagens
#### Compilação
- O programa fonte é **totalmente** traduzido, gerando um programa objeto em linguagem de máquina
- A tradução ocorre apenas **uma vez**, gerando o programa objeto.
- O programa objeto pode ser **executado várias vezes** para processar diferentes entradas.
- Melhor desempenho na execução
![](_assets/Pasted%20image%2020230305132138.png)

#### Interpretação
- As operações especificadas no programa fonte são executadas diretamente sobre as entradas fornecidas pelo usuário.
- Ou seja, a tradução é feita em todas as execuções especificamente para os dados de entrada do programa.
- Oferece melhor diagnóstico de erro, porém é mais lenta, já que executa instrução por instrução todas as vezes.
![](_assets/Pasted%20image%2020230305132543.png)

#### Sistema Híbrido
- O programa fonte é compilado para uma linguagem intermediária, a qual é posteriormente interpretada.
- O código intermediário é mais próximo da linguagem de máquina
- Permite melhor **portabilidade**: o código é independente da arquitetura da máquina, bastante ter um interpretador do código intermediário.
![](_assets/Pasted%20image%2020230305133322.png)

#### Compilação Just-in-Time(JIT)
- Código intermediária é traduzido para linguagem de máquina antes da execução
