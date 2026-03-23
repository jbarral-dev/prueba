---
name: figma-accessibility-frames
description: "Genera los 3 frames de accesibilidad en Figma para cualquier componente del Design System R4: Roles, Orden de lectura y Orden de foco. Usa esta skill cuando el usuario pida anotar accesibilidad de un componente, crear frames de accesibilidad, documentar roles ARIA, orden de foco o de lectura, o mencione 'accesibilidad', 'frames de accesibilidad', 'anotar roles', 'orden de foco' o 'orden de lectura' para un componente de Figma."
---

# Skill: figma-accessibility-frames

## Descripción
Genera los 3 frames de accesibilidad en Figma para cualquier componente del Design System R4: **Roles**, **Orden de lectura por agrupación** y **Orden de foco por teclado**. Usa los mismos componentes y estilos del kit de anotaciones del archivo `Renta 4 - Dashboard accesibilidad` (fileKey: `6LRhrm9IZCFtNc0sBDlUoD`).

---

## ⚠️ REGLAS OBLIGATORIAS — Leer antes de ejecutar

1. **NUNCA crear una página nueva.** Los frames de accesibilidad se añaden SIEMPRE a la **misma página** donde ya existe el componente en Figma. Usar `figma.currentPage` del Desktop Bridge, NO `figma.createPage()`.

2. **Referencia visual exacta obligatoria.** Antes de generar los frames, inspeccionar el ejemplo de referencia en el mismo archivo:
   - **Componente de referencia**: R4InputIcon
   - **nodeId de referencia**: `6555-4320`
   - **Archivo**: `LXKe53jePjDEAVhdYCU4ed` (R4 Design System | Primitivos | 1.2)
   - Los 3 frames generados deben tener exactamente la misma estructura visual que este ejemplo.

3. **Solo 3 frames, exactamente estos:**
   - `Roles`
   - `Orden de lectura por agrupación`
   - `Orden de foco por teclado`
   No generar documentación adicional (checklists WCAG, tablas de contraste, etc.).

4. **Herramienta de ejecución**: Usar `figma_execute` (Desktop Bridge plugin), **no** la Figma REST API.

5. **Posición**: Los 3 frames se colocan **debajo** del componente principal, en fila horizontal, en la **misma sección/canvas** donde vive el componente.

---

## Proceso de ejecución paso a paso

### Paso 1 — Inspeccionar el componente objetivo
```
- Obtener nodeId del componente a documentar
- Usar figma_get_figma_data para obtener estructura, posición (x, y), dimensiones
- Identificar: tipo de componente, sub-elementos, variantes, elementos interactivos
```

### Paso 2 — Inspeccionar la referencia visual
```
- Obtener nodeId 6555-4320 del mismo archivo (R4InputIcon)
- Examinar la estructura exacta de sus 3 frames de accesibilidad
- Reproducir el mismo estilo visual para el componente nuevo
```

### Paso 3 — Obtener posición del componente en la página
```javascript
// Ejecutar en figma_execute para obtener posición real del componente
const node = figma.getNodeById('NODE_ID_DEL_COMPONENTE');
const parent = node.parent;
return {
  x: node.absoluteBoundingBox?.x ?? node.x,
  y: node.absoluteBoundingBox?.y ?? node.y,
  w: node.width,
  h: node.height,
  parentId: parent?.id,
  parentType: parent?.type
};
```

### Paso 4 — Crear los 3 frames con figma_execute
Usar `await figma.setCurrentPageAsync(page)` — NUNCA `figma.currentPage = page`.

---

## Contexto del sistema de anotaciones

### Archivo fuente de componentes
- **File**: Renta 4 - Dashboard accesibilidad (`6LRhrm9IZCFtNc0sBDlUoD`)
- **Sección**: `Kit Anotaciones` (node `44:5383`)

### Componentes disponibles para anotaciones

#### `elements` (ComponentSet `16425:3401`)
Callouts/etiquetas con flecha. Variantes:
- **Heading Level**: `p`, `dt`, `dd`, `dl`, `li`, `ul`, `ol`
- **Callout Direction**: `Down`, `Up`, `Left`, `Right`, `---`
- **target area**: `true` | `false`

Colores según tipo semántico:
- `dt` → `#FA9902` (naranja) + texto blanco
- `dd` → naranja claro + texto negro
- `p` → `#FA9902` + texto negro
- Role badges → `#C97A00` (marrón-naranja) + texto blanco
- ARIA badges → `#AA3C2B` (rojo-marrón) + texto blanco

#### `role` (ComponentSet `16425:3473`)
Badge para `role=` ARIA. Variantes: `group`, `region`, `form`, `listbox`, `option`, `button`, `dialog`, `alert`, `status`, `navigation`, `main`, `banner`, `contentinfo`, `search`, etc.

#### `ARIA` (ComponentSet `16425:3489`)
Badge para atributos ARIA. Variantes: `aria-label`, `aria-live`, `aria-labelledby`, `aria-describedby`, `aria-required`, `aria-invalid`, `aria-expanded`, `aria-controls`, `aria-haspopup`, `aria-hidden`, `aria-disabled`, `aria-checked`, `aria-selected`, `aria-current`, `aria-atomic`, `aria-relevant`.

#### `_pointer` (ComponentSet `16425:3379`)
Flechas sueltas: `point=down`, `point=up`, `point=left`, `point=right`

---

## Estructura visual de los 3 frames

### Layout de cada frame
```
Frame contenedor (e.g. "ButtonCircle / Accesibilidad / Roles"):
  - layout: COLUMN, gap: 48px
  - padding: 64px
  - fill: #F5F5F5 (gris muy claro) o según referencia visual R4InputIcon
  - borderRadius: 24px
  - sizing: hug width y hug height

  ├── Name (badge-título del frame)
  │     - TEXT: "Roles" / "Orden de lectura por agrupación" / "Orden de foco por teclado"
  │     - fontFamily: Inter, fontWeight: 700, fontSize: 40-64px
  │     - fill: blanco, borderRadius: 16px, padding: 32px 48px
  │
  └── Anatomy (zona de anotaciones)
        - fill: #FFFFFF
        - borderRadius: 24px
        - padding: 80px
        - Posicionamiento ABSOLUTO de callouts sobre instancia del componente
```

Los 3 frames se posicionan en fila horizontal con gap de `128px` entre ellos, **debajo** del frame principal del componente:
```
Y_frames = componentY + componentHeight + 100
X_frame1 = componentX                         (Roles)
X_frame2 = componentX + frameWidth + 128      (Orden de lectura)
X_frame3 = componentX + (frameWidth+128)*2    (Orden de foco)
```

---

## Frame 1: ROLES

### Propósito
Mostrar el etiquetado HTML semántico y los atributos `role=` / `aria-*` de cada parte del componente.

### Contenido
1. **1-2 instancias del componente** (Default y opcionalmente Focused/Error)
2. **Callouts `elements`** apuntando a cada sub-elemento con su etiqueta HTML (`p`, `dt`, `dd`, etc.)
3. **Badges `role=`** apuntando al elemento que recibe ese role
4. **Badges `aria-*`** apuntando al elemento con ese atributo

### Reglas de roles por tipo de componente

| Componente | HTML/Role | aria-* relevantes |
|---|---|---|
| Botón de acción (solo icono) | `<button>` | `aria-label` (obligatorio), `aria-disabled` |
| Botón con texto visible | `<button>` | `aria-disabled` |
| Input texto | `<input type="text">` | `aria-label`/`aria-labelledby`, `aria-required`, `aria-invalid`, `aria-describedby` |
| Select | `role="listbox"` o `<select>` | `aria-expanded`, `aria-haspopup`, `aria-controls` |
| Checkbox | `<input type="checkbox">` | `aria-checked`, `aria-label` |
| Icono decorativo | `<svg aria-hidden="true">` | `aria-hidden="true"` |
| Grupo de campos | `<fieldset>` / `role="group"` | `aria-labelledby` |

**Para ButtonCircle específicamente:**
- Contenedor: `<button>` con `role="button"` implícito
- Icono SVG: `aria-hidden="true"` (decorativo, el texto label lo describe)
- `aria-label="Pedir cita"` (necesario porque el label visual podría no bastar)
- `aria-disabled="true/false"` en estado Disabled

---

## Frame 2: ORDEN DE LECTURA POR AGRUPACIÓN

### Propósito
Mostrar el orden en que un lector de pantalla recorre los elementos del componente, organizando por grupos lógicos.

### Contenido
1. **Instancia del componente** (estado Default)
2. **Bloques numerados** (rectángulos o badges con números 1, 2, 3…) sobre cada zona del componente que será leída
3. **Callouts `dl`/`dt`/`dd`** describiendo qué se lee en cada grupo

### Convención
- El orden sigue la secuencia DOM: arriba → abajo, izquierda → derecha
- Numerar solo los bloques de contenido significativo (no elementos `aria-hidden`)
- Indicar textos `aria-live` con nota "(leído asíncronamente)"

### Orden para ButtonCircle
```
1 → Icono (aria-hidden="true", NO leído) — señalar con "aria-hidden"
2 → Label "Pedir cita" + anuncio del rol "botón"
[Si disabled] → "No disponible" (aria-disabled)
```

---

## Frame 3: ORDEN DE FOCO POR TECLADO

### Propósito
Mostrar el orden en que el foco del teclado (Tab / Shift+Tab) recorre los elementos interactivos del componente.

### Contenido
1. **Instancia del componente** (estado Focused si existe, o Default)
2. **Badges numéricos** (1, 2, 3…) sobre cada elemento que recibe foco
3. **Anillo de foco visual** sobre el elemento activo
4. **Indicadores de tecla** (Tab, Space, Enter) opcionales

### Convención
- Solo elementos **interactivos** aparecen (no textos estáticos, no `aria-hidden`)
- Los elementos `Disabled` NO aparecen en el orden de foco (tabindex="-1")
- El anillo de foco del DS: outline 3px, color `#2B7FFF`, offset 2px

### Orden para ButtonCircle
```
Foco 1 → El botón completo (Tab)
         → Enter / Space: activa la acción
         → Disabled: omitido del tab order (tabindex="-1")
```

---

## Naming convention de los frames

```
[NombreComponente] / Accesibilidad / Roles
[NombreComponente] / Accesibilidad / Orden de lectura por agrupación
[NombreComponente] / Accesibilidad / Orden de foco por teclado
```

Ejemplos:
- `ButtonCircle / Accesibilidad / Roles`
- `ButtonCircle / Accesibilidad / Orden de lectura por agrupación`
- `ButtonCircle / Accesibilidad / Orden de foco por teclado`

---

## Ejemplo concreto: ButtonCircle

### Roles
```
<button aria-label="Pedir cita" aria-disabled="false">
  <svg aria-hidden="true"><!-- calendar icon --></svg>
  <span>Pedir cita</span>   ← text visible
</button>
```
Callouts:
1. Badge `button` → sobre el contenedor del componente
2. Badge `aria-label="Pedir cita"` → sobre el icono/componente
3. Badge `aria-hidden="true"` → sobre el SVG del icono
4. Badge `aria-disabled` → sobre la variante Disabled

### Orden de lectura
```
1. [aria-hidden] Icono — NO leído
2. "Pedir cita, botón" — texto + rol anunciado por el AT
```

### Orden de foco
```
Foco 1 → Botón completo (Tab)
         → Activar: Space / Enter
         → Disabled: tabindex="-1", omitido
```

---

## Colores y estilos del kit de anotaciones

| Token | Color | Uso |
|---|---|---|
| `Soporte/alert/base` | `#FA9902` | Badge `dt` |
| `Soporte/alert/light` | `#FEE685` | Badge `dd` |
| `Soporte/info/base` | `#FA9902` | Badge `p` |
| `Soporte/info/dark` | `#C97A00` | Badge `role=` |
| `Gráficas/2` | `#AA3C2B` | Badge `aria-*` |
| Fondo frames | `#F5F5F5` | Background de cada frame |
| Anatomy fill | `#FFFFFF` | Zona de anotaciones |
| Anillo foco | `#2B7FFF` | Outline 3px del foco |

Tipografía de badges: Open Sans, 700, 14px.

---

## Checklist antes de entregar

- [ ] Los 3 frames están en la **misma página** que el componente (nunca página nueva)
- [ ] Los nombres siguen la convención `[Componente] / Accesibilidad / [Tipo]`
- [ ] Los 3 frames se inspeccionaron contra el ejemplo R4InputIcon (`6555-4320`)
- [ ] Frame Roles: incluye `button`, `aria-label`, `aria-hidden` en el icono, `aria-disabled`
- [ ] Frame Lectura: orden correcto, icono marcado como `aria-hidden`
- [ ] Frame Foco: solo el botón activo (Disabled excluido), anillo de foco visible
- [ ] Los textos de badges son exactos: `role="button"`, `aria-label="Pedir cita"`, etc.
