# Editor de Comentarios Anclados

Editor de comentarios tipo Word con soporte para documentos Word (.docx) y PDF. Permite crear comentarios anclados, resaltar texto y gestionar revisiones de documentos de forma interactiva.

## 🚀 Características Principales

- **Comentarios anclados**: Sistema de comentarios vinculados a posiciones específicas en el documento
- **Resaltado de texto**: Herramienta para resaltar texto independiente o vinculado a comentarios
- **Soporte multi-formato**: Carga y visualización de documentos Word (.docx) y PDF
- **Edición inline**: Edición directa de comentarios desde el panel lateral
- **Zoom y navegación**: Control de zoom y herramienta de pan para documentos PDF
- **Estado local**: Todo funciona con estado local de React, sin necesidad de backend o base de datos

## 🛠️ Tecnologías Utilizadas

- **Next.js 16.1.6** (App Router) - Framework React
- **React 19.2.3** - Biblioteca de UI
- **TypeScript** - Tipado estático
- **Tailwind CSS 4** - Estilos utilitarios
- **mammoth.js** - Conversión de documentos Word (.docx) a HTML
- **pdfjs-dist 5.4.530** - Renderizado de documentos PDF
- **shadcn/ui** - Componentes UI (Button, Card, Input, Textarea, Tooltip, Drawer)
- **lucide-react** - Iconos

## 📦 Instalación

### Requisitos previos

- Node.js 18+ 
- npm o yarn

### Pasos de instalación

1. Clonar el repositorio:
```bash
git clone https://github.com/code4ce-latam/commenteditordemo.git
cd commenteditordemo
```

2. Instalar dependencias:
```bash
npm install
```

3. Ejecutar el servidor de desarrollo:
```bash
npm run dev
```

4. Abrir en el navegador:
```
http://localhost:3000
```

### Scripts disponibles

- `npm run dev` - Inicia el servidor de desarrollo
- `npm run build` - Construye la aplicación para producción
- `npm run start` - Inicia el servidor de producción
- `npm run lint` - Ejecuta el linter

## 📁 Estructura del Proyecto

```
src/
├── app/
│   ├── review/                          # Página principal (selector de versiones)
│   ├── review_original_word/            # Versión Original Word
│   ├── review_word_resaltador/          # Versión con Resaltador
│   ├── review_word_resaltador_with_anclaje/  # Versión Word con Resaltador y Anclaje
│   └── review_pdf_resaltador_with_anclaje/   # Versión PDF con Resaltador y Anclaje
├── components/
│   ├── ui/                              # Componentes shadcn/ui
│   └── review_*/                        # Componentes específicos por versión
├── types/
│   └── comments.ts                      # Tipos TypeScript
├── hooks/
│   └── use-media-query.ts               # Hook para responsive
└── lib/
    └── utils.ts                         # Utilidades
```

## 🎯 Versiones Disponibles

El proyecto incluye 4 versiones diferentes del editor, cada una con funcionalidades específicas:

### 1. Versión Original Word (`/review_original_word`)

Versión base con anclajes posicionales y comentarios. Sin funcionalidad de resaltado.

**Funcionalidades:**
- ✅ Anclajes posicionales (x, y)
- ✅ Comentarios anclados
- ✅ Carga de documentos Word (.docx)
- ✅ Edición inline de comentarios
- ✅ Panel lateral de comentarios
- ✅ Navegación bidireccional entre anclajes y comentarios

### 2. Versión con Resaltador (`/review_word_resaltador`)

Incluye todas las funcionalidades de la versión original más herramienta de resaltado de texto independiente.

**Funcionalidades:**
- ✅ Todas las funcionalidades de la versión original
- ✅ Resaltado de texto independiente
- ✅ Selección precisa de texto
- ✅ Eliminación de highlights con doble clic
- ✅ Visualización de highlights en el documento

### 3. Versión con Resaltador y Anclaje - Word (`/review_word_resaltador_with_anclaje`)

Versión completa que combina resaltado de texto independiente con anclajes posicionales y comentarios.

**Funcionalidades:**
- ✅ Resaltado de texto independiente
- ✅ Anclajes posicionales (x, y)
- ✅ Comentarios anclados
- ✅ Carga de documentos Word (.docx)
- ✅ Edición inline de comentarios
- ✅ Eliminación de highlights con botón en hover
- ✅ Creación automática de comentario al resaltar
- ✅ Vinculación entre highlights y comentarios
- ✅ Indicadores visuales de comentarios en highlights

### 4. Versión PDF con Resaltador y Anclaje (`/review_pdf_resaltador_with_anclaje`)

Versión completa para PDF que combina resaltado de texto independiente con anclajes posicionales y comentarios.

**Funcionalidades:**
- ✅ Resaltado de texto con pintura estilo paintbrush
- ✅ Anclajes posicionales (x, y)
- ✅ Comentarios anclados
- ✅ Carga de documentos PDF
- ✅ Edición inline de comentarios
- ✅ Eliminación de highlights con botón en hover
- ✅ Creación automática de comentario al resaltar
- ✅ Zoom in/out/reset (50% - 300%)
- ✅ Herramienta de pan para mover el documento
- ✅ Cursor de lápiz en modo resaltar
- ✅ Preview en tiempo real mientras se pinta
- ✅ Altura estándar A4 para páginas
- ✅ Scroll horizontal y vertical cuando hay zoom

## 💡 Características Detalladas

### Sistema de Comentarios Anclados

- **Creación de comentarios**: Los comentarios se crean automáticamente al resaltar texto o manualmente mediante anclajes
- **Edición inline**: Edición directa desde las tarjetas de comentarios en el panel lateral
- **Información del autor**: Cada comentario muestra quién lo creó
- **Eliminación**: Eliminación de comentarios y sus highlights asociados
- **Navegación bidireccional**: Click en un comentario selecciona el highlight/anclaje y viceversa

### Resaltado de Texto

#### Para documentos Word:
- Selección precisa de texto usando rangos DOM
- Resaltado visual con color amarillo por defecto
- Persistencia de highlights en el estado local

#### Para documentos PDF:
- Pintura estilo paintbrush (pincel)
- Trazado libre sobre el documento
- Preview en tiempo real mientras se pinta
- Cursor de lápiz personalizado
- Línea de preview en color #FFFF66

### Carga de Documentos

- **Word (.docx)**: Conversión a HTML usando mammoth.js
- **PDF**: Renderizado usando pdfjs-dist con canvas y capa de texto
- **Paginación**: Visualización correcta de documentos multi-página
- **Soporte para PDFs sin texto seleccionable**: Indicador visual cuando el PDF es escaneado

### Zoom y Navegación (PDF)

- **Zoom**: Control de zoom desde 50% hasta 300%
- **Reset**: Botón para volver al zoom original (100%)
- **Pan**: Herramienta de mano para mover el documento cuando hay zoom
- **Scroll**: Scroll automático horizontal y vertical cuando el contenido excede el contenedor
- **Altura fija**: Contenedor mantiene altura A4 estándar (1123px) sin crecer con el zoom

### Interfaz de Usuario

- **Panel lateral**: Panel de comentarios con scroll independiente
- **Toolbar**: Barra de herramientas con controles de zoom, herramientas y carga de archivos
- **Responsive**: Adaptación a dispositivos móviles con Drawer
- **Feedback visual**: Indicadores de selección, hover y estados activos
- **Tooltips**: Información contextual en los botones

## 🎨 Componentes Principales

### DocumentViewer
Componente principal que gestiona el estado global y orquesta los demás componentes.

### PageCanvas
Renderiza las páginas individuales del documento y maneja las interacciones del usuario.

### CommentsPanel
Panel lateral que muestra la lista de comentarios y permite su edición.

### Toolbar
Barra de herramientas con controles de modo, zoom y carga de archivos.

### HighlightLayer / PaintLayer
Capa que renderiza los highlights sobre el documento.

### PinsLayer
Capa que renderiza los anclajes/pines de comentarios.

## 🔄 Flujo de Datos

El proyecto utiliza **estado local de React** (useState, useRef) para gestionar:

- Lista de comentarios (`AnchorComment[]`)
- Lista de highlights (`TextHighlight[]`)
- Comentario seleccionado (`selectedId`)
- Modo de herramienta activa (`toolMode`)
- Modo de resaltado (`highlightMode`)
- Zoom actual (`zoom`)
- Documento cargado (`pdfDocument` o `documentPagesHtml`)

No hay backend ni base de datos. Todo el estado se mantiene en memoria durante la sesión del navegador.

## 📝 Uso

### Iniciar el proyecto

1. Navegar a la página principal: `http://localhost:3000/review`
2. Seleccionar una versión del editor
3. Cargar un documento (Word o PDF según la versión)
4. Comenzar a crear comentarios y resaltar texto

### Crear un comentario

1. Activar el modo "Resaltar texto"
2. Seleccionar o pintar sobre el texto deseado
3. Se crea automáticamente un comentario vinculado
4. Escribir el comentario en el panel lateral

### Editar un comentario

1. Hacer clic en el icono de edición (✏️) en la tarjeta del comentario
2. Modificar el texto
3. Guardar los cambios

### Eliminar un highlight

1. Pasar el mouse sobre el highlight
2. Hacer clic en el botón de eliminar (X) que aparece
3. Se elimina el highlight y su comentario asociado (si existe)

### Zoom en PDF

1. Usar los botones de zoom (+/-) en el toolbar
2. Usar la herramienta de pan (mano) para mover el documento
3. Resetear el zoom con el botón de reset

## ⚠️ Limitaciones

- **Sin persistencia**: Los comentarios y highlights se pierden al recargar la página
- **Sin backend**: No hay sincronización entre usuarios
- **Estado local**: Todo funciona en el navegador del cliente
- **PDFs escaneados**: Algunos PDFs escaneados pueden no tener texto seleccionable

## 🤝 Contribuir

Este es un proyecto de demostración. Las contribuciones son bienvenidas.

## 📄 Licencia

Este proyecto es privado y pertenece a code4ce-latam.

## 👥 Autores

Desarrollado por code4ce-latam

---

Para más información, visita el repositorio: [https://github.com/code4ce-latam/commenteditordemo](https://github.com/code4ce-latam/commenteditordemo)
