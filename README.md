# Publigital — Sitio web oficial

Sitio web bilingüe de **Publigital**, agencia de videos publicitarios con inteligencia artificial. Operamos **100% en línea** para micronegocios, pymes y emprendedores de **todo el mundo**.

| | |
|---|---|
| 🇲🇽 **Español** | https://superyeyo8482.github.io/publigital/ |
| 🇬🇧 **English** | https://superyeyo8482.github.io/publigital/index_en.html |
| 📦 **Repositorio** | https://github.com/superyeyo8482/publigital |

---

## ¿Qué incluye?

| Sección | Contenido |
|---|---|
| **Hero** | Titular, propuesta de valor, aviso de cobertura global y llamados a la acción |
| **Servicios** | Los 3 formatos de video: 30s, 60s y 90s, con precios en MXN y USD |
| **Precios** | Tabla con precios en MXN y USD, y botón de pago con cripto por plan |
| **Video** | Video promocional en marco de teléfono (formato vertical 9:16) |
| **Proceso** | Los 3 pasos: solicitud → vista previa con marca de agua → pago y entrega |
| **Testimonios** | Espacio para reseñas de clientes de distintos países |
| **Contacto** | WhatsApp y Telegram de Aurelio y Alejandro + cotizador + cobertura global |
| **Footer** | Redes sociales, navegación, contacto y frase de marca |

**Todo el sitio está disponible en español y en inglés**, con un botón para cambiar de idioma en la barra superior.

---

## Estructura de archivos

```
publigital/
├── index.html                    ← Versión en ESPAÑOL
├── index_en.html                 ← Versión en INGLÉS
├── styles.css                    ← Hoja de estilos COMPARTIDA por ambos idiomas
├── README.md                     ← Este archivo
├── logo.png                      ← Logotipo
├── poster.jpg                    ← Portada del video (generada con ffmpeg)
├── promo_publigital_web.mp4      ← Video promocional optimizado (540×960, 4.8 MB)
├── promo_publigital.mp4          ← Video original 1080×1920 (42 MB, NO se sube al repo)
├── contacto.txt                  ← Nota interna (NO se sube al repo)
├── .nojekyll                     ← Evita el procesado de Jekyll en GitHub Pages
└── .gitignore
```

> 📁 **Ubicación local del proyecto:** `C:\Proyectos\publigital`
>
> Está a propósito **fuera de `Documentos` y fuera de OneDrive**. Un repositorio git nunca debe vivir dentro de una carpeta sincronizada en la nube: OneDrive no entiende de git y puede crear copias en conflicto o dañar el historial. El respaldo correcto es GitHub, que ya lo tienes.

### ¿Por qué el CSS está en un archivo aparte?

Porque el sitio es bilingüe. Si los estilos vivieran dentro de cada HTML, habría **dos copias** del mismo CSS y cualquier cambio de diseño habría que hacerlo dos veces (con riesgo de que las versiones se desincronicen). Con `styles.css` se edita **una sola vez** y afecta a los dos idiomas.

---

## 🌎 Cómo funciona el sistema bilingüe

- `index.html` → español. Botón **🇬🇧 English** en la barra superior.
- `index_en.html` → inglés. Botón **🇲🇽 Español** en la barra superior.
- Ambos archivos incluyen etiquetas `hreflang`, que le dicen a Google que existen dos versiones de la misma página y en qué idioma está cada una. Esto mejora el SEO internacional.

> ⚠️ **Al editar contenido, hazlo en los DOS archivos.** Si cambias un precio en `index.html` pero no en `index_en.html`, las versiones quedarán desfasadas.

---

## 💰 Precios vigentes

| Formato | Precio | Entrega |
|---|---|---|
| Video 30 segundos | $250 MXN / **$15 USD** | 2 días |
| Video 60 segundos | $350 MXN / **$20 USD** | 2 días |
| Video 90 segundos | $400 MXN / **$25 USD** | 2 días |

> 💡 **No hay anticipo.** Se paga cuando el cliente ya vio su video (ver el flujo de pago más abajo).

Los precios aparecen en **cuatro lugares** que debes mantener sincronizados en cada archivo:

1. La sección **Servicios** (`.svc-price`) — tres tarjetas
2. La tabla de **Precios** (`<tbody>`) — tres filas
3. El cotizador (`<select id="qFormato">`) — tres opciones
4. El objeto **`PAGOS`** en el JavaScript — los títulos de los modales de pago

---

## 💳 Pagos con criptomonedas (USDT)

Los pagos se procesan con **NOWPayments** mediante **enlaces de pago alojados**. No se necesita ninguna API Key en el sitio, lo cual es obligatorio porque GitHub Pages es hosting estático y todo su código es público.

### Los 3 enlaces configurados

| Plan | Monto | ID de pago (`iid`) |
|---|---|---|
| Video 30 segundos | $15 USD | `4816081322` |
| Video 60 segundos | $20 USD | `4594712613` |
| Video 90 segundos | $25 USD | `6331297296` |

### Cómo funcionan los botones

Cada plan tiene un botón **"💳 Pagar con cripto (USDT)"** (en la tabla de precios y en el panel lateral). Al hacer clic se abre un **modal** con:

- El título con la duración y el precio: *"Pagar Video de 30 segundos — $15 USD"*
- El **widget de pago embebido** de NOWPayments (iframe 410×696)
- Un botón de **cerrar**
- Un enlace de respaldo para abrir el pago en una pestaña nueva, por si el widget no carga

El widget usa **carga diferida**: solo se descarga cuando el usuario abre el modal, así la página no carga tres iframes de pago al entrar. El modal se cierra con el botón, haciendo clic fuera o con la tecla `Escape`.

### Para cambiar un enlace de pago

Abre el archivo del idioma correspondiente, busca el objeto `PAGOS` (cerca del final, en el `<script>`) y edita la línea que necesites:

```javascript
const PAGOS = {
  v30: {
    titulo: 'Pagar Video de 30 segundos — $15 USD',
    url:   'https://nowpayments.io/payment/?iid=4816081322',          // enlace directo
    embed: 'https://nowpayments.io/embeds/payment-widget?iid=4816081322'  // widget embebido
  },
  ...
};
```

> 🔁 Recuerda cambiar también `titulo` para que el modal muestre el precio correcto.

### 🔄 El flujo de pago (vista previa con marca de agua)

Este es el flujo acordado, y está escrito así en las dos versiones del sitio:

1. **Se acuerda todo por WhatsApp o Telegram.** No se paga nada al inicio. Esto filtra curiosos y permite confirmar el brief antes de que el cliente pague.
2. **Producimos el video** (2 días).
3. **Se lo enviamos terminado, pero con marca de agua y en baja resolución.** El cliente ve exactamente lo que va a recibir.
4. **El cliente paga** con el widget de USDT ($15, $20 o $25 USD según el formato).
5. **Le enviamos el archivo final** en alta calidad y sin marca de agua.

**¿Por qué así y no entregando el archivo limpio antes de cobrar?**
Un video es un archivo digital: en cuanto se envía, el cliente ya lo tiene y no tiene incentivo para pagar. Como ahora atendemos a todo el mundo, reclamar a alguien en otro país no es viable. La vista previa con marca de agua le da al cliente la confianza de *"veo el producto terminado antes de pagar"* **sin regalar el trabajo**. Además genera urgencia por quitar la marca.

> 📌 Si algún día quieres cambiar de modelo (por ejemplo volver al anticipo del 50%), hay que actualizar el texto en **los dos archivos** y quitar la columna de precios correspondiente.

### ⚠️ Dos cosas importantes

**1. Nunca pongas tu API Key de NOWPayments en el HTML.**
Este archivo es público: cualquiera puede abrir el código fuente y copiar la clave, y con ella generar cobros a tu nombre. La API Key solo se usa desde un **servidor** (por ejemplo una función serverless en Cloudflare Workers o Vercel). Los enlaces de pago alojados no necesitan clave.

**2. El pago se hace DESPUÉS de que el cliente ve su video.** Ver el flujo completo abajo.

---

## ✏️ Cómo actualizar el contenido

Abre el archivo del idioma que quieras editar con cualquier editor de texto y busca lo que necesites cambiar.

### Agregar reseñas reales

Busca el comentario `CÓMO AGREGAR RESEÑAS REALES` (o `HOW TO ADD REAL REVIEWS` en inglés). Reemplaza el texto de `.tst-quote`, las iniciales de `.tst-avatar`, el nombre en `.tst-name` y el giro en `.tst-role`.

Los testimonios están marcados por país: México, España y Estados Unidos.

> 🚫 **Nunca publiques testimonios inventados.** Es publicidad engañosa y puede acarrear problemas legales.

### Datos de contacto

| Persona | Teléfono | WhatsApp | Telegram |
|---|---|---|---|
| Aurelio | +52 55 4653 9933 | [wa.me/525546539933](https://wa.me/525546539933) | [t.me/+525546539933](https://t.me/+525546539933) |
| Alejandro | +52 1 55 2253 5418 | [wa.me/5215522535418](https://wa.me/5215522535418) | [t.me/+5215522535418](https://t.me/+5215522535418) |

---

## 🎬 Sobre los dos archivos de video

| Archivo | Resolución | Peso | ¿Va al repo? |
|---|---|---|---|
| `promo_publigital.mp4` | 1080×1920 | 41.7 MB | ❌ No (excluido por `.gitignore`) |
| `promo_publigital_web.mp4` | 540×960 | 4.8 MB | ✅ Sí (el que usa el sitio) |

El original en alta se conserva en tu computadora por si algún día lo necesitas para otro uso (por ejemplo, subirlo a YouTube o a un anuncio pagado). La versión web pesa **casi 9 veces menos** y se ve nítida, porque el reproductor se muestra a 310 px de ancho como máximo.

Para regenerar la versión web después de editar el video original:

```bash
ffmpeg -y -i promo_publigital.mp4 \
  -c:v libx264 -preset slow -crf 30 -r 24 \
  -vf "scale=540:960" \
  -profile:v main -level 3.1 -pix_fmt yuv420p \
  -c:a aac -b:a 64k -ac 1 \
  -movflags +faststart \
  promo_publigital_web.mp4
```

El video se carga con `preload="none"`, así que **no consume datos del visitante hasta que presiona reproducir**.

---

## 👀 Previsualizar el sitio en tu computadora

**Opción A — la más simple:** haz doble clic en `index.html`. Se abrirá en tu navegador.

> ⚠️ Abriéndolo así, algunos navegadores bloquean la carga del video por seguridad. Para verlo todo bien, usa la Opción B.

**Opción B — con servidor local** (recomendado):

```bash
# Con Node.js
npx serve .

# O con Python
python -m http.server 8899
```

Luego abre `http://localhost:8899` (o el puerto que indique la herramienta).

---

## 🚀 Publicar cambios en GitHub Pages

```bash
git add .
git commit -m "Descripción del cambio"
git push
```

GitHub Pages reconstruye el sitio automáticamente en **1 o 2 minutos**. Recarga con `Ctrl + F5` para saltarte la caché del navegador.

> 💡 Si el push falla con un error `408` o `Connection reset`, es un problema de red intermitente con los archivos grandes. **Vuelve a intentarlo**: normalmente funciona en el segundo o tercer intento.

---

© Publigital. Todos los derechos reservados.
