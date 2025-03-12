# Ejercicio de Integración Frontend-Backend

## Descripción del Proyecto

Este proyecto es una aplicación móvil Android que demuestra la integración entre un frontend móvil y un backend REST. La aplicación consta de dos componentes principales:

1. **Backend (Spring Boot)**: Un servidor REST simple desarrollado con Spring Boot que expone un endpoint `/hello` que devuelve un mensaje básico en formato JSON.

2. **Frontend (Android)**: Una aplicación Android desarrollada con Kotlin y Jetpack Compose que consume el endpoint del backend y muestra la respuesta al usuario.

La aplicación Android tiene una interfaz simple con un botón que, al ser presionado, realiza una solicitud al servidor backend. La respuesta obtenida (mensaje y estado) se muestra en la pantalla del usuario.

## Instrucciones para Configurar y Ejecutar

### Requisitos Previos
- Java JDK 17 o superior
- Android Studio Iguana o superior
- Gradle 8.10 o superior
- Emulador Android o dispositivo físico

### Backend (Spring Boot)

1. **Navegar al directorio del backend**:
   ```bash
   cd demo
   ```

2. **Ejecutar el servidor Spring Boot**:
   - En Windows:
     ```bash
     .\gradlew.bat bootRun
     ```
   - En Linux/Mac:
     ```bash
     ./gradlew bootRun
     ```

3. **Verificar que el servidor está funcionando**:
   - Abre un navegador y visita: `http://localhost:8080/hello`
   - Deberías ver una respuesta JSON: `{"message":"Hola Mundo","status":"success"}`

### Frontend (Android)

1. **Abrir el proyecto en Android Studio**:
   - Abre Android Studio
   - Selecciona "Open an existing project"
   - Navega y selecciona el directorio `Tarea3`

2. **Configurar el emulador o dispositivo**:
   - Si usas un emulador: La URL del backend está configurada para usar `10.0.2.2:8080` que redirige al localhost del host
   - Si usas un dispositivo físico: Edita la URL en `RetrofitClient.kt` para que apunte a la IP de tu computadora

3. **Ejecutar la aplicación**:
   - Haz clic en el botón de Run (triangulo verde) o presiona Shift+F10
   - Selecciona tu emulador o dispositivo
   - Espera a que la aplicación se instale y ejecute

4. **Usar la aplicación**:
   - Una vez que la aplicación esté abierta, verás una pantalla con un mensaje "Esperando respuesta..."
   - Presiona el botón "Obtener Mensaje"
   - La aplicación mostrará un indicador de carga y luego el mensaje recibido del servidor

## Arquitectura de la Aplicación

La aplicación sigue una arquitectura de cliente-servidor básica:

```
┌───────────────────────┐        HTTP Request      ┌──────────────────────┐
│ Android App (Cliente) │ ─────────────────────────> Spring Boot (Servidor) │
│                       │ <─────────────────────────                        │
└───────────────────────┘        HTTP Response     └──────────────────────┘
```

### Componentes del Frontend (Android):

1. **Capa de UI**:
   - `MainActivity.kt`: Actividad principal que contiene la UI de la aplicación
   - `HelloScreen()`: Composable de Jetpack Compose que define la interfaz de usuario

2. **Capa de Red**:
   - `RetrofitClient.kt`: Configura la instancia de Retrofit para las solicitudes HTTP
   - `ApiService.kt`: Define la interfaz para las llamadas a la API

3. **Capa de Modelo**:
   - `HelloResponse.kt`: Clase de datos que modela la respuesta JSON del servidor

### Componentes del Backend (Spring Boot):

1. **Controlador**:
   - `HelloController.java`: Controlador REST que expone el endpoint `/hello`

2. **Aplicación Principal**:
   - `DemoApplication.java`: Clase principal que inicia la aplicación Spring Boot

## Desafíos Encontrados y Soluciones

1. **Configuración de la Comunicación HTTP**:
   - **Desafío**: Permitir que la aplicación Android realice solicitudes HTTP al servidor local.
   - **Solución**: Configuración de `android:usesCleartextTraffic="true"` y un archivo `network_security_config.xml` para permitir tráfico HTTP a `10.0.2.2`.

2. **Comunicación entre Emulador y Localhost**:
   - **Desafío**: Hacer que el emulador de Android se comunique con el servidor que se ejecuta en el equipo host.
   - **Solución**: Uso de la dirección especial `10.0.2.2` que en el emulador de Android redirige al localhost (127.0.0.1) de la máquina host.

3. **Manejo de Errores y Estados de Carga**:
   - **Desafío**: Proporcionar una experiencia de usuario fluida durante la comunicación con el servidor.
   - **Solución**: Implementación de estados de carga, éxito y error con indicadores visuales apropiados.

4. **Serialización/Deserialización JSON**:
   - **Desafío**: Convertir entre objetos Kotlin y formato JSON.
   - **Solución**: Uso de Retrofit con el convertidor GSON para manejar automáticamente la conversión.

## Dependencias Utilizadas

### Frontend (Android)

1. **UI y Componentes de Android**:
   - `androidx.core:core-ktx`: Extensiones de Kotlin para Android Core
   - `androidx.lifecycle:lifecycle-runtime-ktx`: Componentes de ciclo de vida para Kotlin
   - `androidx.activity:activity-compose`: Integración de Jetpack Compose con Activity
   - `androidx.compose.*`: Componentes de Jetpack Compose para la UI
   - `androidx.constraintlayout:constraintlayout`: Para layouts XML restrictivos

2. **Networking**:
   - `com.squareup.retrofit2:retrofit`: Cliente HTTP para Android
   - `com.squareup.retrofit2:converter-gson`: Convertidor JSON para Retrofit
   
3. **Testing**:
   - `junit`: Para pruebas unitarias
   - `androidx.test.ext:junit`: Extensiones de JUnit para Android
   - `androidx.test.espresso:espresso-core`: Para pruebas de UI

### Backend (Spring Boot)

1. **Spring Boot Framework**:
   - `spring-boot-starter-web`: Para construir aplicaciones web, incluido RESTful
   - `spring-boot-starter-data-jpa`: Para persistencia de datos con JPA
   - `spring-boot-devtools`: Herramientas de desarrollo (recarga automática, etc.)
   
2. **Base de Datos**:
   - `h2`: Base de datos en memoria para desarrollo
   
3. **Testing**:
   - `spring-boot-starter-test`: Utilidades para testing de Spring Boot
   - `junit-platform-launcher`: Plataforma para ejecución de pruebas

## Cómo Funciona

1. El usuario presiona el botón "Obtener Mensaje" en la aplicación Android
2. La aplicación muestra un indicador de carga
3. Se realiza una solicitud HTTP GET al endpoint `/hello` del servidor
4. El servidor procesa la solicitud y devuelve un objeto JSON con un mensaje y estado
5. La aplicación Android deserializa la respuesta JSON a un objeto `HelloResponse`
6. La UI se actualiza para mostrar el mensaje y estado recibidos

Este flujo simple demuestra los conceptos fundamentales de la comunicación cliente-servidor en una aplicación móvil.
