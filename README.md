# Sabor Catracho – Asistente de compras con IA

Asistente conversacional para **Sabor Catracho**, una tienda en línea de chucherías y productos típicos hondureños dirigida a familias que quieren enviar productos a sus seres queridos en Estados Unidos. El asistente recomienda productos del catálogo, calcula totales según el presupuesto del cliente y permite agregar los productos a la carreta con un clic.

**Sitio en vivo:** https://saborcatrachoexpress.netlify.app
**Proyecto integrador** – Inteligencia Artificial Aplicada, Universidad Tecnológica de Honduras.

## Qué hace

- Responde preguntas sobre los 24 productos del catálogo (precios, categorías, presentaciones).
- Sugiere combos según el presupuesto del cliente y muestra tarjetas de producto con botón **Agregar**.
- Si no sabe algo (envíos, pagos, plazos), lo dice y remite al WhatsApp de la tienda.
- Los precios de las tarjetas salen del archivo `data/catalogo.json`, no de lo que escribe el modelo.
- Si el cliente escribe un presupuesto (por ejemplo "L 500"), el código suma los precios y quita los productos que lo excedan, porque el modelo a veces se equivoca sumando.

## Cómo se usa

1. Abre el sitio y toca el botón **💬 Pregúntanos** (abajo a la derecha).
2. Escribe tu pregunta, por ejemplo: *"Busco un regalo para mi mamá, máximo L 500"*.
3. Toca **Agregar** en los productos que te gusten y finaliza el pedido desde la carreta (se envía por WhatsApp).
4. Usa **Limpiar conversación** para empezar de nuevo o **Hablar por WhatsApp** para atención directa.

## Tecnologías

- **Frontend:** HTML, CSS y JavaScript sin frameworks (`public/index.html`). Tipografías Bricolage Grotesque y DM Sans.
- **Backend:** función serverless de Netlify (`netlify/functions/asistente.js`, Node.js).
- **Modelo de IA:** Gemini 3.5 Flash-Lite, a través de la API de Google (plan gratuito).
- **Hosting:** Netlify, conectado a este repositorio de GitHub.
- **Pruebas:** script en Python (`pruebas/pruebas.py`) con 12 casos de prueba.

## Estructura

```
public/index.html                 Tienda + chat (frontend)
netlify/functions/asistente.js    Función que llama a Gemini (la clave vive aquí, en el servidor)
data/catalogo.json                Catálogo de 24 productos
pruebas/pruebas.py                Casos de prueba del asistente
netlify.toml                      Configuración de Netlify
```

## Cómo ejecutarlo localmente

Requisitos: Node.js 18 o superior y una clave de la API de Gemini (https://aistudio.google.com).

```bash
npm install -g netlify-cli
cp .env.example .env        # en Windows: copy .env.example .env
# edita .env y escribe tu clave:  GEMINI_API_KEY=tu_clave
netlify dev
```

Abre la dirección que muestra la terminal (normalmente http://localhost:8888).

Para correr las pruebas (Python 3 y `pip install google-genai`):

```bash
# define la variable GEMINI_API_KEY en tu terminal y luego, desde la raíz del repositorio:
python pruebas/pruebas.py
```

## Seguridad de la clave

La clave **nunca** está en el código ni en el navegador. En producción se guarda como variable de entorno (`GEMINI_API_KEY`) en Netlify; el navegador solo habla con la función `/.netlify/functions/asistente`. El archivo `.env` está en `.gitignore`.

## Limitaciones conocidas

- El asistente puede equivocarse; no sustituye la confirmación del pedido por WhatsApp.
- El límite de mensajes por usuario es básico y no reemplaza un control de costos real.
- Solo reconoce presupuestos escritos como "L 500", "Lps 500" o "500 lempiras".
- Si la API de Gemini falla de forma temporal, el chat muestra un aviso y no reintenta solo.
- No recuerda la conversación al recargar la página.
