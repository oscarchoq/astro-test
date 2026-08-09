# SPEC 01 — Landing page de prueba

> **Estado:** Aprobado
> **Depende de:** —
> **Fecha:** 2026-08-09
> **Objetivo:** Construir una landing page estática, responsive y presentable con contenido genérico de una marca ficticia sobre este proyecto Astro.

## Alcance

**Dentro:**

- Una única página en `src/pages/index.astro` que reemplaza la home por defecto.
- Layout base `src/layouts/Landing.astro` con `<head>`, metadatos e importación de estilos globales.
- Hoja de estilos global única en `src/styles/global.css` con tokens de diseño (colores, tipografía, espaciados), reset básico y `scroll-behavior: smooth`.
- Cinco componentes de sección en `src/components/`: `Navbar.astro`, `Hero.astro`, `Features.astro`, `CTA.astro`, `Footer.astro`.
- Barra de navegación superior fija con nombre de marca y enlaces ancla con scroll suave (solo CSS).
- Contenido en español de una marca ficticia inventada ("Nimbus").
- Imágenes remotas (Unsplash) en el Hero y en las tarjetas de características.
- Diseño responsive (móvil, tablet, escritorio) con un único tema claro.

**Fuera de alcance (para futuros specs):**

- Internacionalización / versión bilingüe.
- Modo oscuro o conmutador de tema.
- Islas interactivas con React/Vue/Svelte u otro framework.
- Formularios funcionales (envío real, backend, validación).
- Páginas o rutas adicionales (blog, precios detallados, etc.).
- Optimización de imágenes con `astro:assets` (`<Image>`) y configuración de `remotePatterns`.
- SEO avanzado (sitemap, Open Graph completo, structured data).

## Modelo de datos

No hay persistencia ni estado en tiempo de ejecución. El contenido es estático, definido como constantes dentro de cada componente `.astro`. Estructuras de referencia:

```js
// Marca (Navbar y Footer)
const brand = { name: "Nimbus", tagline: "..." };

// Enlaces de navegación (Navbar)
const navLinks = [{ label: "Características", href: "#caracteristicas" } /* ... */];

// Tarjeta de característica (Features)
const features = [
  { title: "...", description: "...", image: "https://images.unsplash.com/..." },
];
```

Convenciones:

- IDs de sección en kebab-case y en español (`#caracteristicas`, `#contacto`) para los anclas.
- Imágenes remotas vía `<img>` con `loading="lazy"`, `width` y `height` explícitos para evitar saltos de layout.

## Plan de implementación

1. Crear `src/styles/global.css` con reset básico, variables CSS de diseño (colores, tipografía, radios, sombras), estilos de `body`/tipografía y `scroll-behavior: smooth`. Prueba manual: importarlo temporalmente y ver que la tipografía base cambia.
2. Crear `src/layouts/Landing.astro`: estructura HTML con `<head>` (charset, viewport, `<title>`, meta description), importa `global.css` y expone un `<slot />`. Prueba manual: usarlo desde `index.astro` con texto suelto y ver la página con estilos base.
3. Crear `src/components/Navbar.astro`: barra fija con nombre de marca y `navLinks` como anclas. Prueba manual: los enlaces hacen scroll suave a secciones vacías.
4. Crear `src/components/Hero.astro`: titular, subtítulo, botones CTA e imagen remota de Unsplash. Prueba manual: se ve el hero a pantalla completa con imagen.
5. Crear `src/components/Features.astro` con `id="caracteristicas"`: rejilla responsive de 3–4 tarjetas (título, descripción, imagen). Prueba manual: la rejilla se reordena a una columna en móvil.
6. Crear `src/components/CTA.astro` con `id="contacto"`: bloque final de llamada a la acción con botón. Prueba manual: se ve la sección destacada.
7. Crear `src/components/Footer.astro`: marca, enlaces secundarios y aviso de copyright. Prueba manual: aparece al final.
8. Reescribir `src/pages/index.astro` para envolver todas las secciones dentro de `Landing.astro` en orden: Navbar, Hero, Features, CTA, Footer. Prueba manual: `astro dev --background` y revisar la página completa.

## Criterios de aceptación

- [ ] `astro dev --background` levanta el servidor sin errores en consola.
- [ ] `astro build` termina sin errores ni advertencias de rutas rotas.
- [ ] La home (`/`) muestra, en orden: navbar, hero, características, CTA y footer.
- [ ] La barra de navegación permanece fija al hacer scroll.
- [ ] Al hacer clic en un enlace del navbar la página se desplaza suavemente a la sección correspondiente.
- [ ] Todo el texto visible está en español.
- [ ] A 375 px de ancho la rejilla de características se muestra en una sola columna y no hay scroll horizontal.
- [ ] Las imágenes del hero y de las características cargan desde Unsplash y tienen `width`/`height` definidos.
- [ ] No se importan integraciones de framework (React/Vue/Svelte) en la página.

## Decisiones

- **Sí:** CSS global único en `src/styles/global.css`. Simple, sin dependencias nuevas; suficiente para una landing estática.
- **No:** Tailwind. Añadir la integración es sobreingeniería para una prueba de una sola página.
- **Sí:** Reemplazar `src/pages/index.astro` (la home por defecto de Astro) como página de la landing. Es una prueba y no hay contenido previo que preservar.
- **Sí:** Marca ficticia "Nimbus". Da un contenido neutro y presentable sin atarse a un producto real.
- **Sí:** Imágenes remotas de Unsplash vía `<img>` estándar. Rápido de montar para una prueba.
- **No:** `astro:assets` con `<Image>` y `remotePatterns`. Queda fuera de alcance; se añadiría en un spec de optimización.
- **Sí:** Un componente `.astro` por sección. Facilita leer y reordenar.
- **No:** Modo oscuro. Un único tema claro para mantener el alcance acotado.
- **No:** i18n / versión bilingüe. Abre demasiado alcance; iría en su propio spec.

## Riesgos

| Riesgo                                              | Mitigación                                                                                     |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Unsplash caído o lento deja huecos en las imágenes  | `width`/`height` fijos reservan el espacio; el texto sigue legible aunque la imagen no cargue. |
| El navbar fijo tapa el inicio de las secciones ancla | Añadir `scroll-margin-top` a las secciones con `id` para compensar la altura del navbar.       |

## Lo que **no** entra en este spec

- Internacionalización / versión bilingüe.
- Modo oscuro o conmutador de tema.
- Islas interactivas con React/Vue/Svelte u otro framework.
- Formularios funcionales con backend.
- Páginas o rutas adicionales.
- Optimización de imágenes con `astro:assets`.
- SEO avanzado (sitemap, Open Graph completo, structured data).

Cada uno de esos, si llega, va en su propio spec.

