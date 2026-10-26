# Mis Finanzas: instrucciones

App de finanzas personales que funciona en el navegador del celular, sin instalar nada, y que se puede agregar a la pantalla de inicio como app (PWA). Todo se guarda en tu dispositivo.

## Archivos

Sube **todos** a la misma carpeta, respetando la subcarpeta `icons`:

```
index.html          ← la app completa (HTML, CSS y JavaScript en un solo archivo)
manifest.json       ← nombre, colores e íconos para "agregar a inicio"
sw.js               ← service worker: guarda la app para que abra sin internet
icons/
  icon-192.png
  icon-512.png
  icon-maskable-512.png
  apple-touch-icon.png
```

`index.html` también funciona solo (por ejemplo, abriéndolo con doble clic), pero así no tendrás modo sin conexión ni podrás instalarla. Para eso necesita publicarse en una dirección `https://`, como GitHub Pages.

---

## Publicarla gratis en GitHub Pages (desde la computadora, sin instalar nada)

1. **Crea una cuenta** en [github.com](https://github.com) (gratis) e inicia sesión.
2. Arriba a la derecha toca **+** y luego **New repository**.
   - *Repository name*: por ejemplo `finanzas`.
   - Déjalo como **Public** (GitHub Pages gratis requiere repositorio público; tus datos **no** se suben, solo el código).
   - Marca **Add a README file** y toca **Create repository**.
3. En el repositorio, toca **Add file** y luego **Upload files**.
4. Arrastra a la ventana los archivos `index.html`, `manifest.json`, `sw.js` **y la carpeta `icons` completa** (en Chrome o Edge puedes arrastrar la carpeta tal cual).
   - Si tu navegador no deja arrastrar carpetas: sube primero los tres archivos. Luego toca **Add file → Create new file**, escribe como nombre `icons/leeme.txt`, pon cualquier texto y toca **Commit changes**. Entra a la carpeta `icons` que se creó y ahí usa **Add file → Upload files** para subir los 4 PNG.
5. Abajo toca **Commit changes**.
6. Ve a **Settings** (pestaña del repositorio) y luego **Pages** en el menú izquierdo.
7. En *Build and deployment* → *Source* elige **Deploy from a branch**. En *Branch* elige **main** y la carpeta **/ (root)**. Toca **Save**.
8. Espera 1 o 2 minutos y recarga la página de *Pages*. Aparecerá tu dirección, algo como:
   `https://tu-usuario.github.io/finanzas/`
9. Abre esa dirección en tu celular.

### Alternativa aún más rápida: Netlify Drop

Entra a [app.netlify.com/drop](https://app.netlify.com/drop), arrastra la carpeta completa (la que contiene `index.html`) y te da una dirección `https://` al instante. Crea una cuenta gratis para que el sitio no se borre.

---

## Agregarla a la pantalla de inicio

### Android (Chrome)
1. Abre tu dirección en **Chrome**.
2. Toca el menú **⋮** (arriba a la derecha).
3. Elige **Instalar app** o **Agregar a la pantalla principal**.
4. Confirma. Aparecerá el ícono "Finanzas" y abrirá a pantalla completa, también sin internet.

### iPhone (Safari)
1. Abre tu dirección en **Safari** (en iPhone solo Safari permite agregarla como app).
2. Toca el botón **Compartir** (cuadro con flecha hacia arriba).
3. Desliza y toca **Agregar a inicio**.
4. Toca **Agregar**.

> **Importante en iPhone:** si usas la app solo dentro de Safari (sin agregarla a inicio), iOS puede borrar los datos de sitios que no visitas en 7 días. Agregada a la pantalla de inicio no tiene ese límite. Aun así, haz respaldos.

---

## Primer uso

Al abrirla por primera vez aparece un asistente de 6 pasos (cada uno se puede saltar):
tu nombre y formato → cuentas y saldos → ingresos → cómo medir periodos → regla de ahorro → metas.
También puedes tocar **Ver primero un ejemplo** para explorar con datos ficticios y borrarlos después en **Ajustes → Datos y respaldo**.

---

## Tus datos y respaldos

- Todo se guarda en el navegador de **ese** dispositivo (localStorage). No hay cuentas ni servidor; nadie más ve tu información.
- Si borras los datos del navegador, desinstalas la app o cambias de teléfono, **se pierden** a menos que tengas un respaldo.
- **Ajustes → Datos y respaldo → Exportar respaldo (JSON)** descarga un archivo; en el celular puedes guardarlo en Archivos, Drive o mandártelo por correo.
- Para pasar tus datos a otro teléfono: exporta en el viejo y en el nuevo usa **Importar respaldo**.
- **Exportar movimientos (CSV)** genera un archivo que abre en Excel o Google Sheets.
- La app te recuerda respaldar cada 30 días (puedes cambiarlo).

---

## Notificaciones

Actívalas en **Ajustes → Presupuestos y recordatorios**. Como la app no usa servidor, los avisos se envían **cuando abres la app o mientras está abierta**, no con el teléfono bloqueado y la app cerrada. Además, siempre verás un aviso dentro de la app con tus pagos próximos al abrirla.
En iPhone, las notificaciones solo funcionan con la app agregada a inicio y iOS 16.4 o posterior.

---

## Actualizar la app más adelante

1. Sube el `index.html` nuevo a GitHub (**Add file → Upload files**, reemplaza el anterior).
2. En `sw.js` cambia la línea `const VERSION = 'v1.5.0';` por otra versión, por ejemplo `'v1.5.1'`, y súbelo también.
3. Al abrir la app aparecerá "Hay una versión nueva" con el botón **Actualizar**. Tus datos no se tocan.

---

## Personalizar el código

Toda la configuración por defecto está en un solo objeto, `CONFIG`, al inicio del `<script>` de `index.html`:

- `CONFIG.defaults`: valores iniciales de Ajustes (moneda, formato de fecha, periodo, regla de ahorro, fines de semana, recordatorios, tema, color principal, tarjetas de Inicio, proyección…).
- `CONFIG.categories`: categorías y subcategorías iniciales con su ícono y color.
- `CONFIG.currencies`, `CONFIG.palette`, `CONFIG.primaryColors`, `CONFIG.icons`: listas para los selectores (los íconos están dibujados en el objeto `GLYPHS`).
- `CONFIG.debtPlans`: nombres de las formas de pagar una deuda en el simulador (próximo cobro, dividir entre cobros, fecha límite, no pagar).
- `CONFIG.tips`: umbrales de las recomendaciones (porcentaje de ahorro, gastos fijos, meses de fondo de emergencia, uso de tarjetas, monto de "gastos hormiga").
- `CONFIG.limits`: límites de seguridad (horizonte máximo de la proyección, fechas en la vista previa).

Cambiar `CONFIG.defaults` afecta solo a instalaciones nuevas o a opciones que el usuario nunca ha modificado; lo que cada persona cambia en Ajustes tiene prioridad.

Los días festivos oficiales de México se calculan en la función `mxHolidays()` (Ley Federal del Trabajo, art. 74). La lista es editable desde **Ajustes → Fechas, fines de semana y festivos**, donde también puedes agregar los días inhábiles bancarios (Jueves y Viernes Santo, 2 de noviembre y 12 de diciembre).

---

## Si algo no funciona

- **No aparece "Instalar" / "Agregar a inicio":** verifica que la dirección empiece con `https://` y que `manifest.json`, `sw.js` y la carpeta `icons` estén junto a `index.html`.
- **No veo los cambios que subí:** sube el número de `VERSION` en `sw.js`; cierra y vuelve a abrir la app.
- **El ícono sale en blanco:** revisa que los PNG estén dentro de `icons/` con esos nombres exactos.
- **Quiero empezar de cero:** **Ajustes → Datos y respaldo → Reiniciar la app desde cero** (exporta un respaldo antes).
