# Casamigos Villas

Landing page de una sola página para **Casamigos Villas**, una villa de lujo eco-friendly ubicada en Providenciales, Turks and Caicos. Construida con [Astro](https://astro.build) 6, Tailwind CSS v4 y animaciones GSAP (ScrollTrigger + ScrollToPlugin).

Single-page landing site for **Casamigos Villas**, an eco-friendly luxury villa located in Providenciales, Turks and Caicos. Built with [Astro](https://astro.build) 6, Tailwind CSS v4, and GSAP animations (ScrollTrigger + ScrollToPlugin).

---

## 🇪🇸 Español

### Stack técnico

- **[Astro 6](https://astro.build)** — framework de renderizado (componentes `.astro`, sin cliente JS por defecto)
- **Tailwind CSS v4** vía `@tailwindcss/vite`
- **GSAP** — `ScrollTrigger` para el efecto de máscara/reveal del Hero y `ScrollToPlugin` para el scroll suave del menú
- **react-icons** — iconografía
- **TypeScript** (`tsconfig.json` base de Astro)
- Node.js **>= 22.12.0**

No usa framework de UI reactivo (React/Vue/Svelte); todo el sitio está construido con componentes `.astro` + `<script>` nativo para las animaciones.

### Estructura del proyecto

```text
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── Hero.astro           # Sección principal con efecto de máscara (logo → reveal)
│   │   ├── NavBar.astro         # Navegación con menú desplegable animado
│   │   ├── AboutUs.astro        # Descripción de la villa
│   │   ├── TypesProperty.astro  # Detalles de habitaciones/baños
│   │   ├── FirstPhotos.astro    # Galería de fotos (bloque 1)
│   │   ├── SecondPhotos.astro   # Galería de fotos (bloque 2)
│   │   ├── Photos.astro         # Galería general
│   │   ├── Localization.astro   # Mapa / ubicación en Providenciales
│   │   ├── ContactButton.astro  # CTA de contacto
│   │   └── Footer.astro         # Pie de página con redes sociales
│   ├── images/                  # Fotografías de la propiedad (.webp) e iconografía (.svg)
│   ├── layout/
│   │   └── layout.astro         # Layout base (head, NavBar, Footer, scripts globales de GSAP)
│   ├── pages/
│   │   └── index.astro          # Única página del sitio, compone todas las secciones
│   └── styles/
│       └── global.css           # Estilos globales + tokens de Tailwind
├── astro.config.mjs             # Config de Astro + plugin de Tailwind
├── tsconfig.json
└── package.json
```

### Requisitos previos

- Node.js `>= 22.12.0`
- npm (el repo trae `package-lock.json`)

### Instalación y desarrollo local

```bash
git clone https://github.com/Raw-Form/Casamigos.git
cd Casamigos
npm install
npm run dev
```

El sitio queda disponible en `http://localhost:4321`.

### Scripts disponibles

| Comando           | Acción                                              |
| :----------------- | :--------------------------------------------------- |
| `npm run dev`       | Levanta el servidor de desarrollo en `localhost:4321` |
| `npm run build`     | Genera el build de producción en `./dist/`            |
| `npm run preview`   | Sirve el build de producción localmente para revisarlo |
| `npm run astro ...` | Ejecuta comandos del CLI de Astro (`astro check`, `astro add`, etc.) |

### Notas de desarrollo

- El efecto de reveal del **Hero** (`src/pages/index.astro`) usa `mask-image` con el logo SVG animado vía GSAP `ScrollTrigger` (con `pin: true`), incluyendo los prefijos `-webkit-mask-*` necesarios para compatibilidad con Safari/iOS.
- El menú y el scroll a anclas (`href="#..."`) se manejan globalmente en `src/layout/layout.astro` con `ScrollToPlugin`.
- No hay variables de entorno requeridas para desarrollo local; el `.gitignore` ya contempla `.env` y `.env.production` por si se agregan más adelante (por ejemplo, para un formulario de contacto o analítica).

### Despliegue

El proyecto es un sitio estático (`astro build` genera HTML/CSS/JS en `./dist/`), por lo que puede desplegarse en cualquier hosting estático (Vercel, Netlify, Cloudflare Pages) o servirse detrás de Nginx/Apache. Ejemplo recomendado con **Coolify + Docker**, consistente con el resto de los proyectos:

**Dockerfile sugerido (multi-stage):**

```dockerfile
# --- Build stage ---
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# --- Serve stage ---
FROM nginx:alpine AS runner
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Pasos en Coolify:

1. Crear una nueva aplicación apuntando a este repositorio (rama `main` o la que corresponda).
2. Build pack: **Dockerfile** (usando el Dockerfile de arriba) o **Nixpacks** si prefieres que Coolify detecte Astro automáticamente.
3. Puerto expuesto: `80` (si usas el Dockerfile con Nginx) o el puerto de `astro preview` si sirves con Node.
4. Si en el futuro se agregan variables `PUBLIC_*` (Astro) o `NEXT_PUBLIC_*`-style, declararlas tanto como `ARG` como `ENV` en el stage de build y marcarlas como *Build Variable* en Coolify — igual que en los demás proyectos del equipo.
5. Configurar el dominio y activar SSL automático desde Coolify.

> Como es un sitio 100% estático, no requiere proceso de Node corriendo en producción; Nginx sirviendo `./dist/` es suficiente y más liviano.

### Créditos

Desarrollado por **Raw-Form**.

---

## 🇬🇧 English

### Tech stack

- **[Astro 6](https://astro.build)** — rendering framework (`.astro` components, zero client JS by default)
- **Tailwind CSS v4** via `@tailwindcss/vite`
- **GSAP** — `ScrollTrigger` for the Hero mask/reveal effect and `ScrollToPlugin` for smooth anchor scrolling
- **react-icons** — iconography
- **TypeScript** (Astro's base `tsconfig.json`)
- Node.js **>= 22.12.0**

No reactive UI framework (React/Vue/Svelte) is used — the whole site is built with `.astro` components plus native `<script>` blocks for animations.

### Project structure

```text
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── Hero.astro           # Main hero with mask reveal effect (logo → full view)
│   │   ├── NavBar.astro         # Navigation with animated dropdown menu
│   │   ├── AboutUs.astro        # Villa description
│   │   ├── TypesProperty.astro  # Bedroom/bathroom details
│   │   ├── FirstPhotos.astro    # Photo gallery (block 1)
│   │   ├── SecondPhotos.astro   # Photo gallery (block 2)
│   │   ├── Photos.astro         # General gallery
│   │   ├── Localization.astro   # Map / location in Providenciales
│   │   ├── ContactButton.astro  # Contact CTA
│   │   └── Footer.astro         # Footer with social links
│   ├── images/                  # Property photos (.webp) and icons (.svg)
│   ├── layout/
│   │   └── layout.astro         # Base layout (head, NavBar, Footer, global GSAP scripts)
│   ├── pages/
│   │   └── index.astro          # Single page of the site, composes all sections
│   └── styles/
│       └── global.css           # Global styles + Tailwind tokens
├── astro.config.mjs             # Astro config + Tailwind plugin
├── tsconfig.json
└── package.json
```

### Prerequisites

- Node.js `>= 22.12.0`
- npm (the repo ships a `package-lock.json`)

### Local development

```bash
git clone https://github.com/Raw-Form/Casamigos.git
cd Casamigos
npm install
npm run dev
```

The site will be available at `http://localhost:4321`.

### Available scripts

| Command             | Action                                              |
| :------------------- | :---------------------------------------------------- |
| `npm run dev`         | Starts the dev server at `localhost:4321`             |
| `npm run build`       | Builds the production site to `./dist/`               |
| `npm run preview`     | Serves the production build locally for review        |
| `npm run astro ...`   | Runs Astro CLI commands (`astro check`, `astro add`, etc.) |

### Development notes

- The **Hero** reveal effect (`src/pages/index.astro`) uses `mask-image` on the SVG logo, animated via GSAP `ScrollTrigger` (`pin: true`), including the `-webkit-mask-*` prefixes needed for Safari/iOS compatibility.
- The menu and anchor scrolling (`href="#..."`) are handled globally in `src/layout/layout.astro` via `ScrollToPlugin`.
- No environment variables are required for local development; `.gitignore` already covers `.env` and `.env.production` in case any are added later (e.g. a contact form or analytics).

### Deployment

This is a static site (`astro build` outputs HTML/CSS/JS to `./dist/`), so it can be deployed to any static host (Vercel, Netlify, Cloudflare Pages) or served behind Nginx/Apache. Recommended setup with **Coolify + Docker**, consistent with the rest of the team's projects:

**Suggested multi-stage Dockerfile:**

```dockerfile
# --- Build stage ---
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# --- Serve stage ---
FROM nginx:alpine AS runner
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Coolify steps:

1. Create a new application pointing to this repo (`main` branch, or whichever applies).
2. Build pack: **Dockerfile** (using the one above) or **Nixpacks** if you'd rather let Coolify auto-detect Astro.
3. Exposed port: `80` (with the Nginx Dockerfile above) or the `astro preview` port if serving via Node.
4. If `PUBLIC_*` (Astro) or similar build-time env vars are added later, declare them as both `ARG` and `ENV` in the build stage and mark them as a *Build Variable* in Coolify — same convention used across the other projects.
5. Set up the domain and enable automatic SSL from Coolify.

> Since this is a fully static site, no Node process needs to run in production — serving `./dist/` with Nginx is enough and much lighter.

### Credits

Built by **Raw-Form**.