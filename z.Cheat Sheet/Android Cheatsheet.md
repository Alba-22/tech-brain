#cheatsheet 
## Erros ao enviar para a loja
#### Permissão AD_ID:
Colocar a seguinte permissão no `AndroidManifest.xml`:
```
<uses-permission android:name="com.google.android.gms.permission.AD_ID" tools:node="remove"/>
```
*OBS:* `<uses-permission>` vem antes do `<application/>`
Caso ocorrá problema na hora de buildar, alterar a primeira linha do `AndroidManifest.xml`:
```
<manifest xmlns:android="http://schemas.android.com/apk/res/android" xmlns:tools="http://schemas.android.com/tools" package="com.example.app">
```
