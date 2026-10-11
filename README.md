# ASTRID — FromTo Delivery Service

Sitio web corporativo de **FromTo Delivery Service**, servicio de mensajería y logística.

> **Estado del proyecto:** en construcción. El código fuente está en proceso de incorporación
> a este repositorio. Consulta [`docs/AUDITORIA-ASTRID.md`](docs/AUDITORIA-ASTRID.md) para
> el estado actual y el plan de mejoras.

---

## Contenido del sitio

| Página | Descripción |
|---|---|
| `index.html` | Portada: propuesta de valor, servicios, app, secciones corporativas |
| `services.html` | Detalle de servicios de mensajería y logística |
| `propuesta_presupuesto.html` | Generador de propuestas / presupuestos para clientes |

## Stack

Sitio estático: **HTML5 + CSS3 + JavaScript vanilla**, con capa de internacionalización
(`translations.js`) para servir el contenido en varios idiomas. Sin dependencias de runtime.

## Estructura

```
/
├── index.html
├── services.html
├── propuesta_presupuesto.html
├── assets/
│   ├── img/
│   └── icons/
├── css/
│   └── style.css
├── js/
│   ├── app.js
│   └── translations.js
└── docs/
    └── AUDITORIA-ASTRID.md
```

## Desarrollo local

Al ser un sitio estático **no abras los archivos con `file://`** — las rutas relativas y
las llamadas a `fetch()` fallarán. Sirve el directorio por HTTP:

```bash
# Opción A — sin instalar nada (Node 18+)
npx serve .

# Opción B — Python
python3 -m http.server 8000
```

Después abre <http://localhost:8000>.

## Despliegue

Pendiente de definir. Opciones evaluadas: GitHub Pages, Netlify o Vercel con deploy
automático desde la rama `main`.

## Roadmap

1. Incorporar el código fuente al repositorio
2. Estructura de carpetas y entorno de desarrollo local
3. Optimización de imágenes y fuentes
4. Accesibilidad (WCAG 2.2 AA) y SEO técnico
5. Páginas legales (RGPD / LSSI-CE)
6. CI con GitHub Actions y despliegue automático
7. Formularios con backend y analítica con consentimiento

## Licencia

[MIT](LICENSE) © 2026 Ignacio (Ignadevv)
