#cheatsheet
### Comandos pro processador M1
- Usando Rosetta no terminal
```shell
arch -x86_64 <COMMAND>
```
**Dica:** colocar um alias pro comando

### Problemas comuns ao desenvolver pra iOS
- Missing compliance:
Adicionar o seguinte código no Info.plist
```
<key>ITSAppUsesNonExemptEncryption</key>
<false/>
```
- SWIFT_VERSION error no OneSignal:
```
[!] There may only be up to 1 unique SWIFT_VERSION per target. Found target(s) with multiple Swift versions: OneSignalNotificationServiceExtension: Swift OneSignalNotificationServiceExtension: Swift 5.0
```
No XCode, ir no Target do OneSignal, Build Settings, e pesquisar por SWIFT_VERSION. A variável deve ter o mesmo valor para todos os schemas.
- CLANG_WARN_QUOTED_INCLUDE_IN_FRAMEWORK_HEADER:
```
The `OneSignalNotificationServiceExtension [Release-prod]` target overrides the `CLANG_WARN_QUOTED_INCLUDE_IN_FRAMEWORK_HEADER` build setting defined in `Pods/Target Support Files/Pods-OneSignalNotificationServiceExtension/Pods-OneSignalNotificationServiceExtension.release-prod.xcconfig'. This can lead to problems with the CocoaPods installation
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.
```

### Emuladores no iOS
- Listar todos os emuladores disponíveis:
```
xcrun simctl list devices
```
- Inicializar emulador:
```
xcrun simctl boot <DEVICE_ID>
```
- Abrir o último emulador:
```
open -a Simulator
```
- Mais comandos:
https://suelan.github.io/2020/02/05/iOS-Simulator-from-the-Command-Line/

### Tamanhos dos prints na App Store
- 6.5 polegadas:
	- 1242x2688 pixels
	- iPhone Prox Max, iPhone Xs Max
- 5.5 polegadas:
	- 1242x2208 pixels
	- iPhone 8 Plus, iPhone 7 Plus, iPhone 6s Plus
- 12.9 polegadas:
	- 2048 x 2732 pixels
	- iPad Pro (12.9-inch) (3rd generation), iPad Pro (12.9-inch) (2nd generation)
Fonte: https://stackoverflow.com/questions/52310183/what-is-app-store-screenshot-size-for-6-5-display

### Verificar IP
- Ver IP externo:
```shell
curl ipconfig.me
```
- Ver IP interno:
```shell
ifconfig -a

## É possível especificar a interface
## en0 é o padrão pra internet cabeada e en1 é o padrão pra wireless
ifconfig en0
ifconfig en1
```
