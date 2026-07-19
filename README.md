# DBML Studio

-  ██████╗  █████╗ ████████╗ █████╗ ██████╗  █████╗ ███████╗███████╗
-  ██╔══██╗██╔══██╗╚══██╔══╝██╔══██╗██╔══██╗██╔══██╗██╔════╝██╔════╝
-  ██║  ██║███████║   ██║   ███████║██████╔╝███████║███████╗█████╗  
-  ██║  ██║██╔══██║   ██║   ██╔══██║██╔══██╗██╔══██║╚════██║██╔══╝  
-  ██████╔╝██║  ██║   ██║   ██║  ██║██████╔╝██║  ██║███████║███████╗
-  ╚═════╝ ╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═════╝ ╚═╝  ╚═╝╚══════╝╚══════╝
-                                                                  
-  ██████╗ ██████╗ ███╗   ███╗██╗     
-  ██╔══██╗██╔══██╗████╗ ████║██║     
-  ██║  ██║██████╔╝██╔████╔██║██║     
-  ██║  ██║██╔══██╗██║╚██╔╝██║██║     
-  ██████╔╝██████╔╝██║ ╚═╝ ██║███████╗
-  ╚═════╝ ╚═════╝ ╚═╝     ╚═╝╚══════╝
-                                    
-  ██████╗████████╗██╗   ██╗██████╗ ██╗ ██████╗ 
- ██╔════╝╚══██╔══╝██║   ██║██╔══██╗██║██╔═══██╗
- ███████╗   ██║   ██║   ██║██║  ██║██║██║   ██║
- ╚════██║   ██║   ██║   ██║██║  ██║██║██║   ██║
- ███████║   ██║   ╚██████╔╝██████╔╝██║╚██████╔╝
- ╚══════╝   ╚═╝    ╚═════╝ ╚═════╝ ╚═╝ ╚═════╝ 


Editor y visualizador de diagramas de bases de datos en un único archivo HTML autocontenido. Escribe en texto plano (DBML, UML o DFD) a la izquierda y ve el diagrama renderizarse en tiempo real a la derecha — con selección múltiple, layouts automáticos, exportación, múltiples proyectos guardados y soporte táctil completo.

**No requiere instalación, build ni backend.** Es un solo archivo `index.html` que corre 100% en el navegador.

## ✨ Demo rápida

Abrí `index.html` en cualquier navegador (doble clic, o `python3 -m http.server` si preferís servirlo) y vas a ver un esquema de ejemplo ya cargado, listo para editar.


## Características

**Edición**
- Editor con resaltado de sintaxis y números de línea (CodeMirror)
- Detección de errores en vivo: relaciones rotas, nombres y campos duplicados, marcados en rojo con tooltip
- Historial de deshacer/rehacer (hasta 100 pasos)
- Normalización automática de acentos/diacríticos para evitar romper el parser

**Tres tipos de diagrama, un solo editor**
- 📐 **DBML / Entidad-Relación** — el formato principal, con `Table`, `Ref`, `Enum`, `TableGroup` y cardinalidad real (`>`, `<`, `-`, `<>`) representada con notación crow's-foot
- 🧩 **UML** — diagramas de clases con atributos, visibilidad (+/-/#), métodos, asociaciones con multiplicidad y herencia
- 🔀 **DFD** — diagramas de flujo de datos (procesos, entidades externas, almacenes de datos y flujos)
- Botón **"⚡ Generar desde…"** para crear un modelo a partir de otro ya existente, sin partir de cero

**Interacción visual**
- Selección múltiple (clic, shift/ctrl-clic, recuadro de selección, modo táctil sin teclado)
- Alinear y distribuir tablas seleccionadas
- Snap to grid opcional
- Nudge con flechas del teclado
- Edición inversa: doble clic en una tabla (DBML) para renombrarla o editar sus campos
- Duplicar / eliminar selección (`Ctrl+D` / `Supr`)
- Zoom a selección, minimapa con navegación, pan y pinch-zoom táctil

**Organización de esquemas grandes**
- 6 layouts automáticos, incluyendo un layout de fuerza dirigida real (Fruchterman-Reingold)
- Agrupar tablas por color, relación o tamaño
- `TableGroup`: marcos de agrupación colapsables

**Persistencia**
- Múltiples proyectos guardados con nombre (cada uno con sus 3 modelos, posiciones, tema y layout)
- 5 slots de "vista guardada" (posición de cámara + zoom)
- Todo vive en `localStorage` del navegador — no hay servidor ni cuenta

**Exportación**
- SVG, PNG y `.dbml` en un clic

**Mobile-first**
- Layout apilado con pestañas Editor/Diagrama en pantallas chicas
- Toolbar con scroll horizontal, objetivos táctiles más grandes
- Gestos táctiles reales: pan de un dedo, pinch-to-zoom, arrastre de tablas y del minimapa

**3 temas visuales**: Slate Oscuro, Cyberpunk Neón, Minimalista Claro

## Uso

1. Abrí `index.html` en el navegador.
2. Editá el texto DBML/UML/DFD en el panel izquierdo — el diagrama se actualiza solo (debounce de 500ms).
3. Arrastrá las tablas para acomodarlas, o usá alguno de los layouts automáticos.
4. Guardá tu trabajo como un "modelo" con nombre (botón **＋ Nuevo** junto al selector de proyectos) para poder volver a cargarlo después.
5. Exportá como SVG/PNG cuando quieras compartir el diagrama, o como `.dbml` para llevarte el texto fuente.

## Formatos de texto soportados

### DBML (Entidad-Relación)

```dbml
Table users [headercolor: #6366f1] {
  id int [pk, increment]
  name varchar(120)
  role user_role
}

Enum user_role {
  customer
  admin
}

Ref: posts.user_id > users.id   // > 1-N   < N-1   - 1-1   <> N-N

TableGroup Ventas {
  users
  posts
}
```

### UML (clases)

```
class Usuario {
  +id: int
  -password: string
  +iniciarSesion()
}

Usuario "1" --> "N" Pedido : realiza
Usuario <|-- Admin
```

### DFD (flujo de datos)

```
Entity E1 "Cliente"
Process P1 "Procesar Pedido"
Store D1 "Base de Pedidos"

E1 -> P1 : "Datos del pedido"
P1 -> D1 : "Guardar pedido"
```

## Atajos de teclado

| Atajo | Acción |
|---|---|
| `F` | Encuadrar la selección actual |
| `Flechas` | Mover la selección 1px (`Shift` + flecha = tamaño de grid) |
| `Ctrl/Cmd + D` | Duplicar selección |
| `Supr` / `Backspace` | Eliminar selección |
| `Escape` | Deseleccionar todo |
| `Shift` + arrastrar (fondo) | Recuadro de selección |

Los atajos se desactivan automáticamente mientras el foco está en el editor de texto o en un campo de un modal.

## Stack técnico y licencias

Todas las dependencias se cargan desde CDN público, sin instalación:

| Librería | Uso | Licencia |
|---|---|---|
| [JointJS](https://www.jointjs.com/) | Motor del diagrama (SVG, drag & drop, routing) | MPL-2.0 |
| [CodeMirror 5](https://codemirror.net/5/) | Editor de texto con resaltado de sintaxis | MIT |
| [Tailwind CSS](https://tailwindcss.com/) (Play CDN) | Estilos | MIT |
| jQuery, Lodash, Backbone.js | Dependencias internas de JointJS | MIT |

Ninguna de estas licencias impide usar, modificar o publicar este proyecto, incluso comercialmente. El único aviso a tener en cuenta es que el CDN de Tailwind (`cdn.tailwindcss.com`) compila el CSS en el navegador en cada carga — funciona perfecto, pero no es la forma más liviana posible; es un tema de rendimiento, no de licencia.

## Limitaciones conocidas

## Roadmap
