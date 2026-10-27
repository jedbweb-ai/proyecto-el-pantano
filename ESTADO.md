# El Pantano — Estado del proyecto

Última actualización: 26 de octubre de 2026.
Juego publicado en: https://jedbweb-ai.github.io/proyecto-el-pantano/

Todo el juego vive en `el-pantano-capitulo-1.html`. Las ilustraciones van en `img/` y el audio en `audio/`; cada carpeta tiene su propio README.

---

## Lo que está hecho

### Ilustraciones por escena
- Cada escena tiene un espacio para una imagen arriba de la card. La imagen hace un zoom lento y, al cambiar de escena, se funde con la siguiente.
- Encima van efectos animados: niebla, resplandor, esporas, luciérnagas, ondas, sombra, llama y bruma.
- Todo se configura en `SCENE_ART`.
- Mientras falta una imagen se ve un fondo provisorio con la leyenda «Ilustración pendiente».
- Los prompts para generar las 8 imágenes están en `img/README.md`.

### Inventario: la silueta
- Un botón con una silueta, arriba a la derecha, que solo aparece durante el juego. Cuando hay algo nuevo se enciende un punto dorado.
- El panel muestra un maniquí sin rostro con los objetos dibujados donde los llevás.
- Lugares del cuerpo, cada uno con capacidad limitada: cabeza, cuello, abrigo, mano izquierda, mano derecha, cinturón, piernas, dos bolsillos, espalda y pies.
- Si llevás una mochila en la espalda, aparece la sección "Adentro de la mochila", con 4 lugares.
- Desde el panel se puede pasar un objeto a otro lugar libre o soltarlo.
- **Al agarrar un objeto:**
  - Si tiene varios lugares posibles, el juego pregunta «¿Dónde llevás…?».
  - Si tiene un solo lugar y está libre, se lo pone directo y avisa, por ejemplo «Te pusiste el pantalón viejo».
  - Si el lugar está ocupado, avisa qué tenés que soltar.
- Lo que soltás queda en esa escena, con la opción "Recoger…".
- Al terminar el capítulo, el inventario queda para mirar, sin acciones.

### Objetos definidos (`ITEMS`)
| Objeto | Dónde puede ir | ¿Está en la historia? |
|---|---|---|
| Amuleto partido | Mano izquierda, mano derecha o cuello | Sí: "Guardarte el amuleto" |
| Pantalón viejo | Solo piernas (se pone directo) | Sí: tirado en el pantano, con "Recoger el pantalón viejo" abajo de las opciones |
| Espada rota | Mano o cinturón | No, solo en modo prueba |
| Mochila de cuero | Espalda (4 lugares adentro) | No, solo en modo prueba |

### Tipos de decisión (`CHOICE_TYPES`)
- **Normal:** verde con →.
- **Objeto ◆ (ámbar):** agrega algo al inventario. Se usa en las opciones con `take` y en "Recoger…".
- **Recuerdo ✦ (azul pálido):** deja un fragmento de memoria. Se usa en las opciones con `memory`.
- **Varios tipos a la vez:** el borde y los íconos pasan en degradado.
- **Para sumar un tipo nuevo:** se agrega una línea en `CHOICE_TYPES`, con su color y su ícono, y se marca en las opciones con `types: ["nombre"]`.

### Fragmentos de memoria
- Se ganan solo al **elegir** una opción con `memory`. No hay un total fijo. Tienen que salir de decisiones importantes: las tres primeras opciones (bosque, resplandor, canto) no dan ninguno.
- Al ganar uno aparece un aviso flotante arriba («✦ Fragmento de memoria · nombre»), que se va solo; si lo tocás, abre el inventario. No queda escrito dentro de la historia.
- En el inventario se listan como «Fragmentos de memoria · N».
- Los actuales:

| Fragmento | Opción que lo da |
|---|---|
| Lo que dejaste ir | Dejarlo hundirse en el barro |

### Herramientas para escribir la historia (objeto `scenes`)
- `take: "id"`: la opción agarra un objeto.
- `memory: "Nombre"`: la opción da un fragmento.
- `when: () => …`: la opción o el párrafo solo aparece si se cumple la condición. Por ejemplo, `inv.isIn("amuleto", "cuello")`.
- `requires: { test, reason }`: la opción se ve, pero queda bloqueada y muestra el motivo.
- `types: [...]`: tipos de decisión extra.
- `items: ["id"]` (en la escena): objetos tirados en el suelo desde el principio, con la opción "Recoger…" abajo. El jugador se queda en la misma escena al recogerlos.
- Ejemplo ya en uso: el texto de `pantano_a` cambia según si el amuleto va en la mano o en el cuello.

### Otros
- **Ventana al comenzar:** «Este viaje no es apto para personas sensibles. ¿Estás seguro de empezar?».
- **Transición de capítulo:** se puede saltear con un clic, Enter o Espacio. Cada capítulo muestra una frase fija: la del Capítulo 1 es de Sartre; la del Capítulo 2 (`CHAPTER_2_SUBTITLE`) es «La única manera de lidiar con un mundo sin libertad…».
- **Bugs corregidos:**
  - el fundido del texto no se repetía entre escenas;
  - el doble clic disparaba dos veces la transición;
  - la música del menú no arrancaba;
  - el crossfade de música no se cancelaba si cambiaba la pista a mitad;
  - se perdía el foco al navegar con teclado.
- **Optimizaciones:** la música del capítulo se precarga durante la transición, los efectos de sonido se cargan una sola vez y las imágenes de las escenas siguientes se precargan.
- **Instrucciones:** explican el inventario y los íconos ◆ y ✦.
- **Modo prueba:** agregando `?objetos=pantalon_viejo,espada_rota,mochila` a la URL, esos objetos aparecen en el suelo de la primera escena.

---

## Pendientes

### Historia (lo próximo)
- [ ] Revisar y modificar textos, recorridos y opciones de la historia.
- [ ] Definir qué decisiones importantes dan fragmentos de memoria y cómo se llaman. Hoy solo queda «Lo que dejaste ir» (provisorio).
- [ ] Definir los nuevos tipos de decisión y sus colores.
- [ ] Decidir dónde aparecen en la historia la espada rota y la mochila. Recordar: **los lugares de cada objeto los define Julián**.
- [ ] Revisar el texto de borrador de `pantano_a` cuando el amuleto va al cuello: «Del amuleto todavía cuelga un resto de cordón de cuero…».
- [ ] La variante del amuleto en la mano dice «peso extra sobre el pecho — no solo el del amuleto», pero en ese caso el amuleto está en el puño.
- [ ] La encrucijada dice «El canto que escuchaste (o no)»: se podría adaptar según el camino que hizo el jugador.
- [ ] Definir el contenido real del Capítulo II.

### Diseño
- [ ] Ubicación del aviso flotante de fragmento: por ahora está arriba, sobre el título; probablemente se cambie.
- [ ] Actualizar las instrucciones cuando se sumen tipos de decisión nuevos.

### Ilustraciones
- [ ] Generar las 8 imágenes con los prompts de `img/README.md`: inicio, bosque, bosque-sombra, pantano-amuleto, pantano, canto, canto-ojos y encrucijada.
- [ ] Después, ajustar la posición de los efectos en `SCENE_ART` para que coincidan con cada imagen.

### Audio
- [ ] Reconvertir la música a 128 kbps (hoy está a 320 kbps; la del Capítulo I pesa 7,6 MB). Hace falta ffmpeg o una herramienta online.
- [ ] El sonido de fragmento (`SFX_TRACKS.fragment`) está descargado pero desactivado.

### Ideas para más adelante
- [ ] Guardar el progreso del jugador.
- [ ] Pasar cada capítulo a su propio archivo cuando haya más de uno.
