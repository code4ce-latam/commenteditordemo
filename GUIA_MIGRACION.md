# Guía de Migración: Editor de Comentarios Anclados

Esta guía documenta el proceso para migrar las funcionalidades de `review_word_resaltador_with_anclaje` y `review_pdf_resaltador_with_anclaje` a otro proyecto Next.js.

## 📋 Tabla de Contenidos

1. [Requisitos Previos](#requisitos-previos)
2. [Dependencias](#dependencias)
3. [Estructura de Archivos](#estructura-de-archivos)
4. [Configuración del Proyecto](#configuración-del-proyecto)
5. [Pasos de Migración](#pasos-de-migración)
6. [Configuraciones Específicas](#configuraciones-específicas)
7. [Verificación y Testing](#verificación-y-testing)
8. [Solución de Problemas](#solución-de-problemas)
9. [Checklist Final](#checklist-final)

---

## Requisitos Previos

### Versiones Mínimas Requeridas

- **Node.js**: 18.0.0 o superior
- **Next.js**: 13.0.0 o superior (App Router requerido)
- **React**: 18.0.0 o superior
- **TypeScript**: 5.0.0 o superior

### Características del Proyecto Destino

- ✅ Next.js con App Router habilitado
- ✅ TypeScript configurado
- ✅ Tailwind CSS 4 configurado
- ✅ Sistema de paths con alias `@/*` configurado

---

## Dependencias

### 1. Instalar Dependencias NPM

Ejecutar en el proyecto destino:

```bash
npm install @radix-ui/react-slot class-variance-authority lucide-react mammoth pdfjs-dist
```

O con yarn:

```bash
yarn add @radix-ui/react-slot class-variance-authority lucide-react mammoth pdfjs-dist
```

### 2. Dependencias por Versión

#### Para Versión Word (`review_word_resaltador_with_anclaje`):
- `mammoth` (^1.11.0) - Conversión de .docx a HTML

#### Para Versión PDF (`review_pdf_resaltador_with_anclaje`):
- `pdfjs-dist` (^5.4.530) - Renderizado de PDFs

#### Comunes a Ambas Versiones:
- `@radix-ui/react-slot` (^1.2.4) - Para componentes UI
- `class-variance-authority` (^0.7.1) - Para variantes de componentes
- `lucide-react` (^0.563.0) - Iconos

---

## Estructura de Archivos

### Archivos a Copiar

```
src/
├── components/
│   ├── review_word_resaltador_with_anclaje/
│   │   ├── CommentsPanel.tsx
│   │   ├── DocumentViewer.tsx
│   │   ├── HighlightLayer.tsx
│   │   ├── PageCanvas.tsx
│   │   ├── PinsLayer.tsx
│   │   └── Toolbar.tsx
│   │
│   ├── review_pdf_resaltador_with_anclaje/
│   │   ├── CommentsPanel.tsx
│   │   ├── DocumentViewer.tsx
│   │   ├── HighlightLayer.tsx (opcional)
│   │   ├── PageCanvas.tsx
│   │   ├── PaintLayer.tsx
│   │   ├── PdfPageRenderer.tsx
│   │   ├── PinsLayer.tsx
│   │   └── Toolbar.tsx
│   │
│   └── ui/  (Componentes shadcn/ui)
│       ├── button.tsx
│       ├── card.tsx
│       ├── drawer.tsx
│       ├── textarea.tsx
│       └── tooltip.tsx
│
├── types/
│   └── comments.ts
│
├── hooks/
│   └── use-media-query.ts
│
└── lib/
    └── utils.ts
```

### Archivos de Página (Crear en el proyecto destino)

```
app/
├── review-word/
│   └── page.tsx
└── review-pdf/
    └── page.tsx
```

---

## Configuración del Proyecto

### 1. TypeScript Configuration

Asegurar que `tsconfig.json` tenga el path alias configurado:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

### 2. Next.js Configuration

Verificar que `next.config.js` o `next.config.ts` esté configurado correctamente. No se requieren configuraciones especiales, pero si usas webpack personalizado, asegúrate de que maneje correctamente los módulos de `pdfjs-dist` y `mammoth`.

### 3. Tailwind CSS

Asegurar que Tailwind CSS 4 esté configurado y que las clases utilizadas estén disponibles. Las clases principales utilizadas son:

- Sistema de colores: `zinc-*`, `blue-*`, `yellow-*`, `red-*`
- Layout: `flex`, `grid`, `absolute`, `relative`
- Spacing: `p-*`, `m-*`, `gap-*`
- Borders: `border`, `rounded-*`
- Overflow: `overflow-auto`, `overflow-hidden`
- Typography: `text-*`, `font-*`

---

## Pasos de Migración

### Paso 1: Copiar Archivos Base

1. Copiar `src/types/comments.ts` al proyecto destino
2. Copiar `src/lib/utils.ts` al proyecto destino
3. Copiar `src/hooks/use-media-query.ts` al proyecto destino

### Paso 2: Copiar Componentes UI

Copiar todos los archivos de `src/components/ui/` necesarios:

- `button.tsx` (requerido)
- `card.tsx` (requerido)
- `drawer.tsx` (requerido)
- `textarea.tsx` (requerido)
- `tooltip.tsx` (requerido)

**Nota**: Si el proyecto destino ya tiene componentes UI similares, deberás adaptar los imports o reemplazar los componentes.

### Paso 3: Copiar Componentes de Versión Word

Copiar toda la carpeta `src/components/review_word_resaltador_with_anclaje/` al proyecto destino.

### Paso 4: Copiar Componentes de Versión PDF

Copiar toda la carpeta `src/components/review_pdf_resaltador_with_anclaje/` al proyecto destino.

### Paso 5: Crear Páginas

#### Para Versión Word:

Crear `app/review-word/page.tsx`:

```typescript
"use client";

import { DocumentViewer } from "@/components/review_word_resaltador_with_anclaje/DocumentViewer";

export default function ReviewWordPage() {
  return <DocumentViewer />;
}
```

#### Para Versión PDF:

Crear `app/review-pdf/page.tsx`:

```typescript
"use client";

import { DocumentViewer } from "@/components/review_pdf_resaltador_with_anclaje/DocumentViewer";

export default function ReviewPdfPage() {
  return <DocumentViewer />;
}
```

### Paso 6: Verificar Imports

Revisar que todos los imports usen el alias `@/` correctamente:

```typescript
// ✅ Correcto
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";
import type { AnchorComment } from "@/types/comments";

// ❌ Incorrecto
import { Button } from "../../components/ui/button";
```

---

## Configuraciones Específicas

### Configuración de PDF.js Worker

En `review_pdf_resaltador_with_anclaje/DocumentViewer.tsx`, el worker de PDF.js se configura automáticamente, pero si necesitas usar un worker local:

1. Descargar el worker de PDF.js:
```bash
npm install pdfjs-dist
```

2. Copiar el worker a `public/pdf.worker.min.mjs`

3. Modificar la configuración en `DocumentViewer.tsx`:

```typescript
// En lugar de CDN
pdfjsLib.GlobalWorkerOptions.workerSrc = 
  `https://unpkg.com/pdfjs-dist@${version}/build/pdf.worker.min.mjs`;

// Usar worker local
pdfjsLib.GlobalWorkerOptions.workerSrc = '/pdf.worker.min.mjs';
```

### Configuración de Mammoth (Word)

`mammoth.js` funciona directamente en el navegador. No requiere configuración adicional, pero asegúrate de que:

- Los archivos `.docx` se carguen correctamente
- El CORS esté configurado si cargas archivos desde otro dominio

### Estilos Globales

Asegurar que los estilos globales incluyan:

```css
/* En tu archivo globals.css o similar */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## Verificación y Testing

### Checklist de Funcionalidades

#### Versión Word:

- [ ] Carga de archivos `.docx` funciona
- [ ] Visualización correcta del documento
- [ ] Paginación correcta (múltiples páginas)
- [ ] Selección de texto funciona
- [ ] Resaltado de texto se crea correctamente
- [ ] Comentarios se crean al resaltar
- [ ] Edición de comentarios funciona
- [ ] Eliminación de highlights funciona
- [ ] Navegación entre comentarios y highlights funciona
- [ ] Panel responsive en móvil funciona

#### Versión PDF:

- [ ] Carga de archivos `.pdf` funciona
- [ ] Renderizado correcto del PDF
- [ ] Zoom in/out funciona (50% - 300%)
- [ ] Reset de zoom funciona
- [ ] Herramienta de pan funciona
- [ ] Pintura estilo paintbrush funciona
- [ ] Preview en tiempo real mientras se pinta
- [ ] Cursor de lápiz se muestra correctamente
- [ ] Comentarios se crean al pintar
- [ ] Edición de comentarios funciona
- [ ] Eliminación de highlights funciona
- [ ] Scroll horizontal y vertical funciona con zoom
- [ ] Altura A4 se mantiene fija
- [ ] Panel responsive en móvil funciona

### Testing Manual

1. **Cargar un documento Word**:
   - Subir un archivo `.docx`
   - Verificar que se renderiza correctamente
   - Verificar paginación

2. **Cargar un documento PDF**:
   - Subir un archivo `.pdf`
   - Verificar que se renderiza correctamente
   - Probar zoom y pan

3. **Crear comentarios**:
   - Resaltar texto (Word) o pintar (PDF)
   - Verificar que se crea el comentario
   - Escribir texto en el comentario
   - Guardar y verificar persistencia en estado

4. **Editar comentarios**:
   - Hacer clic en icono de edición
   - Modificar texto
   - Guardar cambios

5. **Eliminar highlights**:
   - Pasar mouse sobre highlight
   - Hacer clic en botón de eliminar
   - Verificar que se elimina highlight y comentario asociado

6. **Navegación**:
   - Hacer clic en comentario → verificar que se selecciona highlight
   - Hacer clic en highlight → verificar que se selecciona comentario

---

## Solución de Problemas

### Error: "Cannot find module '@/...'"

**Solución**: Verificar que `tsconfig.json` tenga el path alias configurado:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

### Error: "pdfjsLib is not defined" o problemas con PDF.js

**Solución**: 
1. Verificar que `pdfjs-dist` esté instalado
2. Verificar que el worker esté configurado antes de usar PDF.js
3. Revisar la consola del navegador para errores de CORS

### Error: "mammoth is not defined" o problemas con Word

**Solución**:
1. Verificar que `mammoth` esté instalado
2. Asegurar que se importa correctamente: `import mammoth from 'mammoth/mammoth.browser'`
3. Verificar que el archivo se carga como ArrayBuffer

### Estilos no se aplican correctamente

**Solución**:
1. Verificar que Tailwind CSS esté configurado
2. Verificar que las clases de Tailwind estén incluidas en el build
3. Revisar `tailwind.config.js` para asegurar que escanea los archivos correctos

### Componentes UI no funcionan

**Solución**:
1. Verificar que todas las dependencias de shadcn/ui estén instaladas
2. Verificar que los componentes UI estén copiados correctamente
3. Revisar que `cn` utility function esté disponible

### Problemas con responsive/móvil

**Solución**:
1. Verificar que `use-media-query` hook esté copiado
2. Verificar que el Drawer component esté disponible
3. Probar en diferentes tamaños de pantalla

### Zoom no funciona en PDF

**Solución**:
1. Verificar que el estado `zoom` se actualiza correctamente
2. Verificar que `PdfPageRenderer` recibe el prop `zoom`
3. Revisar que el canvas se redimensiona correctamente

### Highlights no se muestran

**Solución**:
1. Verificar que `HighlightLayer` o `PaintLayer` esté renderizado
2. Verificar que los highlights están en el estado
3. Revisar la consola para errores de renderizado

---

## Checklist Final

### Pre-migración

- [ ] Proyecto destino tiene Next.js 13+ con App Router
- [ ] TypeScript configurado
- [ ] Tailwind CSS 4 configurado
- [ ] Path alias `@/*` configurado en tsconfig.json

### Dependencias

- [ ] `@radix-ui/react-slot` instalado
- [ ] `class-variance-authority` instalado
- [ ] `lucide-react` instalado
- [ ] `mammoth` instalado (si usas versión Word)
- [ ] `pdfjs-dist` instalado (si usas versión PDF)

### Archivos Copiados

- [ ] `src/types/comments.ts`
- [ ] `src/lib/utils.ts`
- [ ] `src/hooks/use-media-query.ts`
- [ ] Componentes UI copiados
- [ ] Componentes de versión Word copiados
- [ ] Componentes de versión PDF copiados

### Configuración

- [ ] Páginas creadas (`app/review-word/page.tsx` y/o `app/review-pdf/page.tsx`)
- [ ] Imports verificados (usando alias `@/`)
- [ ] Worker de PDF.js configurado (si aplica)
- [ ] Estilos globales configurados

### Testing

- [ ] Carga de documentos funciona
- [ ] Resaltado/pintura funciona
- [ ] Comentarios se crean correctamente
- [ ] Edición de comentarios funciona
- [ ] Eliminación funciona
- [ ] Navegación bidireccional funciona
- [ ] Zoom y pan funcionan (PDF)
- [ ] Responsive funciona

### Post-migración

- [ ] Código funciona sin errores
- [ ] No hay warnings en consola
- [ ] Performance aceptable
- [ ] Documentación actualizada

---

## Notas Adicionales

### Estado Local

**Importante**: Todo el estado se mantiene en memoria del navegador. Los comentarios y highlights se pierden al recargar la página. Si necesitas persistencia, deberás implementar:

- LocalStorage/SessionStorage
- Backend API
- Base de datos

### Personalización

Los componentes están diseñados para ser personalizables:

- **Colores**: Modificar clases de Tailwind en los componentes
- **Estilos**: Ajustar clases CSS según necesidad
- **Funcionalidad**: Extender o modificar la lógica según requerimientos

### Performance

Para documentos grandes:

- Considerar paginación lazy loading
- Optimizar re-renders con `React.memo` y `useMemo`
- Considerar virtualización para listas largas de comentarios

---

## Recursos Adicionales

- [Documentación de Next.js](https://nextjs.org/docs)
- [Documentación de PDF.js](https://mozilla.github.io/pdf.js/)
- [Documentación de Mammoth](https://github.com/mwilliamson/mammoth.js)
- [Documentación de Tailwind CSS](https://tailwindcss.com/docs)
- [shadcn/ui Components](https://ui.shadcn.com/)

---

## Soporte

Para problemas o preguntas sobre la migración, revisar:

1. Esta guía completa
2. Código fuente original
3. Consola del navegador para errores
4. Logs del servidor de desarrollo

---

**Última actualización**: 2024
**Versión de componentes**: Basado en commenteditordemo

