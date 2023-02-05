#gamedev

## Sprites
- Imagens utilizadas dentro do jogo.
- No GameMarker, tem-se o padrão de colocar o prefixo `spr_` na frente do nome de um sprite
- Animação: sequência de sprites
- Máscara de colisão: "hitbox" de um sprite
- Nine slice: usado para modificar o comportamento de um sprite quando ele for redimensionado, fazendo com que partes dele não sejam necessariamente alteradas
	- *OBS:* Sprites com nine-slice não podem ser usadas como background da room.
- Ao redimensionar um sprite, duas opções:
	- Ajustar escala da imagem -> Pode deformar a imagem
	- Modificar o canvas -> Criará um margin em volta da imagem para não deformá-la

## Rooms
- É onde o jogo roda, podendo ser a tela inicial do jogo, um level, etc.
- Prefixo: `rm_`
- Camadas:
	- Background: fundo do jogo
	- Instâncias: quem de fato joga o jogo, os personagens
	- Tiles:  desenhos
	- Pets:
	- Assets: para sprites
	- Effect:
- Pode-se criar múltiplas camadas do mesmo tipo para definir qual está na frente
- Mas também dá pra mudar a ordem dos sprites dentro de uma mesma camada
- Quando uma camada está selecionada, não é possível selecionar um objeto que está em outra camada(exceto apertando P(atalho no canto superior direito))
- A room também tem grid e snap para mover coisas

## Objetos
- São onde o código é rodado
- Prefixo: `obj_`
- Pode-se associar um sprite ao objeto
- Atalho pra posicionar objeto: selecionar no menu direito e segurar `Alt` e clicar na room
- O objeto posicionado na room é uma instância(relação classe/objeto de POO)
- Eventos: coisas que acontecem no jogo
- Eventos são adicionados no objeto, e não na instância 

## GameMaker Language(GML)
- Semelhante em JavaScript e C#
- F7 e F8 para modificar tamanho da fonte
- Documentação - F1