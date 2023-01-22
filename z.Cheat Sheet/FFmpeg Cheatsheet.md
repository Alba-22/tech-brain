#cheatsheet 

## Download
https://ffmpeg.org/download.html#build-windows

## Cortando vídeo
### Duração específica a partir de um começo
```
ffmpeg -i input.mp4 -ss 00:05:20 -t 00:10:00 -c:v copy -c:a copy output1.mp4
```
- **-i**: caminho do vídeo a ser editado
- **-ss**: seeking parameter, onde começar o corte
- **-t**: duração do corte
- **-c:v copy**: copia o vídeo original sem encoding
- **-c:a copy**: copia o audio original sem encoding

### Especificando começo e fim
```bash
ffmpeg -i input.mp4 -ss 00:05:10 -to 00:15:30 -c:v copy -c:a copy output2.mp4
```
- **-to**: especifica o tempo final do corte

### Cortando a partir do fim do vídeo
```bash
ffmpeg -sseof -600 -i input.mp4 -c copy output5.mp4

ffmpeg -sseof -00:10:00 -i input.mp4 -c copy output6.mp4
```
- **-sseof**: seta o offset relativo ao EOF

### Cortando com re-encoding
Pode-se especificar o formato para que o vídeo seja feito re-encoding. Esse processo levar mais tempo que se não for feito o re-encoding.
```bash
ffmpeg -ss 00:05:20 -accurate_seek -i input.mp4 -t 00:10:00 -c:v libx264 -c:a aac output7.mp4
```

## Especificando tempo
O tempo deve ser especificado no seguinte formato: `HOURS:MM:SS.MILLISECONDS`, sendo que os milisegundos podem ser omitidos.
Também é possível especificar o tempo em segundos.

## Juntando duas tracks de aúdio de um vídeo
```shell
ffmpeg -i input.mkv -filter_complex "[0:a:0][0:a:1]amix=2:longest[aout]" -map 0:V:0 -map "[aout]" -c:v copy -c:a aac -b:a 320k output.mkv
```
- Também é possível boostar uma das tracks:
```shell
ffmpeg -i input.mkv -filter_complex "[0:a:0][0:a:1]amix=2:longest:weights=1 2[aout]" -map 0:V:0 -map "[aout]" -c:v copy -c:a aac -b:a 320k output.mkv
```
https://www.reddit.com/r/ffmpeg/comments/gu5pc8/is_it_possible_to_merge_two_audio_tracks_in_a/