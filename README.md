# Directorio Dolorense

Actividad guiada **#4 · Consumo de una API local desde Flutter**
Desarrollo Móvil Integral · UTNG · Prof. Anastacio Rodríguez García

**Autor(es):** _Ramirez Lopez Maria Yazmin_
**Grupo:** _GTID241_
**Fecha:** 6 de noviembre de 2026

---

## Descripción

Directorio de negocios de Dolores Hidalgo. Una API REST local hecha con Node.js guarda la lista de negocios, y una app Flutter la consulta, agrega negocios, marca favoritos y elimina. Todo se desarrolló con **TDD** (pruebas primero) usando `MockClient`, así que las pruebas no dependen de red ni de servidor.

## Estructura del proyecto

```
directorio_dolores/
├── api/
│   └── server.js            <- API en Node (sin dependencias)
└── app/
    ├── lib/
    │   ├── dominio/         <- Negocio y excepciones (Dart puro)
    │   ├── datos/           <- Cliente HTTP y configuración de URL
    │   ├── presentacion/    <- ViewModel y pantalla
    │   └── main.dart
    └── test/
        ├── negocio_test.dart
        ├── negocios_api_test.dart
        └── viewmodel_test.dart
├── capturas/
│   
└── README.md
  
```

## Requisitos

- Flutter instalado (`flutter doctor` sin errores graves)
- Node.js 18 o superior (`node --version`)
- Emulador Android, o celular en la misma red Wi-Fi que la computadora

## Cómo ejecutarlo

**1. Encender la API** 

```
cd api
node server.js
```

Debe mostrar `API lista en http://localhost:3000/negocios`.

**2. Instalar dependencias y correr las pruebas** 

```
cd app
flutter pub get
flutter test
```

**3. Correr la app:**

```
flutter run
```

Para un celular real, usa la IP de tu computadora:

```
flutter run --dart-define=API_URL=http://TU-IP:3000
```



## Evidencias

### 1. API funcionando

Servidor encendido en la terminal.

![API encendida](captura --> 01_api.png)

### 2. Pruebas pasando (TDD)

`flutter test` con las 19 pruebas del modelo, el cliente HTTP y el ViewModel.

![Pruebas](captura --> 02_tests.png)

### 3. Estado "listo": lista de negocios

La app consume `GET /negocios` y muestra los negocios.

![Lista](captura --> 03_lista.png)

### 4. Agregar un negocio (POST)

![Agregar](capturas --> 04_agregar.png)

### 5. Marcar favorito (PUT)

![Favorito](capturas --> 05_favorito.png)

### 6. Eliminar un negocio (DELETE)

![Eliminar](capturas --> 06_eliminar.png)

### 7. Estado de error sin conexión

Con la API apagada, la app muestra un mensaje claro y el botón **Reintentar**.

![Error](capturas --> 07_error.png)

### 8. Recuperación

Con la API encendida otra vez, al pulsar **Reintentar** la lista regresa.

![Recuperación](capturas --> 08_recuperacion.png)

### 9. Registro de peticiones en el servidor

La terminal de la API muestra cada petición `GET`, `POST`, `PUT` y `DELETE` que envía la app.

![Peticiones](capturas --> 09_peticiones.png)

---

## Retos resueltos

- **Ronda 1:** `GET /negocios/99` responde **404** y `POST` sin categoría responde **400**.
- **Ronda 3:** prueba de `actualizar()` que verifica el método PUT, la ruta `/negocios/ID` y el campo `favorito`.
- **Ronda 5:** prueba de que `agregar()` con un error 400 deja el estado en `error` y el mensaje contiene "400".

## Checklist de calidad

- [x] La URL base no está escrita "a fuego": se configura por plataforma y con `--dart-define`.
- [x] Todos los fallos técnicos se traducen a excepciones con mensajes comprensibles.
- [x] Hay timeout en cada petición.
- [x] Se revisa el `statusCode`: un 404 o 500 nunca se trata como datos.
- [x] Las pruebas usan `MockClient`: no dependen de red ni de servidor.
- [x] La pantalla cubre los cinco estados (inicial, cargando, listo, vacío y error).
- [x] El tráfico HTTP sin cifrar está habilitado **solo** para desarrollo.

## Preguntas para discutir

1. **¿Por qué inyectamos el `http.Client`?** Para poder darle un `MockClient` en las pruebas y controlar exactamente qué respuesta llega, sin red ni servidor.
2. **¿Ventaja del 201 al crear?** Indica que se creó un recurso nuevo, no solo que la petición salió bien (200); el cliente puede distinguir ambos casos.
3. **¿Y si dos celulares borran el mismo negocio a la vez?** El primero recibe 204; el segundo recibe 404, porque ya no existe.
4. **¿Qué cambiaría con un servidor real con HTTPS?** Solo la URL base (`https://...`) y se quitaría `usesCleartextTraffic` del manifiesto.
