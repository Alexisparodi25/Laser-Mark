# Laser·Mark

Aplicación móvil (PWA empaquetable como **APK de Android**) que detecta un **puntero láser rojo** con la cámara del celular, guarda la imagen con un **círculo naranja** alrededor del punto y permite **anotar información** en cada foto (título, descripción, lugar y etiquetas), editable cuando se quiera.

> Pensada para **interiores**, con luz moderada y superficies mates.

## Cómo se usa

1. Al abrir la app se enciende la cámara (aceptá el permiso). Si falla, aparece el botón **Activar cámara**.
2. Apuntá el láser rojo a la escena y mantenelo quieto un instante.
3. La app vibra, guarda la foto con el punto marcado y muestra el aviso **Foto guardada**. Tocalo para anotar la información.
4. En **Archivo** están todas las fotos. Al tocar una se abre su ficha: podés editar los datos, descargar la imagen o eliminarla.

## Cómo funciona

1. Cada 50 ms se analiza un cuadro reducido a 480 px de ancho.
2. Se marcan los píxeles muy rojos y se agrupan en manchas.
3. Se descartan las manchas muy chicas, muy grandes o alargadas.
4. Si una mancha queda en el mismo lugar ≥ 0,3 s, se captura la foto (pausa de 2,5 s entre capturas). Si lleva más de 1,5 s quieta, se considera un objeto fijo y se ignora.
5. La imagen completa, con el círculo dibujado, se guarda en el celular (IndexedDB).

## Archivos

```
index.html            pantallas
estilos.css           apariencia
config.js             ajustes de detección y campos de información
detector.js           detección de puntos rojos (función pura)
app.js                cámara, captura, archivo y fichas
sw.js                 modo sin conexión (busca primero la versión nueva)
manifest.webmanifest  datos de la app instalable
icon-192.png, icon-512.png
```

Para agregar un campo a la ficha, sumá una línea en `CAMPOS` dentro de `config.js`. Los tiempos y umbrales de detección también están ahí.

## Publicar y generar el APK

1. Creá un repositorio público y subí todos los archivos **sueltos en la raíz**.
2. En **Settings > Pages** elegí la rama `main` y la carpeta `/ (root)`. La app queda en `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`.
3. En [pwabuilder.com](https://www.pwabuilder.com) pegá esa dirección, elegí **Package for stores > Android** y completá el *Package ID* (por ejemplo `com.tuescuela.lasermark`).
4. Descargá el zip y usá el `.apk` firmado. Guardá también la clave (`signing.keystore`) y su contraseña.
5. Instalá el APK en el celular habilitando "instalar apps de fuentes desconocidas".

Como el APK abre la página publicada, los cambios que subas se reflejan sin generarlo de nuevo.

## Limitaciones

- La detección se probó con imágenes generadas; con un láser real puede requerir ajustar `rojoMin`, `difMin` y `elongMax` en `config.js`.
- Con mucha luz, sol o superficies brillantes la detección es poco fiable.
- Un objeto rojo quieto más de 1,5 s se ignora.
- Las fotos quedan dentro de la app hasta que se descargan. Si se borran los datos de la app o del navegador, se pierden.
- El celular puede abrir la lente gran angular (0.5); la app intenta elegir la principal sola, pero no depende de un menú.

## Problemas frecuentes

- **No abre la cámara:** revisá el permiso en Ajustes > Apps > (la app) > Permisos.
- **No cambia después de actualizar:** el título muestra la versión (`v1`). Cerrá la app por completo y abrila de nuevo. No borres los datos del sitio: se perderían las fotos.

## Autores

Proyecto educativo. Autores: _(completar)_.
