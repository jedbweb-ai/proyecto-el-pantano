# Carpeta de audio de El Pantano

Colocá acá los archivos con estos nombres exactos (tal cual se descargan, sin renombrar). Si un archivo falta, esa pista puntual queda en silencio pero el juego sigue funcionando normalmente.

## Música (loop de fondo)

| Archivo | Cuándo suena | Estado |
|---|---|---|
| `atlasaudio-dark-ambient-590151.mp3` | Menú principal e instrucciones | ✅ Descargado |
| `phantomxcutzz-swamp-tension-suspense-thriller-background-music-351808.mp3` | Todo el Capítulo I (una sola pista por ahora, sin variar todavía según escena o elección) | ✅ Descargado |

## Efectos de sonido

| Archivo | Cuándo suena | Estado |
|---|---|---|
| `freesound_community-evil-demonic-laugh-6925.mp3` | Botón "Comenzar tu viaje" | ✅ Descargado |
| `juniorsoundays-ui-sound-22-527807.mp3` | Botones "Instrucciones", "Volver al menú" y cada elección de la historia (incluye "Volver a empezar") | ✅ Descargado |
| `soundshelfstudio-deep-ui-chime-585839.mp3` | Al recuperar un fragmento de memoria (El Nombre, El Símbolo, La Voz) | ✅ Descargado, ⏸️ desactivado por ahora en el código |

Más adelante vamos a sumar pistas específicas para escenas y elecciones puntuales del Capítulo I. Eso se define aparte y no hace falta anticiparlo todavía.

## Notas

- Formato: `.mp3`.
- Las dos pistas de música deben poder loopearse sin corte audible. El cambio entre ambas hace un crossfade automático de ~1.4s.
- Los efectos son cortos y no loopean: se superponen a la música de fondo en el momento correspondiente.
- Este README no afecta al juego, es solo una guía.
