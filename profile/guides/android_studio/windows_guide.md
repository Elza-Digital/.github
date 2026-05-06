# Guía de Configuración: Android Studio en Windows

### 1. Instalación de SDK y Herramientas
1. Abre **Android Studio > Settings > Languages & Frameworks > Android SDK**.
2. En **SDK Platforms**, instala **Android 14.0 (API 34)**.
3. En **SDK Tools**, marca:
    * Android SDK Build-Tools
    * Android Emulator
    * Android SDK Platform-Tools
    * Intel x86 Emulator Accelerator (HAXM) — *Solo si usas Intel y no tienes Hyper-V activo*.

![Imagen: Captura de pantalla de la pestaña SDK Tools en Windows]

### 2. Configuración de Variables de Entorno (CRÍTICO)
Windows necesita saber dónde están estas herramientas para que el comando `npx expo start` funcione.

1. Busca "Editar las variables de entorno del sistema" en el inicio.
2. Haz clic en **Variables de entorno**.
3. En **Variables de usuario**, haz clic en **Nueva**:
    * Nombre: `ANDROID_HOME`
    * Valor: `C:\Users\TU_USUARIO\AppData\Local\Android\Sdk`
4. En la misma sección, busca la variable **Path**, edítala y añade estas dos líneas:
    * `%ANDROID_HOME%\platform-tools`
    * `%ANDROID_HOME%\emulator`

### 3. Creación del Emulador
1. Abre el **Device Manager**.
2. Crea un dispositivo **Pixel 8**.
3. Descarga la imagen **x86_64** (importante para el rendimiento en Windows).
4. Activa la "Aceleración de Gráficos" por hardware en la configuración avanzada del emulador.

---
![Imagen: Terminal de Windows ejecutando 'adb devices' para verificar la conexión]