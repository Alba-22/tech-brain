#cheatsheet
### Firebase Analytics
- Habilitar/desabilitar o Debug View no Android:
```
adb shell setprop debug.firebase.analytics.app <package_name>
adb shell setprop debug.firebase.analytics.app .none. 

-FIRDebugEnabled
-FIRDebugDisabled
```

### Android:
- Visualizar Chave SHA-1 da aplicação:
```shell
cd ./android
./gradlew signingReport
```
- Conectar no Android físico pelo Wi-Fi
```
adb devices
adb tcpip 5555
adb connect 192.168.1.101:5555 
```
https://medium.com/@jmellodev/flutter-debug-por-wifi-435d508a09cf

### VS Code
- Remover logs inúteis do Debug Console:
Colocar o seguinte Regex no filtro do Debug Console
```
!D/EGL_emulation, !D/InsetsController, !D/InputMethodManager
```

### Configuração do Crashlytics
![[Pasted image 20230108114330.png]]
