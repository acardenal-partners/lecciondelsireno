# La Lección del Sireno — DELAGALA

Landing de la campaña **«La Lección del Sireno»** de DELAGALA, consultoría inmobiliaria en Las Arenas (Getxo, Bizkaia). Invita a los propietarios de Getxo a pedir una **valoración real de su vivienda**, con un relato de campaña y un vídeo de fondo en la cabecera.

**Para quién:** propietarios de Getxo y alrededores que se plantean vender o simplemente conocer el valor de su casa. Para el equipo, es una pieza de captación independiente de la web principal.

**En vivo:** https://valoramipisocondelagalaengetxo.com (GitHub Pages con dominio propio).

---

## Estado actual (octubre 2026)

- **Último commit:** 10-09-2026 — `Create CNAME` (se restauró el dominio propio).
- Publicada en GitHub Pages desde la rama `main`, carpeta raíz.
- **Formulario de valoración:** la constante `SCRIPT_URL` no está configurada, por lo que al enviar el formulario la página **abre WhatsApp** con los datos (nombre, teléfono y zona) para que el lead no se pierda. Cuando se configure `SCRIPT_URL`, enviará primero a la hoja de leads y solo usará WhatsApp como respaldo si falla la red.

---

## Stack y estructura

Página estática: HTML + CSS + JavaScript sin frameworks ni proceso de build.

```
.
├── index.html       # Toda la landing (estilos, contenido y script del formulario)
├── sireno_hero.mp4  # Vídeo de fondo de la cabecera (~1,6 MB)
└── CNAME            # Dominio propio para GitHub Pages
```

Comportamiento del script (`index.html`, al final del archivo):
- Barra de navegación que cambia de estilo al hacer scroll.
- `enviar()` recoge el formulario, añade el campo `origen` y:
  - si `SCRIPT_URL` empieza por `http`, hace `POST` (modo `no-cors`) a esa URL;
  - si no está configurada o falla, abre una conversación de WhatsApp con los datos.

---

## Cómo arrancarlo desde cero

1. Clonar:
   ```bash
   git clone https://github.com/acardenal-partners/lecciondelsireno.git
   cd lecciondelsireno
   ```
2. Verla en local (recomendado servirla para que el vídeo cargue igual que en producción):
   ```bash
   npx serve .          # o:  python -m http.server 8000
   ```
   También funciona abriendo `index.html` directamente en el navegador.
3. Editar el HTML con cualquier editor (VS Code recomendado). Los colores están en `:root { ... }` al principio del archivo.

---

## Configuración

No hay variables de entorno ni dependencias. Solo dos constantes en el script de `index.html`:

| Nombre | Propósito |
|---|---|
| `SCRIPT_URL` | URL de la aplicación web (Google Apps Script u otro endpoint) que recibe los formularios. Con el valor de marcador actual, el formulario deriva a WhatsApp |
| `ORIGEN` | Etiqueta de procedencia que se envía con cada lead (el dominio de la campaña) |

Para conectar una hoja de Google Sheets: crear un Apps Script con una función `doPost(e)` que guarde `e.parameter` en la hoja, implementarlo como *Aplicación web* con acceso «Cualquiera» y pegar la URL `/exec` en `SCRIPT_URL`.

---

## Despliegue

- **GitHub Pages**: *Settings → Pages → Deploy from a branch → `main` / root*. Cada push a `main` publica en uno o dos minutos.
- **Dominio propio**: el archivo `CNAME` contiene el dominio; los registros DNS del dominio apuntan a GitHub Pages. Si se borra `CNAME`, la web vuelve a la URL `*.github.io` (ya pasó en julio 2026).
- Activar «Enforce HTTPS» en la configuración de Pages.
- Servicios externos: Google Fonts y WhatsApp (enlace `wa.me`).

---

## Pendientes conocidos

- Configurar `SCRIPT_URL` para que los leads se guarden automáticamente, sin depender de que el usuario pulse «enviar» en WhatsApp.
- Enviar los leads al sistema de gestión de DELAGALA en lugar de a una hoja de cálculo.
- Dar de alta el dominio en Google Search Console y revisar SEO básico (meta descripción, Open Graph).
- Mantener sincronizada esta página con su copia en el material de campañas para no publicar versiones distintas.
