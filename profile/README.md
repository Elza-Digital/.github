# Guía: Entorno de Desarrollo en Windows (WSL2 + Docker + Mobile)
<p align="center">
<img width="380" height="380" alt="image" src="https://github.com/user-attachments/assets/33c7771c-0612-4aba-8a16-47f58ba56fd8" />
</p>

Bienvenido/a al equipo. En esta guía configuraremos tu máquina Windows para que funcione como un entorno Linux puro utilizando **WSL2**. Esto nos evita problemas de compatibilidad y nos da un ecosistema limpio para Node, Python, Docker y React Native/Expo.

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
curl -o- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh) | bash
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
curl [https://pyenv.run](https://pyenv.run) | bash
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

## FASE 4: El Puente con Android Studio (Expo / React Native)
Para correr aplicaciones móviles, el emulador de Android debe ejecutarse en Windows (por la tarjeta gráfica), pero nuestro código se ejecuta en Linux. Vamos a conectarlos.

1. Instalación en Windows
- Descarga e instala Android Studio (.exe normal).

- Abre Android Studio, instala un emulador (Virtual Device) y déjalo abierto y corriendo.

2. Configuración del Puente en WSL
- Tenemos que decirle a nuestro Linux que se comunique con el emulador de Windows.

- En tu terminal Ubuntu, instala las herramientas de Android:

```Bash
sudo apt update && sudo apt install android-tools-adb -y
```
- Añade esta variable de entorno a tu Linux. Abre tu archivo de configuración (nano ~/.bashrc o ~/.zshrc si usas Zsh) y pega esto al final:

```Bash
export ADB_SERVER_SOCKET=tcp:host.docker.internal:5037
```
- Guarda (Ctrl+O, Enter) y sal (Ctrl+X). Reinicia la terminal.

3. Exponer el servidor en Windows
- En Windows, abre un PowerShell como Administrador (solo PowerShell sirve para esto).

- Mata cualquier servidor ADB viejo y arranca uno que escuche en todos los puertos:

```PowerShell
adb kill-server
adb -a nodaemon server start
```
> [!WARN]
> (Deja esa ventana de PowerShell abierta en segundo plano).

### ¡Prueba Final!
Ve a tu terminal de Ubuntu, entra en tu proyecto de Expo y ejecuta:

```Bash
npx expo run:android
# o también dependiendo de cómo esté en el package.json probar con npm run android
```
Si todo está bien configurado, ¡tu código de Linux compilará y la app aparecerá mágicamente en el emulador de tu Windows!
