# Carpeta de imágenes de El Pantano

Colocá acá las ilustraciones con estos nombres exactos. Si una falta, la escena muestra un fondo provisorio con la leyenda «Ilustración pendiente», y los efectos animados (niebla, brillos, partículas) siguen funcionando encima. Nada se rompe.

Son **8 imágenes para las 11 escenas**: algunas escenas reusan la misma imagen con otro encuadre o con otros efectos.

| Archivo | Escenas | Qué muestra | Efectos encima |
|---|---|---|---|
| `inicio.webp` | inicio | Figura hundida en el lodo, tres caminos en la niebla | niebla + resplandor en el horizonte |
| `bosque.webp` | bosque, bosque_b | Claro con troncos tallados y piedra con musgo | niebla + esporas + luz desde arriba (en bosque_b: zoom hacia la derecha) |
| `bosque-sombra.webp` | bosque_a | El mismo bosque, con una silueta entre los árboles | niebla + sombra que cruza |
| `pantano-amuleto.webp` | pantano | Mano que saca el amuleto partido del agua | niebla + brillo de fondo + ondas |
| `pantano.webp` | pantano_a, pantano_b | Vista amplia del pantano con el resplandor | pantano_a: brillo fuerte y rápido · pantano_b: ondas donde se hundió |
| `canto.webp` | canto, canto_b | Figura pálida cantando sobre la roca con flores | niebla + luciérnagas (en canto: zoom sobre la figura; en canto_b: vista completa y más bruma) |
| `canto-ojos.webp` | canto_a | Primer plano de los ojos abiertos | niebla + bruma que se cierra |
| `encrucijada.webp` | encrucijada | Claro de piedra, vela que flota | niebla + llama que parpadea |

## Formato

- **Tamaño:** 1600 × 900 px (16:9).
- **Formato:** WebP, calidad 75–80. Lo ideal es que cada imagen pese menos de 200 KB. Los generadores suelen entregar PNG o JPG, que podés convertir gratis en [squoosh.app](https://squoosh.app).
- Si preferís usar `.jpg`, cambiá la extensión en `SCENE_ART`, dentro del HTML.

## Reglas de composición (importante)

- **El cuarto inferior de la imagen se funde con la card**, así que no pongas nada importante abajo del todo.
- **En el celular la imagen se recorta por los costados** (pasa a 3:2), así que lo importante tiene que estar en la franja central.
- Los efectos están ubicados según la composición que piden los prompts. Si la imagen generada quedó distinta, se ajustan sin rehacer la imagen (ver más abajo).

## Cómo mantener el mismo estilo en todas

1. Generá primero `inicio.webp` y elegí la que más te guste.
2. Usala como referencia de estilo para las demás:
   - **Midjourney:** agregá `--sref <URL de la imagen de inicio>` a cada prompt.
   - **ChatGPT / DALL·E:** generalas todas en la misma conversación y agregá «same style as the first image».
   - **Firefly / Ideogram:** cargá la de inicio como *style reference*.
3. Para la figura de `canto` y `canto-ojos`, usá la imagen de `canto` como referencia de personaje (en Midjourney, `--cref`).

## Estilo base

Pegá este bloque **al final de cada prompt**:

```
dark atmospheric painterly illustration, muted palette of deep swamp greens, charcoal black and fog grey with faint antique gold highlights, soft volumetric fog, subtle film grain, textured like an old oil painting mixed with a 19th-century engraving, eerie and melancholic mood, cinematic composition, 16:9, no text, no letters, no watermark, no border
```

En Midjourney sumá además: `--ar 16:9 --style raw`

## Prompts

### 1. `inicio.webp`
```
A lone figure seen from behind, face not visible, dark ragged clothes, standing knee-deep in black swamp mud, placed left of center with head and shoulders at mid-height. Thick fog everywhere. On the left edge, a dense forest of twisted dark trunks with faint carved shapes in the bark. Straight ahead, the swamp stretches to a distant horizon where the water gives off an unnatural pale green-gold glow, just right of center. On the right, tall reeds fading into the mist. Twilight, almost night.
```

### 2. `bosque.webp`
```
A small clearing in an ancient, dense forest. The twisted tree trunks around the edges are carved with spirals, eyes and worn runes. In the center stands a tall stone, as tall as a person, covered in thick moss, with a few weathered illegible engraved marks peeking through. Thin pale shafts of light fall from the canopy onto the stone. Tiny spores floating in the air. On the right side the trees thin out toward a paler, misty opening.
```

### 3. `bosque-sombra.webp`
```
The same dark carved forest, colder and darker. In the left foreground, the tall stone with its moss torn away, revealing scratched illegible engravings. Deep among the trunks on the right, barely visible in the shadows, a tall thin silhouette walking on two legs but not quite like a man: elongated limbs, slightly hunched, no visible face. Menacing and ambiguous, more suggested than shown.
```

### 4. `pantano-amuleto.webp`
```
Close-up at water level in a murky swamp. A muddy human hand emerges from the dark water left of center, holding a blackened metal amulet broken into two halves that still fit together. Small ripples around the wrist. In the blurred background, slightly above center, the water glows with an eerie pale green light through the fog.
```

### 5. `pantano.webp`
```
Wide view of an endless swamp at dusk: black water with floating weeds and dead reeds. In the distance, slightly above the center of the image, the surface of the water glows with an unnatural pale green-gold light, as if something underneath were watching. The water in the foreground is calm, dark and mirror-like. Fog drifting low over the surface. No people.
```

### 6. `canto.webp`
```
Among tall spear-like reeds and swirling fog, a pale ethereal figure sits on a rock covered with delicate faintly glowing flowers that should not grow in a swamp. Long hair tangled with moss, eyes closed, singing with the head slightly raised. The figure is slightly right of center, at mid-distance. A few fireflies around. Beautiful, melancholic and unsettling.
```

### 7. `canto-ojos.webp`
```
Close-up portrait of the same pale figure, face partly hidden by long hair tangled with moss and tiny flowers. The eyes are open and not quite human: the murky green-brown color of stagnant water, looking straight at the viewer with quiet recognition, as if they had been waiting. Fog closing in from the edges. Centered composition.
```

### 8. `encrucijada.webp`
```
A circular clearing where swamp mud gives way to ancient flat stones covered in silvery lichen. Beyond the clearing, right of center, through thick fog, a single small candle flame floats in mid-air with no candle and no one holding it. Dead silence, ominous atmosphere.
```

## Ajustar los efectos a la imagen

Si la imagen quedó con otra composición (por ejemplo, el resplandor salió más a la izquierda), no hace falta rehacerla: se ajusta en `SCENE_ART`, dentro de `el-pantano-capitulo-1.html`.

```js
inicio: {
  src: "inicio.webp",
  fx: ["fog", { type: "glow", x: "60%", y: "46%", size: "26%" }]
}
```

- `x` / `y`: posición del efecto. Se mide desde la esquina superior izquierda: `"0%"` es el borde izquierdo o el de arriba, y `"100%"` el borde derecho o el de abajo.
- `size`: tamaño, relativo al ancho de la imagen.
- `strength`: intensidad (1 = normal).
- `speed`: duración de un ciclo (`"5s"`). Cuanto menor el valor, más rápido.
- `color`: color del brillo, por ejemplo `"rgba(226,232,196,0.35)"`.
- `count`: cantidad de partículas (para esporas y luciérnagas).
- `scale` / `focus` (en la escena, no en el efecto): zoom base y punto de enfoque, para reencuadrar la misma imagen en otra escena.

Efectos disponibles: `fog` (niebla), `glow` (brillo que late), `spores` (esporas), `fireflies` (luciérnagas), `ripples` (ondas en el agua), `shadow` (silueta que cruza), `flame` (llama), `mist` (bruma que se cierra).
