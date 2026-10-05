CONTADOR DE STOCK COMPARTIDO ENTRE DISPOSITIVOS
================================================

Por defecto, el contador de unidades reclamadas de cada prenda vive en el
localStorage de cada navegador, así que cada dispositivo ve su propio
número. Esta guía activa un backend gratuito (Firebase Realtime Database)
para que el contador sea el mismo sin importar desde qué computador o
celular entres.

No necesitas tarjeta de crédito ni pagar nada: el plan gratuito de
Firebase (Spark) alcanza de sobra para un proyecto académico.

PASO 1: Crear el proyecto de Firebase
--------------------------------------
1. Entra a https://console.firebase.google.com con una cuenta de Google.
2. "Add project" / "Agregar proyecto" -> ponle un nombre (ej. nyst-demo).
3. Puedes desactivar Google Analytics, no lo necesitas para esto.
4. Espera a que termine de crearse el proyecto.

PASO 2: Crear la Realtime Database
------------------------------------
1. En el menú izquierdo: Build -> Realtime Database.
2. "Create Database".
3. Elige cualquier ubicación (da igual para este proyecto).
4. Cuando pregunte por las reglas de seguridad, elige "Start in test mode"
   por ahora (lo ajustamos en el paso 4).
5. Copia la URL que aparece arriba de la base de datos, algo como:
   https://nyst-demo-default-rtdb.firebaseio.com

PASO 3: Pegar la URL en el sitio
-----------------------------------
Abre app.js y busca este bloque, cerca del inicio del archivo:

    backend: { enabled: false, url: '' },

Cámbialo por:

    backend: { enabled: true, url: 'https://nyst-demo-default-rtdb.firebaseio.com' },

(usa tu propia URL del paso 2). Guarda el archivo y vuelve a subirlo
donde tengas el resto del sitio.

PASO 4: Reglas de acceso
----------------------------
El modo de prueba de Firebase expira solo a los 30 días y luego bloquea
todo. Para que el contador siga funcionando y, al mismo tiempo, nadie
pueda borrar ni dañar los demás datos de la base, entra a
Realtime Database -> Rules y reemplaza el contenido por:

    {
      "rules": {
        "claimed": {
          ".read": true,
          "$pid": {
            ".write": true,
            ".validate": "newData.isNumber() && newData.val() >= 0 && newData.val() <= 40"
          }
        }
      }
    }

Esto permite leer y escribir SOLO la rama "claimed" (el contador de
unidades), limitando cada valor a un número entre 0 y 40. El resto de la
base queda cerrado. Dale a "Publish".

Nota honesta: estas reglas siguen siendo abiertas (cualquiera con la URL
puede escribir un número ahí), lo cual es aceptable para una demo de
clase, pero no es apto para un lanzamiento real con compras de verdad.
Para eso se necesitaría autenticación de usuarios y validación desde un
servidor propio.

CÓMO FUNCIONA
----------------
- Al cargar cualquier página, el sitio consulta el contador compartido y
  actualiza lo que ves en pantalla.
- Cada 8 segundos vuelve a consultarlo, así que si alguien compra algo
  desde otro dispositivo, el contador se actualiza solo, sin recargar.
- Cuando alguien completa una compra, el sitio suma la cantidad comprada
  al contador compartido.
- El carrito, las "mis piezas" (certificados) y la demo del footer
  siguen siendo locales a cada navegador, solo el contador de unidades
  reclamadas por prenda se comparte.
- Si no hay internet o Firebase no responde, el sitio sigue funcionando
  con el último número que alcanzó a guardar localmente.

SIN BACKEND
--------------
Si dejas backend.enabled en false (como viene por defecto), el sitio
funciona exactamente igual que antes: cada navegador lleva su propio
contador. Es la opción más simple si no quieres crear una cuenta de
Firebase para la entrega.
