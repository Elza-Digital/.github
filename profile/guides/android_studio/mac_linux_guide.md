# 🍎/🐧 Guía de Configuración: Android Studio en macOS y Linux

Esta guía está optimizada para **Mac con chips Apple Silicon (M1, M2, M3, M4)** y distribuciones **Linux (Debian/Ubuntu)**.

### 1. Instalación de SDK
1. Ve a **Settings > Languages & Frameworks > Android SDK**.
2. Instala la plataforma **Android 14.0 (API 34)**.
3. En **SDK Tools**, asegúrate de tener:
    * Android SDK Build-Tools
    * Android SDK Platform-Tools
    * Android Emulator

![Imagen: Panel de configuración de SDK en macOS]

### 2. Configuración del Shell (Zsh o Bash)
Debes añadir las rutas al archivo de configuración de tu terminal (`.zshrc` en Mac/Zsh o `.bashrc` en Linux).

1. Abre el archivo: `nano ~/.zshrc` (o `~/.bashrc`).
2. Pega estas líneas al final:
```bash
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

3. Guarda y aplica: `source ~/.zshrc`.

### 3. Emulador Optimizado (Especial para M4/ARM)

1. Abre el Device Manager.

2. Crea un dispositivo Pixel 7/8.

3. **Paso Clave**: En la selección de imagen, elige la pestaña "Other Images" y busca una que sea arm64-v8a. Esto permitirá que el emulador corra de forma nativa en tu procesador M4 sin usar Rosetta, lo que lo hace extremadamente rápido y ligero.

4. Lanzamiento con Expo
Con el emulador abierto, ve a tu proyecto y ejecuta:

```Bash
npx expo start
```
Pulsa la tecla a y el proyecto se abrirá automáticamente en el dispositivo virtual.

![Imagen: Emulador de Android corriendo una app de Expo en un MacBook]