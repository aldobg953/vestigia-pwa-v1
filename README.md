# Vestigia · Presentación con scroll (v1)

Página de presentación para la PWA: el faro gira con el scroll, el título gigante queda detrás,
los textos cuentan la premisa sin revelar el final y, a mitad del recorrido, la escena se
convierte en apuntes de lápiz y acuarela sobre papel viejo.

## Cómo verla en local

El navegador no puede cargar el modelo con `file://`, así que necesita un servidor local.
Desde la raíz del repositorio:

```bash
npx serve .
# o bien
python -m http.server 5173
```

Abre la dirección que te muestre (por ejemplo http://localhost:5173).

## Modelo 3D del faro

La página intenta cargar `assets/lighthouse/Lighthouse_v1_Unlit.gltf`.
Las texturas ya están optimizadas ahí (2048 px, WebP/JPG, ~1.6 MB en lugar de ~38 MB).

**Falta el archivo de geometría `Lighthouse_v1_Unlit.bin`.** No venía en
`Assets/[GLTF] Lighthouse_v1_Unlit`. Cópialo a `assets/lighthouse/` con ese mismo nombre.
Mientras no esté, la página usa un faro procedural con la misma silueta (en consola aparece
el aviso `[Vestigia] Modelo GLTF no disponible`). Cuando lo agregues, se carga solo, se
normaliza a la altura de la escena y el haz de luz se coloca en la malla `Bulbs_low`.

## Línea de tiempo (0 → 1 del scroll)

| Tramo | Qué pasa |
|---|---|
| 0.00 – 0.07 | Portada: título gigante detrás del faro |
| 0.075 – 0.195 | Cap. 1 · La tormenta |
| 0.20 – 0.315 | Cap. 2 · Janeth |
| 0.32 – 0.44 | Cap. 3 · El faro (interferencia de radio en pantalla) |
| 0.445 – 0.555 | La noche se convierte en libreta (lápiz → acuarela) |
| 0.555 – 0.785 | Cap. 4 y 5 en papel, con notas a mano que se dibujan solas |
| 0.79 – 0.87 | Regreso a la noche |
| 0.875 – 1.00 | Final: el faro y el cielo pulsan a la vez, llamado a la acción |

Todo se ajusta en el bloque `CONFIGURACIÓN` del script (`SKETCH_IN`, `SKETCH_OUT`, `GLITCH`,
`CAM_KEYS`) y en los atributos `data-a` / `data-b` de cada capítulo.

## Cómo está hecho

- **Three.js 0.160** (módulos desde jsDelivr). Para la PWA con Vite: `npm i three` y cambiar el
  `importmap` por imports normales.
- **Tres pases**: noche (HDR, cielo, mar, lluvia, haz), día (albedo limpio) y normales + profundidad.
  Solo se renderiza lo necesario según el tramo.
- **Shader de composición**: contornos de grafito (normales, profundidad y luminancia) con
  temblor de mano, sombreado con trama cruzada, acuarela con sangrado, granulación y pigmento
  acumulado en los bordes, papel café horneado una vez (cuadrícula de libreta, aro de café,
  fibras, doblez central, bordes envejecidos). El revelado avanza con un frente húmedo:
  primero aparece el papel, luego el trazo y al final el color.
- **Sonido opcional** sintetizado con Web Audio (lluvia, mar, estática y truenos), sin archivos.
- Respeta `prefers-reduced-motion` (sin inercia, sin relámpagos ni temblor de línea).

## Pendiente

- Archivo `.bin` del faro.
- Destino real del botón "Abrir la libreta" (ahora no navega).
- Copys definitivos y revisión de spoilers con el equipo.
