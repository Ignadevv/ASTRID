# Auditoría y plan de mejoras — ASTRID / FromTo Delivery Service

> Documento de trabajo. Generado tras revisar el estado del repositorio `Ignadevv/ASTRID`.
> Los puntos marcados con **[PENDIENTE]** requieren el código fuente para cerrarse.

---

## 0. Punto de partida real

| Elemento | Estado encontrado |
|---|---|
| Archivos en el repo | `README.md` de **8 bytes** (`# ASTRID`) |
| Commits | 1 (`Initial commit`) |
| Ramas | `main` + rama de trabajo |
| Issues / PRs | 0 / 0 |
| Tags / Releases | Ninguno |
| LICENSE | **No existe** |
| .gitignore | **No existe** |
| CI/CD | **No existe** |
| Descripción / topics / homepage en GitHub | Vacíos |
| Tamaño | 0 KB — sin código |

**Conclusión:** el repositorio es un cascarón. La web de FromTo Delivery Service nunca se subió.

---

## 1. Lo que se ha hecho ya en esta rama

- [x] `LICENSE` — MIT, alineado con la convención del resto de repos (`AURAMOVIES`, `Gran-Zona-Marciana`).
- [x] `.gitignore` — cubre SO, editores, Node, builds, `.env`, Python y backups tipo `*.old.html`.

---

## 2. Bloqueadores / alto impacto

### 2.1 Subir el código fuente
Es el bloqueador raíz: sin `index.html`, `style.css`, `app.js`, `translations.js`, `services.html`,
`propuesta_presupuesto.html` y los assets, no se puede auditar nada más.

### 2.2 Peso de assets
El sitio usa muchas imágenes de alta resolución (PNG/JPG con nombres tipo
`about_app.png`, `services_slide1.png`, `slide2.jpg`, `logo-removebg-preview.png`).
Las imágenes tipo hero de una sola sección ya pesan cientos de KB cada una.

**Acciones concretas:**
- Convertir a **WebP/AVIF** (ahorro típico del 60–80 %).
- Servir con `srcset`/`sizes` responsive en lugar de una única resolución.
- `loading="lazy"` + `decoding="async"` en todo lo que no sea above-the-fold.
- `width`/`height` explícitos para eliminar CLS.
- Comprimir el logo (un PNG de logo no debería pasar de ~20 KB; mejor SVG).

### 2.3 Naming de assets
`logo-removebg-preview.png`, `slide_new_1.png`, `slide3.jpg`, `slide3.png` mezclados…
Hay duplicados y nombres de herramienta. Sin un esquema claro (`logo.svg`,
`hero-01.webp`) el mantenimiento se vuelve un infierno.

### 2.4 Legales y RGPD (estás en España — esto no es opcional)
- **Aviso legal**, **Política de privacidad** y **Política de cookies** son obligatorias
  para un sitio comercial que capta leads (LSSI-CE + RGPD).
- Si hay formulario de contacto o `propuesta_presupuesto.html` recoge datos:
  consentimiento explícito, finalidad y base legal.
- Banner de cookies real (con rechazo tan fácil como aceptación) **solo si** usas
  cookies no esenciales/analítica.

### 2.5 Formularios sin backend
**[PENDIENTE]** Si el formulario de contacto o el generador de presupuestos solo hace
`console.log`, el lead se pierde. Opciones: Formspree, Resend, o función serverless.

---

## 3. Calidad de código y estructura

### 3.1 Sin build step ni servidor local
Abrir `index.html` con `file://` rompe rutas relativas y `fetch()`.
**Acción:** añadir `package.json` con un script de desarrollo (p. ej. `npx serve`,
Vite o Parcel) y scripts de validación.

### 3.2 Estructura de carpetas propuesta
```
/
├── index.html
├── services.html
├── propuesta_presupuesto.html
├── assets/
│   ├── img/        (optimizadas, organizadas por sección)
│   ├── icons/      (SVG)
│   └── fonts/      (self-hosted, woff2)
├── css/
├── js/
└── docs/
```

### 3.3 i18n (`translations.js`)
**[PENDIENTE]** Revisar cómo se aplica el idioma. Buenas prácticas:
- Persistir el idioma en `localStorage` **y** reflejarlo en la URL (`/es/`, `/en/`)
  para que sea indexable y compartible.
- `<html lang="…">` dinámico y etiquetas `hreflang` alternas.
- No dejar textos hardcodeados fuera del diccionario.

### 3.4 Accesibilidad (WCAG 2.2 AA)
- `alt` descriptivo en todas las imágenes (no `alt=""` decorativo mal usado).
- Foco visible en navegación por teclado; el carrusel debe ser operable con teclado.
- **Contraste**: hay texto blanco sobre fotos claras en varios slides — revisar
  ratio ≥ 4.5:1 y añadir overlay si hace falta.
- `aria-label` en los botones de navegación del carrusel (los dots).
- Respetar `prefers-reduced-motion`.

### 3.5 SEO
- `title` y `meta description` únicos por página.
- Open Graph + Twitter Card (clave si compartes por WhatsApp/LinkedIn).
- `sitemap.xml` y `robots.txt`.
- **JSON-LD `LocalBusiness`/`Organization`** con NIF, dirección y teléfono — ayuda
  muchísimo a aparecer en búsquedas locales en Valencia.
- URLs canónicas.

---

## 4. Infraestructura y automatización

### 4.1 CI con GitHub Actions
Workflow en cada push/PR: validación de HTML (html-validate), lint de CSS/JS
(stylelint + ESLint), y comprobación de enlaces rotos (lychee).

### 4.2 Deploy
Elegir y documentar: **GitHub Pages** (gratis, simple), **Netlify** o **Vercel**.
Añadir deploy automático desde `main` con previews por PR.

### 4.3 Calidad de repositorio
- Plantillas de issue y PR (`.github/`).
- `CONTRIBUTING.md` y Conventional Commits.
- `CHANGELOG.md`.
- Branch protection en `main` + Dependabot.

### 4.4 Metadatos de GitHub
Rellenar descripción, homepage, y topics (`delivery`, `logistics`, `web`, `valencia`…).

---

## 5. Roadmap sugerido

| Fase | Entregable |
|---|---|
| **1** | Subir el código fuente + `.gitignore` + LICENSE *(en curso)* |
| **2** | Estructura de carpetas + `package.json` con dev server |
| **3** | Optimización de imágenes y fuentes |
| **4** | Accesibilidad + SEO técnico |
| **5** | Páginas legales (RGPD/LSSI) |
| **6** | CI + deploy automático |
| **7** | Formularios con backend real + analítica con consentimiento |
