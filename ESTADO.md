# El Pantano: estado del proyecto

Última actualización: 28 de octubre de 2026.
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
| Pantalón viejo | Solo piernas (se pone directo) | Sí: tirado en la escena de inicio, con "Recoger el pantalón viejo" junto a las tres primeras opciones (bosque, resplandor, canto) |
| Espada incandescente | Mano izquierda o mano derecha | Sí: apoyada en la puerta de la cabaña, con «Tomar la espada» |
| Madera astillada | Mano izquierda o mano derecha | Sí: al asesinar a los goblins sin la espada, queda sola en una mano libre |
| Espada manchada | Mano izquierda o mano derecha | Sí: la espada incandescente se transforma en esta al asesinar a los goblins con ella |
| Mochila de cuero | Espalda (4 lugares adentro) | No, solo en modo prueba |

### Tipos de decisión (`CHOICE_TYPES`)
- **Normal:** verde con →.
- **Objeto ◆:** agrega algo al inventario. Se usa en las opciones con `take` (como "Guardarte el amuleto") y en "Recoger…": las dos se ven exactamente igual. No se inventa otro color para recoger objetos.
- **Recuerdo ✦ (azul pálido):** deja un fragmento de memoria. Se usa en las opciones con `memory`.
- **Determinante ✷ (rojo):** decisiones que pesan más. También dejan un fragmento, pero se ven solo en rojo, sin el degradado con el azul. Se marcan con `types: ["determinante"]` y `memory`. Las instrucciones del juego ya lo explican.
- **Varios tipos a la vez:** el borde y los íconos pasan en degradado.
- **Para sumar un tipo nuevo:** se agrega una línea en `CHOICE_TYPES`, con su color y su ícono, y se marca en las opciones con `types: ["nombre"]`.

### Fragmentos de memoria
- Se ganan solo al **elegir** una opción con `memory`. No hay un total fijo. Tienen que salir de decisiones importantes: las tres primeras opciones (bosque, resplandor, canto) no dan ninguno.
- Al ganar uno aparece un aviso arriba de las opciones de la escena siguiente («✦ Fragmento de memoria: nombre.»), en azul pálido. Va en el mismo lugar y con el mismo formato que el aviso de objeto («◆ Te pusiste el pantalón viejo.»). Ya no hay aviso flotante.
- En el inventario se listan como «Fragmentos de memoria · N».
- Los actuales:

| Fragmento | Opción que lo da |
|---|---|
| Lo que dejaste ir | Dejarlo hundirse en el barro |
| Infanticida armado | Asesinar a las criaturas, con la espada incandescente en la mano |
| Infanticida monstruoso | Asesinar a las criaturas, sin la espada (con una madera del piso) |
| Héroe de los goblins | Irte y dejarlas vivas |

### Herramientas para escribir la historia (objeto `scenes`)
- `take: "id"`: la opción agarra un objeto.
- `memory: "Nombre"`: la opción da un fragmento.
- `when: () => …`: la opción o el párrafo solo aparece si se cumple la condición. Por ejemplo, `inv.isIn("amuleto", "cuello")`.
- `requires: { test, reason }`: la opción se ve, pero queda bloqueada y muestra el motivo.
- `types: [...]`: tipos de decisión extra.
- `items: ["id"]` (en la escena): objetos tirados en el suelo desde el principio, con la opción "Recoger…" abajo. El jugador se queda en la misma escena al recogerlos y el texto se actualiza. Con `{ id, label }` la opción lleva otro texto (por ejemplo «Tomar la espada»).
- `inv.liesIn("objeto", "escena")`: verdadero si el objeto está en el suelo de esa escena. Sirve para cambiar el texto cuando ya se lo llevaron.
- `effect: () => …`: la opción cambia algo al elegirla. Por ejemplo, `giveItem("madera")` pone un objeto en el primer lugar libre sin preguntar, y `swapItem("espada_incandescente", "espada_manchada", aviso)` transforma un objeto en otro en el mismo lugar.
- `visited.has("escena")`: verdadero si el jugador ya salió alguna vez de esa escena. Sirve para mostrar otro texto al volver a un lugar. Se usa en `inicio` y `bosque`.
- Ejemplo ya en uso: el texto de `pantano_a` cambia según si el amuleto va en la mano o en el cuello.

### Rama del bosque (en reescritura)
- `cabana`: al acercarte crujen las maderas, se oyen chillidos de algo chico adentro y brilla una espada apoyada en la puerta. Opciones: «Tomar la espada» (te quedás en la escena), «Entrar a la cabaña» (`cabana_adentro`) y «Continuar el camino de árboles tallados» (`bosque_camino`). Al tomar la espada el texto cambia («La puerta quedó vacía…»).
- `cabana_adentro`: tres goblins bebés que chillan. Dos opciones determinantes (rojas): «Asesinar a las criaturas» e «Irte y dejarlas vivas».
  - **Con la espada en la mano:** `cabana_matanza_espada`. La espada se mancha (pasa a ser «Espada manchada») y da «Infanticida armado».
  - **Sin la espada:** `cabana_matanza_madera`. Matás a los goblins con una madera del piso y da «Infanticida monstruoso». La madera queda en una mano libre, y al salir todavía podés tomar la espada con la otra.
  - Las dos matanzas tienen un solo botón, «Salir de la cabaña», que vuelve afuera. Si la espada sigue en la puerta, se puede tomar.
  - **Irte y dejarlas vivas:** da «Héroe de los goblins» y vuelve afuera, con los chillidos detrás de la puerta.
  - Después de entrar, afuera ya no aparece «Entrar a la cabaña» y el texto cambia según lo que hiciste.
- Se sacó la espada rota: ya no está en la encrucijada ni en los objetos.
- `bosque`: el camino de árboles tallados lleva a una cabaña de pantano medio derrumbada, con ruidos que vienen de adentro. Ya no está la piedra con musgo, y se borraron `bosque_a` y `bosque_b`.
- Opciones: «Acercarte a la cabaña» (`cabana`), «Volver hacia atrás, al pantano» (`inicio`) y «Continuar el camino de árboles tallados» (`bosque_camino`).
- **Al volver al inicio** desde el bosque se ve otro texto («Desandás el camino de árboles marcados…»), con las mismas tres opciones. El pantalón sigue tirado ahí si no se recogió.
- **Al volver a elegir el bosque**, la opción dice «Volver al bosque de troncos tallados» y lleva directo a la cabaña, con un texto corto.

### Estilo de los textos
- Se sacaron todos los guiones largos (—) del juego y de los README. Las frases se reescribieron con punto, coma o dos puntos.
- De acá en adelante, lo que dicta Julián se adapta al estilo del juego y se pasa por el skill The Humanizer (reglas en `CLAUDE.md`).

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
- **Modo prueba:** agregando `?objetos=pantalon_viejo,mochila` a la URL, esos objetos aparecen en el suelo de la primera escena.

---

## Pendientes

### Historia (lo próximo)
- [ ] Revisar y modificar textos, recorridos y opciones de la historia. Se está haciendo rama por rama, empezando por el bosque.
- [x] Escribir `cabana` («Acercarte a la cabaña»).
- [x] Escribir `cabana_adentro` («Entrar a la cabaña») con los tres goblins bebés y sus dos decisiones determinantes.
- [ ] Escribir `bosque_camino` («Continuar el camino de árboles tallados»). Hoy tiene un texto provisorio que lleva a la encrucijada.
- [ ] Definir qué decisiones importantes dan fragmentos de memoria y cómo se llaman. Hoy solo queda «Lo que dejaste ir» (provisorio).
- [ ] Definir los nuevos tipos de decisión y sus colores.
- [ ] Decidir dónde aparece en la historia la mochila. Recordar: **los lugares de cada objeto los define Julián**.
- [ ] Revisar el texto de borrador de `pantano_a` cuando el amuleto va al cuello: «Del amuleto todavía cuelga un resto de cordón de cuero…».
- [ ] La variante del amuleto en la mano dice «peso extra sobre el pecho, y no es solo el del amuleto», pero en ese caso el amuleto está en el puño.
- [ ] La encrucijada dice «El canto que escuchaste (o no)»: se podría adaptar según el camino que hizo el jugador.
- [ ] Definir el contenido real del Capítulo II.

### Diseño
- [x] Ubicación del aviso de fragmento: ahora va arriba de las opciones, igual que el de objeto.
- [ ] Actualizar las instrucciones cuando se sumen tipos de decisión nuevos.

### Ilustraciones
- [ ] Generar las 8 imágenes con los prompts de `img/README.md`: inicio, bosque, bosque-sombra, pantano-amuleto, pantano, canto, canto-ojos y encrucijada.
- [ ] Después, ajustar la posición de los efectos en `SCENE_ART` para que coincidan con cada imagen.
- [ ] Rehacer los prompts de `img/README.md` para la rama nueva del bosque: `bosque.webp` ahora es la cabaña (no la piedra), y `bosque-sombra.webp` ya no se usa. Faltan las imágenes de `cabana` y `bosque_camino`.

### Audio
- [ ] **Sonidos a buscar** (se agregan todos juntos al final). Escena u opción, y qué sonido:
  - Al agarrar cualquier objeto: sonido de equiparse, como en los juegos de RPG.
  - `cabana` (al acercarte): maderas que crujen.
  - `cabana` (al acercarte): chillidos de un monstruo chico y grotesco (los goblins bebés).
  - `cabana_matanza_madera` (asesinar con la madera): golpes fuertes.
  - `cabana_matanza_espada` (asesinar con la espada): espada que corta carne.
  - `cabana_matanza_madera` y `cabana_matanza_espada`: un monstruo chico que agoniza.
- [ ] Reconvertir la música a 128 kbps (hoy está a 320 kbps; la del Capítulo I pesa 7,6 MB). Hace falta ffmpeg o una herramienta online.
- [ ] El sonido de fragmento (`SFX_TRACKS.fragment`) está descargado pero desactivado.

### Ideas para más adelante
- [ ] Guardar el progreso del jugador.
- [ ] Pasar cada capítulo a su propio archivo cuando haya más de uno.
