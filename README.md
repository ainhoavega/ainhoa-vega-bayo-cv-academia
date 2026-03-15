# CV Académico — Quarto Website

Página académica multilingüe (Euskera · English · Castellano) construida con [Quarto](https://quarto.org/), lista para publicar en **GitHub Pages**.

---

## Estructura del proyecto

```
cv-academico/
├── cv-academico.Rproj  # Proyecto RStudio (ábrelo con doble clic)
├── _quarto.yml         # Configuración del proyecto Quarto
├── index.qmd           # Página principal (3 idiomas)
├── styles.css          # Estilos personalizados
├── assets/             # Foto, favicon, etc. (crear si es necesario)
│   └── foto.jpg        # Tu foto (130×130 px mínimo, cuadrada)
└── docs/               # Output HTML (generado automáticamente)
```

---

## Instalación y uso local

### 1. Requisitos

- [R](https://cran.r-project.org/) ≥ 4.2
- [RStudio](https://posit.co/download/rstudio-desktop/) ≥ 2023.06
- [Quarto CLI](https://quarto.org/docs/get-started/) ≥ 1.4

```r
# En R, instala quarto si no lo tienes:
install.packages("quarto")
```

### 2. Abrir el proyecto

1. Haz doble clic en **`cv-academico.Rproj`** — RStudio abre el proyecto automáticamente
2. En el panel **Build** (pestaña superior derecha) aparecerá el botón **"Render Website"**

### 3. Previsualizar en local

Desde RStudio: pestaña **Build → Render Website**, o desde el terminal integrado:

```bash
quarto preview
```

Abre automáticamente en `http://localhost:4848`

### 4. Compilar el sitio

```r
# Desde la consola de R (con el proyecto abierto):
quarto::quarto_render()
```

O desde el terminal:

```bash
quarto render
```

Genera la carpeta `docs/` con el HTML listo para publicar.

---

## Publicar en GitHub Pages

1. Crea un repositorio en GitHub (p.ej. `tu-usuario.github.io` o `cv-academico`)
2. Sube todos los archivos:
   ```bash
   git init
   git add .
   git commit -m "Initial CV site"
   git branch -M main
   git remote add origin https://github.com/TU-USUARIO/cv-academico.git
   git push -u origin main
   ```
3. En GitHub → **Settings → Pages**:
   - Source: `Deploy from a branch`
   - Branch: `main` / folder: `/docs`
4. ¡Listo! Tu CV estará en `https://tu-usuario.github.io/cv-academico`

---

## Personalización

### Cambiar datos personales

Edita `index.qmd` y reemplaza:

| Placeholder | Tu dato real |
|---|---|
| `Ainhoa Vega-Bayo` | Tu nombre completo |
| `ainhoa.vega@ehu.eus` | Tu email |
| `0000-0000-0000-0000` | Tu ORCID |
| `ane-martinez` | Tu usuario GitHub |
| Las entradas de hezkuntza/educación | Tu formación real |
| Las publicaciones | Tus artículos reales |

### Añadir foto

1. Pon tu foto en `assets/foto.jpg` (cuadrada, ≥ 400×400 px)
2. En `index.qmd`, busca la línea con `avatar-placeholder` y sustitúyela:
   ```html
   <img src="assets/foto.jpg" alt="Foto" class="avatar">
   ```


---

## Idiomas

El cambio de idioma es instantáneo (JavaScript puro, sin recarga).  
El sitio recuerda el idioma elegido entre visitas (localStorage).  
Por defecto carga en **euskera**.

---

## 📄 Licencia

MIT — libre para usar y adaptar.
