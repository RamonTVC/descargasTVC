# Descargas TVC — redirector en Netlify

## Cómo funciona
- El botón de descarga en el foro apunta a algo como:
  `https://descargastvc.netlify.app/zkbiocvsecurity`
  (nota: el dominio de Netlify es `.netlify.app`, no `.netlify.com`)
- Netlify no encuentra un archivo real con ese nombre, así que la regla en
  `_redirects` reescribe la petición hacia `download.html`, **sin cambiar**
  lo que se ve en la barra de direcciones.
- `download.html` lee el slug de la URL (`zkbiocvsecurity`, `biotimepro`, etc.),
  busca ese slug en `downloads.json`, muestra el mensaje "espere 3 segundos"
  y luego redirige al usuario a la URL real del archivo.
- La URL real del servidor de descargas nunca se muestra en el foro ni en el
  código visible de la página del foro — solo aparece en `downloads.json`,
  que vive en el sitio de Netlify.

## Para desplegar
1. Arrastra esta carpeta completa a https://app.netlify.com/drop
   (o conéctala como repositorio Git en Netlify para desplegar automáticamente).
2. Netlify te dará una URL tipo `algo-random.netlify.app`; puedes ponerle un
   nombre fijo en Site settings → Change site name (ej. `descargastvc`),
   o conectar tu propio dominio.

## Para agregar o actualizar un software
Solo edita **`downloads.json`** y agrega/actualiza una entrada:

```json
"nombreslug": {
  "name": "Nombre visible",
  "version": "1.2.3",
  "url": "https://tu-servidor-real.com/ruta/archivo.zip"
}
```

- `nombreslug` es lo que va después de la `/` en el enlace del botón del foro.
- No necesitas tocar `download.html` ni `_redirects` para nada de esto.
- Vuelve a subir/desplegar el sitio (o si usas Git, con el commit basta).

## Botones en el foro
```html
<a href="https://descargastvc.netlify.app/zkbiocvsecurity" target="_blank" rel="noopener">
  Descargar ZKBio CVSecurity
</a>

<a href="https://descargastvc.netlify.app/biotimepro" target="_blank" rel="noopener">
  Descargar BioTime Pro
</a>
```

## Nota sobre "ocultar" la URL
Este método oculta la URL real del servidor de descargas en el **enlace del
foro y en el código visible del foro**. Sin embargo, un usuario que abra las
herramientas de desarrollador del navegador durante los 3 segundos, o que
revise el archivo `downloads.json` directamente (es público, como todo en un
sitio estático), sí podría ver la URL real. Si necesitas ocultarla también de
ahí, la única forma robusta es que la redirección la haga un backend/servidor
(no un sitio estático), consultando la URL real en el momento y sirviendo el
archivo o un enlace de un solo uso.
