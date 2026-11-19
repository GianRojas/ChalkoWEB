# Especificación: Página de Productos ChalkoTEX

## 1. Objetivo
Crear una nueva página `productos/index.html` que muestre los 7 productos de ChalkoTEX en un **grid de 3 columnas estilo ecommerce**, accesible desde la navegación principal.

---

## 2. Estructura de Navegación
- Agregar enlace "Productos" en el **navbar** (entre "Proyectos" e "Impacto")
- URL: `/productos/index.html`
- Breadcrumbs: Inicio > Productos

---

## 3. Modelo de Datos del Producto

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
| `destacado` | boolean | No | Para badge "Novedad" / "Más vendido" |

---

## 4. Los 7 Productos Iniciales

| # | ID | Nombre | Categoría | Imagen sugerida |
|---|----|--------|-----------|-----------------|
| 1 | `kala-lamina-natural` | Lámina KALA Natural | Láminas | `foto-lamina-1.jpg` |
| 2 | `kala-lamina-teñida` | Lámina KALA Teñida | Láminas | `foto-lamina-2.jpg` |
| 3 | `kala-lamina-texturada` | Lámina KALA Texturada | Láminas | `foto-lamina-3.jpg` |
| 4 | `kala-cartera` | Cartera KALA | Accesorios | `cartera-1.jpg` |
| 5 | `kala-cartuchera` | Cartuchera KALA | Accesorios | `cartuchera-y-tarjetero.jpg` |
| 6 | `kala-muestra-pack` | Pack Muestras KALA | Muestras | `foto-lamina-1.jpg` (placeholder) |
| 7 | `kala-lamina-grande` | Lámina KALA Formato Grande | Láminas | `foto-lamina-2.jpg` (placeholder) |

> **Nota**: Las imágenes 6 y 7 usan placeholders; se reemplazarán cuando haya assets reales.

---

## 5. Layout del Grid

### Desktop (≥992px)
- **3 columnas** iguales (`col-lg-4`)
- Gap: 30px (`g-4` de Bootstrap)
- Max-width container: 1200px

### Tablet (768px–991px)
- **2 columnas** (`col-md-6`)

### Mobile (<768px)
- **1 columna** (`col-12`)

---

## 6. Diseño de la Product Card

```
┌─────────────────────────────────────┐
│  [BADGE: "Novedad" / "Más vendido"] │  ← opcional, esquina sup-izq
│                                     │
│     ┌─────────────────────┐         │
│     │                     │         │  ← Imagen (aspect-ratio 1:1 o 4:5)
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

### Estilos clave
- **Card**: `border: 1px solid rgba(0,0,0,.07)`, `border-radius: 16px`, `overflow: hidden`, `transition: transform .3s, box-shadow .3s`
- **Hover**: `transform: translateY(-4px)`, `box-shadow: 0 20px 40px rgba(0,0,0,.08)`
- **Imagen**: `aspect-ratio: 4/5`, `object-fit: cover`, `transition: transform .5s`
- **Hover imagen**: `transform: scale(1.05)`
- **Badge**: `position: absolute`, `top: 12px`, `left: 12px`, `z-index: 2`

---

## 7. Header de la Página

```html
<section class="page-header-products">
  <div class="container">
    <div class="row justify-content-center text-center">
      <div class="col-lg-8">
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

## 8. Filtros por Categoría (Opcional v1.1)

Sobre el grid, barra de filtros:
```
[Todos]  [Láminas]  [Accesorios]  [Muestras]
```
- Implementación: Isotope.js (ya incluido en `lib/isotope/`)
- Clase en card: `portfolio-item categoria-laminas`

---

## 9. Integración con Navbar

En `index.html` (línea ~102), agregar:
```html
<a href="productos/index.html" class="nav-item nav-link">Productos</a>
```

Y en `productos/index.html`, marcar como `active`.

---

## 10. Archivos a Crear/Modificar

### Nuevos
| Archivo | Descripción |
|---------|-------------|
| `productos/index.html` | Página principal |
| `css/products.css` | Estilos específicos (o extender `custom.css`) |
| `js/products.js` | Lógica de filtros, lazy-load, animaciones |

### Modificar
| Archivo | Cambio |
|---------|--------|
| `index.html` | Agregar link "Productos" en navbar |
| `css/style.css` / `custom.css` | Importar `products.css` si es archivo separado |

---

## 11. Stack Técnico
- **HTML5** semántico
- **Bootstrap 5** (ya en proyecto) para grid y utilidades
- **CSS Custom Properties** (variables existentes: `--primary`, `--secondary`, `--light`, `--dark`)
- **Isotope.js** (ya incluido) para filtros/masonry
- **WOW.js** (ya incluido) para animaciones on-scroll
- **Lightbox2** (ya incluido) para gallery de imágenes
- **Vanilla JS** (ES6+) para interactividad ligera

---

## 12. Accesibilidad
- `alt` descriptivo en imágenes
- Contraste AA mínimo
- Focus visible en botones/links
- Navegación por teclado funcional
- ARIA labels en botones de acción

---

## 13. SEO
- `<title>Productos KALA - ChalkoTEX</title>`
- Meta description única
- Open Graph tags
- JSON-LD `Product` schema para cada card
- Sitemap.xml actualizado

---

## 14. Performance
- Lazy loading nativo (`loading="lazy"`) en imágenes del grid
- Imágenes optimizadas (WebP + fallback)
- CSS/JS minificados en build
- Preconnect a fonts.googleapis.com (ya existe)

---

## 15. Próximos Pasos (Fuera de alcance v1.0)
- Página de detalle individual (`producto.html?id=...`)
- Carrito / cotización integrada
- Filtros por tags, precio, disponibilidad
- Paginación / infinite scroll (>12 productos)
- Comparador de productos
- Wishlist / favoritos

---

## 16. Criterios de Aceptación
1. ✅ Página carga en `/productos/index.html`
2. ✅ 7 productos visibles en grid 3/2/1 columnas
3. ✅ Cards tienen hover effect y badge si corresponde
4. ✅ Navbar destaca "Productos" como activo
5. ✅ Responsive correcto en breakpoints Bootstrap
6. ✅ Lightbox funciona al click en imagen
7. ✅ Filtros Isotope funcionan (si se implementan v1.0)
8. ✅ Sin errores de consola
9. ✅ Lighthouse Performance ≥ 90
10. ✅ Validación HTML (W3C) sin errores