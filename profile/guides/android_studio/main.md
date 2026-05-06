# 🛠️ Configuración de Android Studio para Expo React Native

Para desarrollar con Expo y probar tus aplicaciones en un entorno local, Android Studio es una pieza fundamental. Aunque Expo se encarga de mucho trabajo, necesitamos las herramientas de Android (SDK) y el emulador (AVD) para ejecutar el código.

## 🎯 Recomendaciones de Rendimiento (Mínimo peso)

Para que tu máquina vuele y no consumas todos los recursos, sigue estas pautas:

1.  **Mejor Android SDK:** Actualmente, te recomiendo instalar el **Android 14.0 (UpsideDownCake) - API 34** o el **Android 15 (VanillaIceCream) - API 35**. Son las versiones más estables y compatibles con las librerías modernas de React Native.
2.  **Virtual Device (AVD) Ligero:**
    * **Modelo:** Selecciona el **Pixel 8** o **Pixel 7**. Tienen una densidad de píxeles equilibrada.
    * **Perfil de hardware:** Si buscas lo más ligero posible, crea un dispositivo con poca RAM (1.5GB - 2GB) y una resolución de pantalla menor (ej. 720p).
    * **Arquitectura:** * Para **Mac (M1/M2/M3/M4) y Linux ARM**: Usa imágenes `arm64-v8a`.
        * Para **Windows e Intel/AMD**: Usa imágenes `x86_64`.

---

## 🧭 Guías de Configuración por Sistema

Selecciona tu sistema operativo para ver los pasos detallados:

👉 [Guía de Configuración para Windows](windows_guide.md)

👉 [Guía de Configuración para Linux y macOS](mac_linux_guide.md)

---
![Imagen: Panel principal de Android Studio con el Device Manager abierto]