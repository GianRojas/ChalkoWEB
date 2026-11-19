# Especificación: Página de Productos ChalkoTEX

## 1. Objetivo
Crear una nueva sección **`/productos/`** que muestre los 7 productos de ChalkoTEX en un **grid de 3 columnas estilo ecommerce**, accesible desde la navegación principal. Cada producto tendrá su propia carpeta con `index.html` para detalle futuro.

---

## 2. Estructura de Carpetas

```
ChalkoWEB/
├── productos/
│   ├── index.html                    ← Página principal (grid)
│   ├── kala-lamina-natural/
│   │   └── index.html                ← Detalle producto (vacío por ahora)
│   ├── kala-lamina-teñida/
│   │   └── index.html
│   ├── kala-lamina-texturada/
│   │   └── index.html
│   ├── kala-cartera/
│   │   └── index.html
│   ├── kala-cartuchera/
│   │   └── index.html
│   ├── kala-muestra-pack/
│   │   └── index.html
│   └── kala-lamina-grande/
│       └── index.html
```

> **Nota**: Las subcarpetas de producto usan **kebab-case** coincidiendo con el `id` del producto. Cada `index.html` inicial estará vacío (placeholder) para ser completado en fase posterior.

---

## 3. Estructura de Navegación
- Agregar enlace **"Productos"** en el **navbar** (entre "Proyectos" e "Impacto")
- URL: `/productos/`
- Breadcrumbs en `productos/index.html`: `Inicio > Productos`
- Breadcrumbs en `productos/<id>/index.html`: `Inicio > Productos > Nombre Producto`

---

## 4. Modelo de Datos del Producto

Cada producto tiene:
| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| `id` | string | Sí | Identificador único (ej: `kala-lamina`) |
| `nombre` | string | Sí | Nombre comercial |
| `descripcion_corta` | string | Sí | 1-2 líneas para la card |
| `descripcion_larga` | string | No | Para modal/detalle futuro |
| `precio` | number/null | No | Precio referencial (CLP) |
| `unidad` | string | No | "m²", "unidad", "kg" |
| `imagen_principal` | string | Sí | Ruta relativa en `img/chalko/` |
| `imagenes_extra` | string[] | No | Gallery para lightbox |
| `categoria` | string | Sí | "Láminas", "Accesorios", "Muestras" |
| `tags` | string[] | No | ["compostable", "personalizable", "vegano"] |
| `stock` | string | No | "Disponible", "Bajo stock", "Pre-orden" |
| `destacado` | string/null | No | "novedad", "mas-vendido", o null |

---

## 5. Los 7 Productos Iniciales

| # | ID (carpeta) | Nombre | Categoría | Imagen sugerida | Destacado |
|---|--------------|--------|-----------|-----------------|-----------|
| 1 | `kala-lamina-natural` | Lámina KALA Natural | Láminas | `foto-lamina-1.jpg` | "novedad" |
| 2 | `kala-lamina-teñida` | Lámina KALA Teñida | Láminas | `foto-lamina-2.jpg` | null |
| 3 | `kala-lamina-texturada` | Lámina KALA Texturada | Láminas | `foto-lamina-3.jpg` | null |
| 4 | `kala-cartera` | Cartera KALA | Accesorios | `cartera-1.jpg` | "mas-vendido" |
| 5 | `kala-cartuchera` | Cartuchera KALA | Accesorios | `cartuchera-y-tarjetero.jpg` | null |
| 6 | `kala-muestra-pack` | Pack Muestras KALA | Muestras | `foto-lamina-1.jpg` (placeholder) | null |
| 7 | `kala-lamina-grande` | Lámina KALA Formato Grande | Láminas | `foto-lamina-2.jpg` (placeholder) | null |

> **Nota**: Las imágenes 6 y 7 usan placeholders; se reemplazarán cuando haya assets reales.

---

## 6. Layout del Grid

### Desktop (≥992px)
- **3 columnas** iguales (`col-lg-4`)
- Gap: 30px (`g-4` de Bootstrap)
- Max-width container: 1200px

### Tablet (768px–991px)
- **2 columnas** (`col-md-6`)

### Mobile (<768px)
- **1 columna** (`col-12`)

---

## 7. Diseño de la Product Card

```
┌─────────────────────────────────────┐
│  [BADGE: "Novedad" / "Más vendido"] │  ← opcional, esquina sup-izq
│                                     │
│     ┌─────────────────────┐         │
│     │                     │         │  ← Imagen (aspect-ratio 4:5)
│     │    IMAGEN PRODUCTO  │         │     object-fit: cover
│     │                     │         │
│     └─────────────────────┘         │
│                                     │
│  Nombre del Producto                │  ← h5, font-weight: 600
│  Categoría • Desde $XX.XXX          │  ← small, text-muted
│                                     │
│  [Ver detalle]  [Agregar consulta]  │  ← 2 botones: outline-primary / primary
└─────────────────────────────────────┘
```

### Botón "Ver detalle"
- **Href**: `/productos/<id>/` (ej: `/productos/kala-lamina-natural/`)
- Estilo: `btn btn-outline-primary rounded-pill px-4`
- Texto: "Ver detalle" + ícono `fa fa-arrow-right ms-1`

### Botón "Agregar consulta"
- **Href**: `#contacto` (scroll a formulario de contacto en footer) o abre modal
- Estilo: `btn btn-primary rounded-pill px-4`
- Texto: "Consultar" + ícono `fa fa-envelope ms-1`

### Estilos clave
- **Card**: `border: 1px solid rgba(0,0,0,.07)`, `border-radius: 16px`, `overflow: hidden`, `transition: transform .3s, box-shadow .3s`
- **Hover**: `transform: translateY(-4px)`, `box-shadow: 0 20px 40px rgba(0,0,0,.08)`
- **Imagen**: `aspect-ratio: 4/5`, `object-fit: cover`, `transition: transform .5s`
- **Hover imagen**: `transform: scale(1.05)`
- **Badge**: `position: absolute`, `top: 12px`, `left: 12px`, `z-index: 2`
  - `.badge-novedad`: `background: var(--primary)`, `color: white`
  - `.badge-mas-vendido`: `background: #e8a838`, `color: white`

---

## 8. Header de la Página (`productos/index.html`)

```html
<section class="page-header-products">
  <div class="container">
    <div class="row justify-content-center text-center">
      <div class="col-lg-8">
        <nav aria-label="breadcrumb" class="mb-4">
          <ol class="breadcrumb justify-content-center">
            <li class="breadcrumb-item"><a href="/">Inicio</a></li>
            <li class="breadcrumb-item active" aria-current="page">Productos</li>
          </ol>
        </nav>
        <h1 class="display-4 fw-bold mb-3">Nuestros Productos</h1>
        <p class="lead text-secondary">
          Biotextiles KALA: materiales vegetales, compostables y trazables
          para una industria textil regenerativa.
        </p>
      </div>
    </div>
  </div>
</section>
```

- Fondo: `var(--light)` (#F5F5F5) con patrón sutil o gradiente
- Padding: `py-5 py-lg-6`

---

## 9. Filtros por Categoría (Opcional v1.1)

Sobre el grid, barra de filtros:
```
[Todos]  [Láminas]  [Accesorios]  [Muestras]
```
- Implementación: Isotope.js (ya incluido en `lib/isotope/`)
- Clase en card: `portfolio-item categoria-laminas`

---

## 10. Integración con Navbar

En `index.html` (línea ~102), agregar:
```html
<a href="/productos/" class="nav-item nav-link">Productos</a>
```

Y en `productos/index.html`, marcar como `active`.

---

## 11. Archivos a Crear/Modificar

### Nuevos (estructura completa)
| Archivo | Descripción |
|---------|-------------|
| `productos/index.html` | Página principal con grid |
| `productos/kala-lamina-natural/index.html` | Placeholder detalle |
| `productos/kala-lamina-teñida/index.html` | Placeholder detalle |
| `productos/kala-lamina-texturada/index.html` | Placeholder detalle |
| `productos/kala-cartera/index.html` | Placeholder detalle |
| `productos/kala-cartuchera/index.html` | Placeholder detalle |
| `productos/kala-muestra-pack/index.html` | Placeholder detalle |
| `productos/kala-lamina-grande/index.html` | Placeholder detalle |
| `css/products.css` | Estilos específicos del grid y cards |
| `js/products.js` | Lógica de filtros, lazy-load, animaciones |

### Modificar
| Archivo | Cambio |
|---------|--------|
| `index.html` (raíz) | Agregar link "Productos" en navbar con `href="/productos/"` |
| `css/custom.css` | Importar `@import "products.css";` (si archivo separado) |

---

## 12. Stack Técnico
- **HTML5** semántico
- **Bootstrap 5** (ya en proyecto) para grid y utilidades
- **CSS Custom Properties** (variables existentes: `--primary`, `--secondary`, `--light`, `--dark`)
- **Isotope.js** (ya incluido) para filtros/masonry
- **WOW.js** (ya incluido) para animaciones on-scroll
- **Lightbox2** (ya incluido) para gallery de imágenes
- **Vanilla JS** (ES6+) para interactividad ligera

---

## 13. Accesibilidad
- `alt` descriptivo en imágenes
- Contraste AA mínimo
- Focus visible en botones/links
- Navegación por teclado funcional
- ARIA labels en botones de acción

---

## 14. SEO
- `<title>Productos KALA - ChalkoTEX</title>` en `productos/index.html`
- Meta description única por página
- Open Graph tags
- JSON-LD `Product` schema para cada card
- Sitemap.xml actualizado con todas las URLs `/productos/<id>/`

---

## 15. Performance
- Lazy loading nativo (`loading="lazy"`) en imágenes del grid
- Imágenes optimizadas (WebP + fallback)
- CSS/JS minificados en build
- Preconnect a fonts.googleapis.com (ya existe)

---

## 16. Próximos Pasos (Fuera de alcance v1.0)
- Página de detalle individual (`producto.html?id=...`)
- Carrito / cotización integrada
- Filtros por tags, precio, disponibilidad
- Paginación / infinite scroll (>12 productos)
- Comparador de productos
- Wishlist / favoritos

---

## 17. Criterios de Aceptación
1. ✅ Estructura de carpetas `productos/` creada con 8 `index.html` (1 principal + 7 producto)
2. ✅ Página principal carga en `/productos/`
3. ✅ 7 productos visibles en grid 3/2/1 columnas
4. ✅ Cards tienen hover effect y badge según campo `destacado`
5. ✅ Botón "Ver detalle" navega a `/productos/<id>/`
6. ✅ Navbar destaca "Productos" como activo en `/productos/`
7. ✅ Breadcrumbs funcionan en ambas páginas (usando `/` y `/productos/`)
8. ✅ Responsive correcto en breakpoints Bootstrap
9. ✅ Lightbox funciona al click en imagen (prepara para galería futura)
10. ✅ Filtros Isotope funcionan (si se implementan v1.0)
11. ✅ Sin errores de consola
12. ✅ Lighthouse Performance ≥ 90
13. ✅ Validación HTML (W3C) sin errores