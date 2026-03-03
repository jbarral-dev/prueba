# Skill: figma-accessibility-frames

## Descripción
Genera los 3 frames de accesibilidad en Figma para cualquier componente del Design System R4: **Roles**, **Orden de lectura** y **Orden de foco**. Usa los mismos componentes y estilos del kit de anotaciones del archivo `Renta 4 - Dashboard accesibilidad` (fileKey: `6LRhrm9IZCFtNc0sBDlUoD`).

---

## Contexto del sistema de anotaciones

### Archivo fuente de componentes
- **File**: Renta 4 - Dashboard accesibilidad (`6LRhrm9IZCFtNc0sBDlUoD`)
- **Sección**: `Kit Anotaciones` (node `44:5383`)

### Componentes disponibles para anotaciones

#### `elements` (ComponentSet `16425:3401` en Design System / `44:5384` en Dashboard)
Callouts/etiquetas con flecha. Cada variante combina:
- **Heading Level**: `p`, `dt`, `dd`, `dl`, `li`, `ul`, `ol`
- **Callout Direction**: `Down`, `Up`, `Left`, `Right`, `---` (sin flecha, solo target area)
- **target area**: `true` | `false`

Colores de badge según tipo semántico:
- `dt` → `Soporte/alert/base` (#FA9902, fondo naranja oscuro) + texto blanco
- `dd` → `Soporte/alert/light` (naranja claro) + texto negro
- `p` → `Soporte/info/base` (#FA9902) + texto negro (o según variante)
- Role badges → `Soporte/info/dark` (#C97A00) + texto blanco
- ARIA badges → `Gráficas/2` (#AA3C2B, rojo-marrón) + texto blanco

#### `role` (ComponentSet `16425:3473`)
Badge para anotar `role=` de ARIA. Variantes: `group`, `region`, `form`, `listbox`, `option`, `button`, `dialog`, `alert`, `status`, `navigation`, `main`, `banner`, `contentinfo`, `search`, etc.

#### `ARIA` (ComponentSet `16425:3489`)
Badge para atributos ARIA. Variantes:
- `Link target=aria-label`
- `Link target=aria-live`
- `Link target=aria-labelledby`
- `Link target=aria-describedby`
- `Link target=aria-required`
- `Link target=aria-invalid`
- `Link target=aria-expanded`
- `Link target=aria-controls`
- `Link target=aria-haspopup`
- `Link target=aria-hidden`
- `Link target=aria-disabled`
- `Link target=aria-checked`
- `Link target=aria-selected`
- `Link target=aria-current`
- `Link target=aria-atomic`
- `Link target=aria-relevant`

#### `_pointer` (ComponentSet `16425:3379` / `44:5512`)
Flechas sueltas: `point=down`, `point=up`, `point=left`, `point=right`

---

## Estructura de los 3 frames de accesibilidad

Los 3 frames se posicionan **debajo** del frame principal del componente, en la misma página, **alineados a la derecha** del panel de definición del componente o **en fila horizontal** con un gap de `128px` entre ellos.

### Dimensiones y layout de los frames contenedores

Cada frame sigue este patrón (basado en el ejemplo del Input OTP node `16425:3884`):
```
Frame principal (Roles / Orden de lectura / Orden de foco):
  - layout: ROW, gap: 128px, sizing: fixed width / hug height
  - width: ~844px (ajustar según tamaño del componente)
  
  └── Specification (FRAME)
        - layout: COLUMN, gap: 48px, sizing: fill / fixed
        
        ├── Name (FRAME - badge título del frame)
        │     - fills: #FFFFFF (blanco)
        │     - borderRadius: 24px
        │     - padding: 64px
        │     └── TEXT: "Roles" / "Orden de lectura" / "Orden de foco"
        │           - fontFamily: Inter, fontWeight: 700, fontSize: 64
        
        └── Anatomy (FRAME - zona de anotaciones)
              - fills: #FFFFFF
              - borderRadius: 24px
              - padding: 80px (todos lados) o 80px 270px (izq/der)
              - layout: COLUMN, gap: 94px
              
              └── [Instancias del componente + callouts posicionados en absolute]
```

---

## Frame 1: ROLES

### Propósito
Mostrar el etiquetado HTML semántico: qué elemento HTML corresponde a cada parte del componente, y los atributos `role=` y `aria-*` que se aplican.

### Qué incluir
1. **Instancia del componente** — mostrar 2-3 variantes del componente si tiene estados relevantes (Default, Error, etc.)
2. **Callouts de elemento HTML** (`elements`) — apuntando a cada parte del componente con el nivel semántico:
   - Contenedor principal → `p` o `section` o `form` según corresponda
   - Labels → `p` (párrafo normal)
   - Inputs → `dt` (término de definición) indicando el tipo de campo
   - Captions/helper text → `dd` (descripción de definición)
3. **Badges `role=`** — posicionados con flecha hacia el elemento que recibe ese role
4. **Badges `ARIA`** — posicionados con flecha hacia el elemento que recibe el atributo

### Reglas de inferencia de roles por tipo de componente

| Tipo de componente | Role HTML | aria-* relevantes |
|---|---|---|
| Botón de acción | `<button>` | `aria-label` (si solo icono), `aria-disabled`, `aria-pressed` (toggle) |
| Input de texto | `<input type="text">` | `aria-label` o `aria-labelledby`, `aria-required`, `aria-invalid`, `aria-describedby` |
| Input OTP | `<group>` contenedor + `<input>` × N | `role="group"`, `aria-label="Dígito X de N"`, `aria-live="polite"` (countdown), `aria-labelledby` |
| Select / Dropdown | `<select>` o `role="listbox"` | `aria-label`, `aria-expanded`, `aria-haspopup`, `aria-controls` |
| Checkbox | `<input type="checkbox">` | `aria-checked`, `aria-label`, `aria-required` |
| Radio button | `<input type="radio">` | `role="radiogroup"` (contenedor), `aria-checked`, `aria-label` |
| Modal / Dialog | `role="dialog"` | `aria-modal="true"`, `aria-labelledby`, `aria-describedby` |
| Alert / Toast | `role="alert"` o `role="status"` | `aria-live="assertive"` (alert) / `aria-live="polite"` (status) |
| Navegación | `<nav>` | `aria-label` (si hay varias navs) |
| Lista | `<ul>` / `<ol>` | — |
| Item de lista | `<li>` | — |
| Tab panel | `role="tablist"` / `role="tab"` / `role="tabpanel"` | `aria-selected`, `aria-controls`, `aria-labelledby` |
| Accordion | `<button>` en header | `aria-expanded`, `aria-controls` |
| Tooltip | `role="tooltip"` | `aria-describedby` en el trigger |
| Breadcrumb | `<nav aria-label="breadcrumb">` | `aria-current="page"` en el último item |
| Badge / Tag | `<span>` | `aria-label` si solo visual |
| Icono decorativo | `<svg aria-hidden="true">` | `aria-hidden="true"` |

### Valores de los callout text
- El texto del badge debe ser el **valor exacto del atributo**, por ejemplo:
  - `role="group"` 
  - `aria-label="Dígito 1 de 6"`
  - `aria-live="polite"`
  - `aria-labelledby="id-label id-hint"`
  - `p` (para indicar elemento `<p>`)
  - `dt` (para indicar elemento de definición)

---

## Frame 2: ORDEN DE LECTURA

### Propósito
Mostrar el orden en que un lector de pantalla recorre los elementos del componente. Organiza los elementos en bloques lógicos secuenciales.

### Qué incluir
1. **Instancia del componente** (estado Default)
2. **Overlays/bloques numerados** — rectángulos translúcidos o contornos con números (1, 2, 3...) posicionados sobre cada zona del componente que será leída
3. **Callouts `dl`/`dt`/`dd`** — mostrando el orden y agrupación

### Convención visual
- Cada bloque de lectura se anota con un número o descripción de qué se lee:
  - `1` → Label del componente
  - `2` → Campo/Input principal  
  - `3` → Helper text / caption
  - `4` → Error message (si hay)
- El orden sigue la secuencia DOM natural: de arriba a abajo, de izquierda a derecha
- Para componentes OTP con 6 dígitos: cada dígito individual es un punto de lectura (`Dígito 1 de 6`, `Dígito 2 de 6`...)
- Los mensajes `aria-live` (contadores, errores) se anotan indicando cuándo se leen (asíncrono)

### Reglas por tipo de componente
| Componente | Orden de lectura |
|---|---|
| Input texto | Label → Input (con placeholder) → Helper text → Error |
| Input OTP | Label/instrucción → Dígito 1 → Dígito 2 → ... → Dígito N → Caption/Error |
| Botón | Texto del botón (+ "botón" anunciado por el AT) |
| Checkbox | Checkbox (estado) + Label texto |
| Select | Label → Trigger → [Opciones cuando abierto] |
| Modal | Heading del modal → Contenido → Acciones |
| Alert | Mensaje completo (leído inmediatamente por aria-live) |

---

## Frame 3: ORDEN DE FOCO

### Propósito
Mostrar el orden en que el foco del teclado recorre los elementos interactivos del componente al usar Tab / Shift+Tab.

### Qué incluir
1. **Instancia del componente** (estado Default o con foco visible)
2. **Numeración del orden de foco** — badges numerados (1, 2, 3...) indicando qué elemento recibe foco primero, segundo, etc.
3. **Indicador visual de foco** — mostrar el anillo de foco (outline azul de 3px) sobre el elemento que tiene foco en ese momento
4. **Callout de tecla** — opcionalmente indicar qué tecla activa o interactúa con ese elemento

### Convención visual
- Solo los **elementos interactivos** aparecen en el orden de foco (no los textos estáticos ni labels no focusables)
- Números en badges circulares o cuadrados ordenados secuencialmente
- El anillo de foco estándar del DS es: outline de 3px, color `#2B7FFF` (azul), offset 2px

### Reglas por tipo de componente
| Componente | Elementos focusables | Orden |
|---|---|---|
| Input texto | El propio input | 1 |
| Input OTP | Cada input dígito | 1 → 2 → ... → N |
| Botón | El propio botón | 1 |
| Checkbox | El propio checkbox | 1 |
| Select cerrado | El trigger | 1 |
| Select abierto | Trigger → Opciones | 1 → 2 → ... |
| Modal | [Primer elemento interactivo] → ... → [Botón cerrar] → [Loop al inicio] | 1 → 2 → ... → trap focus |
| Accordion | Header/botón de cada item | 1 → 2 → ... |
| Tab | Cada tab item | 1 → 2 → ... (luego Tab va al tabpanel) |

---

## Proceso paso a paso para generar los frames

### 1. Obtener datos del componente
```
1. Leer el nodeId del componente objetivo en Figma
2. Obtener su estructura con figma:get_figma_data
3. Identificar:
   - Tipo de componente (button, input, select, etc.)
   - Sub-elementos y su jerarquía
   - Variantes disponibles
   - Posición (x, y) del frame principal para posicionar los nuevos frames debajo
```

### 2. Crear los frames en Figma via API REST
Usar el endpoint POST de la Figma API para crear nodos. Los frames de accesibilidad se añaden como **children del mismo parent frame** que el componente original, posicionados debajo.

#### Posición de los frames
```
componentY + componentHeight + 100  → Y de inicio de los frames de accesibilidad
componentX                          → X del primer frame (Roles)
componentX + frameWidth + 128       → X del segundo frame (Orden lectura)
componentX + (frameWidth + 128) * 2 → X del tercer frame (Orden foco)
```

### 3. Estructura JSON para crear un frame vía Figma API

```json
{
  "type": "FRAME",
  "name": "Roles",
  "x": [calculado],
  "y": [calculado],
  "width": 844,
  "height": 600,
  "fills": [{"type": "SOLID", "color": {"r": 1, "g": 1, "b": 1, "a": 1}}],
  "children": [
    {
      "type": "FRAME",
      "name": "Name",
      "fills": [{"type": "SOLID", "color": {"r": 1, "g": 1, "b": 1, "a": 1}}],
      "cornerRadius": 24,
      "children": [
        {
          "type": "TEXT",
          "characters": "Roles",
          "style": {"fontFamily": "Inter", "fontWeight": 700, "fontSize": 64}
        }
      ]
    },
    {
      "type": "FRAME",
      "name": "Anatomy",
      "fills": [{"type": "SOLID", "color": {"r": 1, "g": 1, "b": 1, "a": 1}}],
      "cornerRadius": 24,
      "children": [
        // Instancia del componente
        // Callouts/badges posicionados en absolute
      ]
    }
  ]
}
```

### 4. Instanciar el componente dentro del frame Anatomy
```json
{
  "type": "INSTANCE",
  "componentId": "[id del componente a documentar]",
  "x": 80,
  "y": 80
}
```

### 5. Añadir callouts/badges

Para cada anotación, instanciar el componente correcto del kit:

**Badge de rol HTML (elemento como `dt`, `p`, etc.)**:
```json
{
  "type": "INSTANCE",
  "componentId": "[id de la variante correcta de 'elements']",
  "x": [posición relativa al elemento anotado],
  "y": [posición relativa al elemento anotado]
}
```

**Badge `role=`**:
```json
{
  "type": "INSTANCE", 
  "componentId": "16425:3480",
  "x": [x],
  "y": [y],
  "componentProperties": {
    "role": {"value": "group", "type": "VARIANT"}
  }
}
```

**Badge `aria-*`**:
```json
{
  "type": "INSTANCE",
  "componentId": "16425:3490",
  "x": [x],
  "y": [y],
  "componentProperties": {
    "Link target": {"value": "aria-label", "type": "VARIANT"}
  }
}
```

---

## Naming convention de los frames

```
[NombreComponente] / Accesibilidad / Roles
[NombreComponente] / Accesibilidad / Orden de lectura
[NombreComponente] / Accesibilidad / Orden de foco
```

Ejemplos:
- `R4InputOTP / Accesibilidad / Roles`
- `R4Button / Accesibilidad / Orden de foco`
- `R4Select / Accesibilidad / Orden de lectura`

---

## Ejemplo concreto: Input OTP

### Roles frame — anotaciones aplicadas
El Input OTP (`R4InputOTP`, node `16425:3210`) tiene:

```
Estructura HTML semántica:
<section aria-labelledby="id-otp-label">     ← p + aria-labelledby="id1 id2"
  <p id="id-otp-label">                      ← Texto instrucción (aria-live="polite" si hay countdown)
    El código enviado al teléfono...
  </p>
  <div role="group"                           ← role="group"
       aria-label="Código OTP">
    <input aria-label="Dígito 1 de 6" />     ← aria-label="Dígito X de 6" × 6
    <input aria-label="Dígito 2 de 6" />
    ...
    <input aria-label="Dígito 6 de 6" />
  </div>
  <span aria-live="polite">                  ← aria-live="polite" (countdown)
    Caduca en 1:30 minutos
  </span>
</section>
```

Callouts visibles en el frame Roles:
1. Badge `p` apuntando al texto de instrucción (flecha izquierda)
2. Badge `role="group"` apuntando al contenedor de inputs (flecha izquierda)  
3. Badge `aria-label="Dígito X de 6"` apuntando a cualquiera de los inputs (flecha arriba)
4. Badge `aria-labelledby="id1 id2"` apuntando al input group (flecha derecha)
5. Badge `aria-live="polite"` apuntando al caption de countdown (flecha arriba)
6. Badge `aria-live="assertive"` apuntando al caption de error (flecha arriba)

### Orden de lectura — secuencia
```
1. "El código enviado al teléfono *****5151 caduca a los 2 minutos."
2. "Dígito 1 de 6" [input]
3. "Dígito 2 de 6" [input]
4. "Dígito 3 de 6" [input]
5. "Dígito 4 de 6" [input]
6. "Dígito 5 de 6" [input]
7. "Dígito 6 de 6" [input]
8. [Si countdown activo] "Caduca en X:XX minutos" (aria-live, leído asíncronamente)
9. [Si error] "Código incorrecto o caducado" (aria-live="assertive", interrupción inmediata)
```

### Orden de foco — elementos focusables
```
Foco 1 → Input dígito 1 (Tab)
Foco 2 → Input dígito 2 (Tab o auto-avance al escribir)
Foco 3 → Input dígito 3
Foco 4 → Input dígito 4
Foco 5 → Input dígito 5
Foco 6 → Input dígito 6
[Botón "Reenviar código" si visible]
```

---

## Colores y estilos visuales del kit

| Token | Color hex | Uso |
|---|---|---|
| `Soporte/alert/base` | `#FA9902` | Badge naranja para `dt` (término) |
| `Soporte/alert/light` | ~`#FEE685` | Badge amarillo claro para `dd` (descripción) |
| `Soporte/info/base` | `#FA9902` | Badge naranja para `p` (párrafo) |
| `Soporte/info/dark` | `#C97A00` | Badge marrón-naranja para `role=` |
| `Gráficas/2` | `#AA3C2B` | Badge rojo-marrón para `aria-*` |
| `Fondo/white` | `#FFFFFF` | Texto sobre badges oscuros |
| `Contenido/black` | `#1F2331` | Texto sobre badges claros |
| `#2B7FFF` | azul | Anillo de foco (3px outline) |

Tipografía de badges:
- fontFamily: `Open Sans`
- fontWeight: `700`
- fontSize: `14`
- lineHeight: `1.43em`

---

## Checklist de calidad antes de entregar

- [ ] Los 3 frames están presentes: Roles, Orden de lectura, Orden de foco
- [ ] Cada frame tiene el nombre correcto con la convención `[Componente] / Accesibilidad / [Tipo]`
- [ ] Los callouts usan los componentes correctos del kit de anotaciones
- [ ] Los valores de `aria-label` son descriptivos y específicos (no genéricos)
- [ ] El orden de lectura es coherente con la estructura DOM del componente
- [ ] Solo los elementos interactivos aparecen en el orden de foco
- [ ] Se han incluido los estados relevantes (Default + Error mínimo)
- [ ] Los frames están posicionados debajo/al lado del componente original, no encima
- [ ] Los textos de los badges son en minúsculas cuando corresponde (ej: `role="group"` no `Role="Group"`)
