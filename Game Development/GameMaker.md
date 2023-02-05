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
