# NYst — landing page (proyecto académico)

Sitio estático de varias páginas para la marca ficticia NYst, hecho para un
proyecto universitario. No procesa pagos reales ni recolecta datos
personales reales.

## Estructura del proyecto

```
index.html         Portada: hero, tablero de edición (Board) y About
presale.html        Tablero de edición + Presale
stock.html           Tablero de edición + Remaining stock
gallery.html         Gallery (runways, storylines, celebs)
collections.html    Collections
styles.css            Todos los estilos, compartidos por las 5 páginas
app.js                Toda la lógica, compartida por las 5 páginas
README-backend.txt   Cómo activar el contador de stock compartido entre dispositivos
```

No hay build ni dependencias: son archivos estáticos planos. Para verlo
localmente basta con abrir `index.html` en el navegador, siempre y cuando
los demás archivos (`styles.css`, `app.js`) estén en la misma carpeta.

## Contador de stock compartido (opcional)

Por defecto, el contador de unidades vendidas por prenda vive en el
`localStorage` de cada navegador (cada dispositivo ve su propio número).
Para que el contador sea el mismo en cualquier dispositivo, hay soporte
opcional para un backend gratuito (Firebase Realtime Database). Ver
`README-backend.txt` para la guía paso a paso.

## Publicar el sitio

Cualquiera de estas opciones es gratuita:

- **GitHub Pages** (requiere que el repositorio sea público): Settings →
  Pages → Branch `main`, carpeta `/ (root)`.
- **Netlify / Vercel / Cloudflare Pages**: conectas el repositorio
  (puede ser privado) y seleccionas "no build command, publish directory:
  /". Te dan una URL pública al instante.

## Controles de demo

El footer tiene un bloque desplegable "Class demo controls" para simular
el cierre de la preventa, avanzar el stock restante, o resetear todos los
datos guardados — pensado solo para la sustentación del proyecto.
