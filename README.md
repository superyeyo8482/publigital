# Publidigital — Sitio web oficial

Landing page de **Publidigital**, agencia de videos publicitarios con inteligencia artificial para micronegocios, pymes y emprendedores de **Toluca, Metepec y zona metropolitana**.

🌐 **Sitio publicado:** https://superyeyo8482.github.io/publidigital/

---

## ¿Qué incluye?

| Sección | Contenido |
|---|---|
| **Hero** | Titular, propuesta de valor y llamados a la acción |
| **Servicios** | Los 3 formatos de video: 30s, 60s y 90s |
| **Precios** | Tabla de precios, anticipo del 50% y botones de pago |
| **Video** | Video promocional en marco de teléfono (formato vertical 9:16) |
| **Proceso** | Los 3 pasos: solicitud → anticipo → entrega en 2 días |
| **Testimonios** | Espacio para reseñas de clientes |
| **Contacto** | WhatsApp y Telegram de Aurelio y Alejandro + cotizador |
| **Footer** | Redes sociales, navegación y datos de contacto |

---

## Características técnicas

- **Un solo archivo.** Todo el HTML, CSS y JavaScript vive en `index.html`. No hay dependencias, ni build, ni npm.
- **Sin frameworks.** Carga instantánea y cero mantenimiento de dependencias.
- **Responsive.** Adaptado a móvil, tablet y escritorio.
- **Diseño oscuro.** Fondo `#0a0a1a`, acentos dorados `#c9a84c` y azules `#00d4ff`.
- **Tipografía Inter** (Google Fonts).
- **Accesible.** Etiquetas ARIA, foco visible y soporte para `prefers-reduced-motion`.
- **SEO local.** Datos estructurados JSON-LD (`LocalBusiness`) con precios y zona de cobertura.

---

## Estructura de archivos

```
publidigital/
├── index.html                    ← Todo el sitio (HTML + CSS + JS)
├── README.md                     ← Este archivo
├── logo.png                      ← Logotipo
├── poster.jpg                    ← Portada del video (generada con ffmpeg)
├── promo_publidigital_web.mp4    ← Video promocional optimizado (720×1280, 9.6 MB)
├── promo_publidigital.mp4        ← Video original 1080×1920 (42 MB, NO se sube al repo)
├── contacto.txt                  ← Nota interna (NO se sube al repo)
├── .nojekyll                     ← Evita el procesado de Jekyll en GitHub Pages
└── .gitignore
```

### Sobre los dos archivos de video

| Archivo | Resolución | Peso | ¿Va al repo? |
|---|---|---|---|
| `promo_publidigital.mp4` | 1080×1920 | 41.7 MB | ❌ No (excluido por `.gitignore`) |
| `promo_publidigital_web.mp4` | 720×1280 | 9.6 MB | ✅ Sí (el que usa el sitio) |

El original en alta se conserva en tu computadora por si algún día lo necesitas para otro uso (por ejemplo, subirlo a YouTube o a un anuncio pagado). La versión web es la que se muestra en la página: se ve nítida en pantallas de celular y carga **4 veces más rápido**.

Si quieres regenerar la versión web después de editar el video original:

```bash
ffmpeg -y -i promo_publidigital.mp4 \
  -c:v libx264 -preset medium -crf 28 \
  -vf "scale=720:1280" \
  -profile:v high -level 4.0 -pix_fmt yuv420p \
  -c:a aac -b:a 96k -ac 2 \
  -movflags +faststart \
  promo_publidigital_web.mp4
```

El video se carga con `preload="none"`, así que **no consume datos del visitante hasta que presiona reproducir**. Eso mantiene la página ligera aunque el video sea grande.

---

## ✏️ Cómo actualizar el contenido

Abre `index.html` en cualquier editor de texto y busca lo que quieras cambiar. Los precios aparecen en **tres lugares** que debes mantener sincronizados:

1. La sección **Servicios** (`.svc-price`) — tres tarjetas
2. La tabla de **Precios** (`<tbody>`) — tres filas
3. El objeto **`PLANES`** en el JavaScript — al final del archivo

> ⚠️ Si cambias un precio, cámbialo en los tres lugares. Si no, la página mostrará información contradictoria.

### Agregar reseñas reales

Busca el comentario `CÓMO AGREGAR RESEÑAS REALES` en la sección de testimonios. Reemplaza el texto de `.tst-quote`, las iniciales en `.tst-avatar`, el nombre en `.tst-name` y el giro en `.tst-role`.

> 🚫 **Nunca publiques testimonios inventados.** Es publicidad engañosa y puede acarrear problemas legales.

---

## 💳 Configurar los pagos con NOWPayments

Los botones de "Pagar anticipo" funcionan con **enlaces de factura alojada (hosted invoice links)**. Esto significa que **no se necesita ninguna API Key en el sitio**, lo cual es obligatorio porque GitHub Pages es hosting estático y todo su código es público.

### Pasos

1. Entra a tu panel de **NOWPayments** → *Payment Links* / *Invoices*.
2. Crea **tres enlaces de pago**, uno por cada anticipo:
   - Video 30s → **$125 MXN**
   - Video 60s → **$175 MXN**
   - Video 90s → **$200 MXN**
3. Abre `index.html`, busca `NOWPAYMENTS_LINKS` (cerca del final) y pega cada enlace:

```javascript
const NOWPAYMENTS_LINKS = {
  v30: 'https://nowpayments.io/payment/?iid=XXXXXXX',  // 30s - $125 MXN
  v60: 'https://nowpayments.io/payment/?iid=YYYYYYY',  // 60s - $175 MXN
  v90: 'https://nowpayments.io/payment/?iid=ZZZZZZZ'   // 90s - $200 MXN
};
```

4. Guarda, haz commit y push. Los botones abrirán el pago automáticamente.

**Mientras estos campos estén vacíos**, el botón de pago abre WhatsApp con el pedido ya escrito. Así el sitio nunca pierde una venta, incluso antes de configurar los pagos.

### 🔒 Sobre la API Key

> **NUNCA pongas tu API Key de NOWPayments dentro de `index.html`.**
>
> Este archivo es público: cualquiera puede abrir el código fuente del sitio y copiar la clave. Con ella podrían consultar tus pagos y generar cobros a tu nombre.
>
> La API Key solo se usa desde un **servidor**. Si en el futuro quieres generar facturas automáticas por API (monto variable, datos del cliente), la clave debe vivir en una función serverless (Cloudflare Workers, Vercel Functions o Netlify Functions) que reciba la petición del navegador, llame a NOWPayments y devuelva únicamente la URL de la factura.

---

## 👀 Previsualizar el sitio en tu computadora

**Opción A — la más simple:** haz doble clic en `index.html`. Se abrirá en tu navegador.

**Opción B — con servidor local** (recomendado; reproduce mejor el comportamiento real):

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

GitHub Pages reconstruye el sitio automáticamente en **1 o 2 minutos**. Recarga la página con `Ctrl + F5` para saltarte la caché del navegador.

---

## 📞 Contacto

| Persona | Teléfono | WhatsApp | Telegram |
|---|---|---|---|
| Aurelio | +52 55 4653 9933 | [wa.me/525546539933](https://wa.me/525546539933) | [t.me/+525546539933](https://t.me/+525546539933) |
| Alejandro | +52 1 55 2253 5418 | [wa.me/5215522535418](https://wa.me/5215522535418) | [t.me/+5215522535418](https://t.me/+5215522535418) |

---

## 💰 Precios vigentes

| Formato | Precio | Anticipo (50%) | Entrega |
|---|---|---|---|
| Video 30 segundos | $250 MXN | $125 MXN | 2 días |
| Video 60 segundos | $350 MXN | $175 MXN | 2 días |
| Video 90 segundos | $400 MXN | $200 MXN | 2 días |

---

© Publidigital. Todos los derechos reservados.
