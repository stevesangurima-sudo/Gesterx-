# GestureX AI (Android, Kotlin + Jetpack Compose)

Estado honesto: el código está **escrito pero NO compilado ni probado en un dispositivo** (el entorno donde se generó no tiene Android SDK ni Internet). Puede requerir pequeños ajustes al compilar por primera vez.

## Compilar el APK
Requisitos: Android Studio (Koala o superior), JDK 17, Internet en el primer build.
1. Descomprime y abre la carpeta con **File > Open** en Android Studio. Deja que sincronice Gradle (genera el wrapper por sí mismo).
2. El build descarga automáticamente el modelo oficial `gesture_recognizer.task` a `app/src/main/assets/`.
   Si falla, descárgalo a mano desde
   https://storage.googleapis.com/mediapipe-models/gesture_recognizer/gesture_recognizer/float16/1/gesture_recognizer.task
   y colócalo en `app/src/main/assets/gesture_recognizer.task`.
3. **Build > Build Bundle(s)/APK(s) > Build APK(s)**, o por terminal: `./gradlew assembleDebug`
   → `app/build/outputs/apk/debug/app-debug.apk`. Cópialo al móvil e instálalo (permite «orígenes desconocidos»).
Plataformas online que compilan Gradle (GitHub Actions, Codemagic…) también sirven.

## Qué implementa
- Biblioteca de gestos con nivel de fiabilidad (MediaPipe Gesture Recognizer + heurísticas sobre 21 puntos).
- Asistente «Crear mi gesto» (5 pasos), nombre personalizado, varias acciones por regla (secuencias).
- Editar, duplicar, activar/desactivar, eliminar, reordenar y probar reglas. Guardado local (SharedPreferences).
- Pantalla «Activar / Probar»: cámara frontal, gesto detectado, umbral de confianza, mensajes de fallo.
- Anti-repetición: tiempo mínimo, espera entre ejecuciones y vuelta a posición neutral.
- Procesamiento local; los fotogramas no se guardan ni se envían. Permisos pedidos en tiempo de ejecución.

## Límites reales
- El reconocimiento solo funciona **con la app en pantalla**. Si una acción abre otra app, la cámara se detiene.
- Fiables: palma, puño, pulgar arriba/abajo, índice, paz. Heurísticos: 3 y 4 dedos. Experimentales (umbrales sin calibrar): juntos/separados y movimientos.
- «Dos dedos» = «Signo de paz» y «Cinco dedos» = «Palma abierta» para este modelo, por eso no se ofrecen aparte.
- Foto automática: solo con «Activar» abierto (cámara frontal, guarda en la carpeta de la app). Si no, se abre la cámara y se avisa de que NO se tomó foto.
- Inicio y captura de pantalla requieren activar a mano el servicio de accesibilidad «GestureX AI».
- Sin gestos nuevos: no hay entrenamiento y no se simula aprendizaje.
- En secuencias, Android puede bloquear sin error que un paso abra una app desde segundo plano.
- Criterio de aceptación sin verificar en dispositivo real.
