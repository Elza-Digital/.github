# Guía: Entorno de Desarrollo en Windows (WSL2 + Docker + Mobile)
<p align="center">
<img width="380" height="380" alt="image" src="https://github.com/user-attachments/assets/33c7771c-0612-4aba-8a16-47f58ba56fd8" />
</p>

Bienvenido/a al equipo. En esta guía configuraremos tu máquina Windows para que funcione como un entorno Linux puro utilizando **WSL2**. Esto nos evita problemas de compatibilidad y nos da un ecosistema limpio para Node, Python, Docker y React Native/Expo.

## 🌟 Herramientas y Repos de Interés (Externos)

[![Win11Debloat Card](https://github-readme-stats.vercel.app/api/pin/?username=Raphire&repo=Win11Debloat&theme=default&show_owner=true)](https://github.com/Raphire/Win11Debloat)
---

## FASE 1: La Terminal y WSL (El Núcleo)

Vamos a jubilar PowerShell y a utilizar Ubuntu como nuestra terminal por defecto.

1. Abre la **Microsoft Store** e instala **Windows Terminal**.
2. Abre tu actual PowerShell **como Administrador** y ejecuta:
   ```bash
   wsl --install
Reinicia tu ordenador. Al volver, se abrirá una consola pidiéndote crear un usuario y contraseña de Linux. (Guárdalos bien).

Abre Windows Terminal, ve a la flecha hacia abajo ⬇️ en la pestaña superior -> Configuración.

En Perfil predeterminado, selecciona Ubuntu. Guarda los cambios. A partir de ahora, cada vez que abras la terminal, estarás en Linux.

## FASE 2: Ecosistema Interno (Node y Python)
> [!TIP]
> A partir de ahora, NUNCA instales Node o Python usando archivos .exe en Windows. Todo se instala dentro de la terminal de Ubuntu.

1. NVM (Node Version Manager)
Para instalar Node y npm, usaremos NVM, que nos permite cambiar de versión fácilmente.

- Ejecuta en tu terminal Ubuntu:

```Bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
- Cierra la terminal y vuelve a abrirla. Luego instala la última versión de Node:

```Bash
nvm install --lts
nvm use --lts
```
2. Pyenv (Python)
- Para Python, usaremos Pyenv. Primero, instala las dependencias necesarias de Ubuntu:

```Bash
sudo apt update && sudo apt install build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev -y
```
- Instala Pyenv:

```Bash
curl https://pyenv.run | bash
```
> [!NOTE]
> El instalador te pedirá que copies 3 líneas al final de tu archivo ~/.bashrc. Hazlo, guarda el archivo y reinicia la terminal.

- Instala Python (ejemplo con la 3.11):

```Bash
pyenv install 3.11.0
pyenv global 3.11.0
```
## FASE 3: Docker Integrado
Queremos que Docker corra en Windows pero que sea accesible desde nuestra terminal Linux.

- Descarga e instala Docker Desktop para Windows desde su web oficial (instalador .exe).

- Abre Docker Desktop en Windows.

- Ve a la rueda dentada (Settings) > General y marca la casilla "Use the WSL 2 based engine".

- Ve a Settings > Resources > WSL Integration. Asegúrate de que el interruptor de Ubuntu esté activado.

- Haz clic en "Apply & Restart".

- Entra en tu terminal Ubuntu y prueba que funciona escribiendo: docker ps.

## FASE 4: SDK de Android y Java en WSL (Para compilar)
Para que comandos como expo run:android puedan compilar el código nativo, WSL necesita su propio Java y el SDK de Android.

- Instala Java (JDK 17) y utilidades básicas:

```Bash
sudo apt update && sudo apt install unzip openjdk-17-jdk android-tools-adb -y
```
- Descarga y extrae las herramientas de línea de comandos de Android:

```Bash
mkdir -p ~/Android/Sdk/cmdline-tools
wget [https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip](https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip) -O cmdline-tools.zip
unzip cmdline-tools.zip -d ~/Android/Sdk/cmdline-tools
rm cmdline-tools.zip
mv ~/Android/Sdk/cmdline-tools/cmdline-tools ~/Android/Sdk/cmdline-tools/latest
```
- Acepta todas las licencias oficiales de Android (escribe y y presiona Enter a todo lo que salga):

```Bash
yes | ~/Android/Sdk/cmdline-tools/latest/bin/sdkmanager --licenses
```
## FASE 5: El Puente con el Emulador de Windows
El código se compila en Linux, pero el Emulador se ejecutará en Windows (para aprovechar la tarjeta gráfica).

1. Variables de entorno en WSL
Abre tu archivo de configuración en Linux (nano ~/.bashrc) y pega todo este bloque de configuración al final del documento:

```Bash
# Variables de Android SDK y ADB Bridge
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
export ADB_SERVER_SOCKET=tcp:host.docker.internal:5037
```
Guarda (Ctrl+O, Enter) y sal (Ctrl+X). Reinicia la terminal de Ubuntu.

2. Exponer el servidor en Windows
En Windows, abre Android Studio (.exe normal), ve al "Device Manager" y arranca tu Emulador. Déjalo abierto.

- Abre una ventana de PowerShell como Administrador en Windows y ejecuta:

```PowerShell
adb kill-server
adb -a nodaemon server start
```

> [!WARNING]
> Deja esta ventana negra de PowerShell abierta y minimizada. Esto es lo que permite que WSL se conecte a tu emulador.

### ¡Prueba Final!
Ve a tu terminal de Ubuntu, entra en tu proyecto de Expo y ejecuta:

```Bash
npx expo run:android
# o también dependiendo de cómo esté en el package.json probar con npm run android
```
Si todo está bien configurado, ¡tu código de Linux compilará y la app aparecerá mágicamente en el emulador de tu Windows!

## Error final (por ahora)
```Bash
Error: could not connect to TCP port 5554: Connection refused
Error: /home/powasote/Android/Sdk/platform-tools/adb -s emulator-5554 emu avd name exited with non-zero code: 1
    at ChildProcess.completionListener (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/@expo/spawn-async/src/spawnAsync.ts:67:13)
    at Object.onceWrapper (node:events:631:26)
    at ChildProcess.emit (node:events:509:28)
    at maybeClose (node:internal/child_process:1124:16)
    at Process.ChildProcess._handle.onexit (node:internal/child_process:306:5)
    ...
    at spawnAsync (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/@expo/spawn-async/src/spawnAsync.ts:28:21)
    at ADBServer.runAsync (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/expo/node_modules/@expo/cli/src/start/platforms/android/ADBServer.ts:85:59)
    at processTicksAndRejections (node:internal/process/task_queues:104:5)
    at getAdbNameForDeviceIdAsync (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/expo/node_modules/@expo/cli/src/start/platforms/android/adb.ts:329:19)
    at /home/powasote/proyects/fichYa/fichYa/frontend/node_modules/expo/node_modules/@expo/cli/src/start/platforms/android/adb.ts:312:14
    at async Promise.all (index 0)
    at getDevicesAsync (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/expo/node_modules/@expo/cli/src/start/platforms/android/getDevices.ts:7:25)
    at Object.resolveAsync [as resolveDeviceAsync] (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/expo/node_modules/@expo/cli/src/start/platforms/android/AndroidDeviceManager.ts:41:21)
    at AndroidPlatformManager.openProjectInExpoGoAsync (/home/powasote/proyects/fichYa/fichYa/frontend/node_modules/expo/node_modules/@expo/cli/src/start/platforms/PlatformManager.ts:109:27)
    at async Promise.allSettled (index 0)
```

Al intentar ejecutar npm run android en la carpeta (/frontend) del proyecto.
