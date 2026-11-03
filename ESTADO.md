# El Pantano: estado del proyecto

Última actualización: 3 de noviembre de 2026.
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
- Lugares del cuerpo, cada uno con capacidad limitada: cabeza, cuello, abrigo, mano izquierda, mano derecha, dedo de la mano izquierda, dedo de la mano derecha (solo dos lugares para anillos), cinturón, piernas, dos bolsillos, espalda y pies.
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
| Casco gastado | Solo cabeza (se pone directo) | Sí: tirado junto a la muralla, a la derecha, con «Ponerte el casco» |
| Anillo del muerto | Dedo de la mano izquierda o dedo de la mano derecha | Sí: en el dedo del cuerpo junto a la muralla, con «Tomar el anillo». Más adelante va a tener un impacto en la historia |
| Mochila de cuero | Espalda (4 lugares adentro) | No, solo en modo prueba |

### Tipos de decisión (`CHOICE_TYPES`)
- **Normal:** verde con →.
- **Objeto ◆:** agrega algo al inventario. Se usa en las opciones con `take` (como "Guardarte el amuleto") y en "Recoger…": las dos se ven exactamente igual. No se inventa otro color para recoger objetos.
- **Recuerdo ✦ (azul pálido):** deja un fragmento de memoria. Se usa en las opciones con `memory`.
- **Determinante ✷ (rojo):** decisiones que pesan más. También dejan un fragmento, pero se ven solo en rojo, sin el degradado con el azul. Se marcan con `types: ["determinante"]` y `memory`. Las instrucciones del juego ya lo explican.
- **Por lo que llevás ⚔ (gris acero):** opciones que solo aparecen gracias a un objeto. Adentro del botón, en letra chica (`note`), dice qué objeto la habilita. Las opciones de objetos que no tenés quedan ocultas. Se marcan con `types: ["llevas"]`. Las instrucciones del juego ya lo explican.
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
| La madre del pantano (provisorio) | Hacerle frente con la espada (matar a la criatura de la muralla) |

### Herramientas para escribir la historia (objeto `scenes`)
- `take: "id"`: la opción agarra un objeto.
- `memory: "Nombre"`: la opción da un fragmento.
- `when: () => …`: la opción o el párrafo solo aparece si se cumple la condición. Por ejemplo, `inv.isIn("amuleto", "cuello")`.
- `requires: { test, reason }`: la opción se ve, pero queda bloqueada y muestra el motivo.
- `note: "…"`: un aviso chico dentro de la opción, en itálica, sin bloquearla. Se usa en «Continuar acercándote» («⚔ Lo que hiciste con la espada va a cambiar lo que pase.»).
- `types: [...]`: tipos de decisión extra.
- `items: ["id"]` (en la escena): objetos tirados en el suelo desde el principio, con la opción "Recoger…" abajo. El jugador se queda en la misma escena al recogerlos y el texto se actualiza. Con `{ id, label }` la opción lleva otro texto (por ejemplo «Tomar la espada»).
- `inv.liesIn("objeto", "escena")`: verdadero si el objeto está en el suelo de esa escena. Sirve para cambiar el texto cuando ya se lo llevaron.
- `isDeath: true` (en la escena): el jugador muere. Se ve «☠ Moriste.» y el botón «Volver dos decisiones atrás». Antes de cada decisión el juego guarda una copia de todo (escena, objetos, fragmentos, visitas, la criatura) en `history`, y al morir vuelve a la de dos decisiones antes. Las opciones que matan no se marcan. Las instrucciones lo explican.
- `creature.mode()` y `creature.outcome`: cómo reacciona la criatura de la muralla y cómo terminó el encuentro.
- `cameFrom`: la escena de la que viene el jugador. Sirve para cambiar el texto según por dónde llega (se usa en `cabana`).
- Avisos sin tipo de decisión (`NOTICE_TYPES`): por ejemplo `addNotice("llave", "…")` muestra «🔒 …» arriba de las opciones. Todos los avisos van en ese lugar.
- `effect: () => …`: la opción cambia algo al elegirla. Por ejemplo, `giveItem("madera")` pone un objeto en el primer lugar libre sin preguntar, y `swapItem("espada_incandescente", "espada_manchada", aviso)` transforma un objeto en otro en el mismo lugar.
- `visited.has("escena")`: verdadero si el jugador ya salió alguna vez de esa escena. Sirve para mostrar otro texto al volver a un lugar. Se usa en `inicio` y `bosque`.
- Ejemplo ya en uso: el texto de `pantano_a` cambia según si el amuleto va en la mano o en el cuello.

### Rama del bosque (en reescritura)
- `cabana`: al acercarte crujen las maderas, se oyen chillidos de algo chico adentro y brilla una espada apoyada en la puerta. Opciones: «Tomar la espada» (te quedás en la escena), «Entrar a la cabaña» (`cabana_adentro`) y «Continuar el camino de árboles tallados» (`bosque_camino`) y «Volver hacia atrás, al pantano» (`inicio`), para no quedar atrapado entre la cabaña y la muralla. Al tomar la espada el texto cambia («La puerta quedó vacía…»).
- `cabana_adentro`: tres goblins bebés que chillan. El texto va en tres párrafos: la cabaña rota, los goblins en el rincón y las preguntas («¿Tienen padre, tienen madre?»). Dos opciones determinantes (rojas): «Asesinar a las criaturas» e «Irte y dejarlas vivas».
  - **Con la espada en la mano:** `cabana_matanza_espada`. La espada se mancha (pasa a ser «Espada manchada») y da «Infanticida armado».
  - **Sin la espada:** `cabana_matanza_madera`. Matás a los goblins con una madera del piso y da «Infanticida monstruoso». La madera queda en una mano libre, y al salir todavía podés tomar la espada con la otra.
  - Las dos matanzas tienen un solo botón, «Salir de la cabaña», que vuelve afuera. Si la espada sigue en la puerta, se puede tomar.
  - **Irte y dejarlas vivas:** da «Héroe de los goblins» y vuelve afuera, con los chillidos detrás de la puerta.
  - Después de entrar, afuera ya no aparece «Entrar a la cabaña» y el texto cambia según lo que hiciste.
- `bosque_camino`: el camino de árboles tallados termina frente a una muralla negra y alta que se pierde en la niebla. Opciones: «Caminar hacia la izquierda y revisar la muralla» (`muralla_izquierda`) y «Caminar hacia la derecha y revisar la muralla» (`muralla_derecha`). Se llega desde el bosque o desde la cabaña, se haya entrado o no: cada camino sigue funcionando por su cuenta. Sin sonidos por ahora.
- `muralla_izquierda`: la muralla sigue sin fin hasta una puerta negra en un recoveco. «Intentar abrir la puerta» muestra el aviso «🔒 Se necesita la llave trasera del mausoleo.» (en el mismo lugar que los avisos de objetos) y deja solo «Volver sobre tus pasos». Al volver más tarde se puede intentar de nuevo.
- `muralla_derecha`: la muralla sigue a la izquierda y a lo lejos se oyen golpes, espadas y gritos. Opciones: «Seguir caminando» (`muralla_casco`) y «Volver sobre tus pasos» (`bosque_camino`). Si ya se llegó al silencio, el texto dice «Desde adelante ya no llega ningún ruido.».
- `muralla_casco`: pisás algo de metal: un casco gastado con marcas. Opciones: «Ponerte el casco» (te quedás en la escena), «Dejarlo tirado» (sigue a `muralla_silencio`) y «Volver sobre tus pasos». Después de ponértelo, o al volver a pasar, la opción de seguir dice «Seguir hacia adelante». Si lo dejaste, sigue tirado ahí.
- `muralla_silencio`: los golpes y los gritos crecen y de golpe se callan. Opciones: «Continuar acercándote» (`muralla_pelea`), con el aviso «⚔ Lo que hiciste con la espada va a cambiar lo que pase.», y «Volver sobre tus pasos». Al volver: «Volvés al lugar donde se apagaron los ruidos…».
- `muralla_pelea`: la primera variable del juego. Un cuerpo en el barro y una criatura del pantano (la madre de los goblins) que lo arrastra al agua. Si entraste a la cabaña tiene «los mismos ojos»; si viste el casco, la armadura del cuerpo tiene sus mismas marcas. Según lo que hiciste (`creature.mode()`):
  - **Mataste a los goblins** (furia): «⚔ Hacerle frente con la espada» (espada manchada: la matás, determinante, da «La madre del pantano»; con casco el zarpazo pega en el casco, sin casco te deja un tajo), «⚔ Defenderte con la madera» (con casco la espantás; sin casco morís), «⚔ Alzar la espada» (si además tenés la incandescente), «Huir por donde viniste» (con casco escapás; sin casco morís) y «Quedarte quieto» (morís).
  - **Tenés la espada incandescente** (luz): «⚔ Alzar la espada» (la espanta), «Quedarte quieto» (se lleva el cuerpo) y «Volver sobre tus pasos».
  - **Dejaste vivos a los goblins** (perdón): te huele, ronronea y se va sin el cuerpo. «Acercarte al cuerpo» o volver.
  - **Nunca entraste a la cabaña** (extraño): «Quedarte quieto» (se lleva el cuerpo) o «Huir por donde viniste» (con casco escapás; sin casco morís).
- Escenas de cada salida: `muralla_pelea_espada`, `muralla_pelea_madera`, `muralla_luz`, `muralla_se_lo_lleva`, `muralla_huida` y las muertes `muerte_madera`, `muerte_huida` y `muerte_quieto`.
- `muralla_cuerpo`: el cuerpo está muerto y tiene un anillo. «Tomar el anillo» (pregunta en qué dedo), «Dejarle el anillo» y «Volver sobre tus pasos».
- `muralla_fin`: el pantano y la muralla se extienden sin límite y es imposible seguir. Solo queda «Volver sobre tus pasos». Si la criatura se llevó el cuerpo, al volver dice que no queda rastro.
- Al volver frente a la muralla (`bosque_camino`), el texto es «Volvés al lugar donde el bosque se termina…» y las opciones son izquierda, derecha y «Volver a la cabaña por el camino de árboles tallados». Al llegar así a la cabaña, el texto empieza con «Desandás el camino de árboles tallados…».
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

## Próxima sesión (lo que dejó dicho Julián el 29/10)
1. ~~Seguir con «Continuar el camino de árboles tallados»~~ y ~~el lado izquierdo de la muralla~~ (hechos). Lo próximo: qué pasa al revisar la muralla hacia la derecha.
2. **Correcciones de texto** de lo que se hizo el 28 y 29/10. Hecha: el orden de los párrafos de `cabana_adentro`. Si hay más, las aclara Julián.
3. **Un cambio en una decisión u opción** de lo que se hizo esos días. También lo aclara Julián.

### Lo que se hizo el 28 y 29/10 (ya publicado en `main`)
- Se sacaron todos los guiones largos y se sumaron reglas de estilo en `CLAUDE.md`: adaptar lo dictado, no usar guiones largos y pasar los textos por The Humanizer.
- La cabaña (`cabana`): espada incandescente en la puerta, «Tomar la espada», «Entrar a la cabaña» y «Continuar el camino de árboles tallados».
- Adentro (`cabana_adentro`): tres goblins bebés y el tipo nuevo de decisión «Determinante» (rojo, ✷). Las tres salidas dan «Infanticida armado», «Infanticida monstruoso» o «Héroe de los goblins».
- La espada se mancha si la usás. Sin la espada usás una madera, que te queda en la mano.
- Se sacó la espada rota de la encrucijada y de los objetos.
- Se anotaron los sonidos a buscar (ver Audio).
- Julián confirmó lo que había decidido Claude: el ícono ✷, «Tomar la espada» abajo de las otras opciones, la madera directo a una mano (después se puede cambiar desde el inventario) y los textos que no fueron dictados.

## Pendientes

### Historia (lo próximo)
- [ ] Revisar y modificar textos, recorridos y opciones de la historia. Se está haciendo rama por rama, empezando por el bosque.
- [x] Escribir `cabana` («Acercarte a la cabaña»).
- [x] Escribir `cabana_adentro` («Entrar a la cabaña») con los tres goblins bebés y sus dos decisiones determinantes.
- [x] Escribir `bosque_camino` («Continuar el camino de árboles tallados»).
- [x] Escribir `muralla_izquierda` (la puerta negra cerrada con llave).
- [x] Escribir `muralla_derecha` (los ruidos de pelea a lo lejos).
- [x] Escribir `muralla_casco` (el casco gastado en el suelo).
- [x] Escribir `muralla_silencio` (los ruidos se apagan de golpe).
- [x] Escribir `muralla_pelea`, la criatura, el cuerpo con el anillo y el final del camino de la derecha.
- [x] Sumar las muertes con «Volver dos decisiones atrás».
- [ ] Julián va a probar todo el camino de la derecha y pasar correcciones.
- [ ] Definir el impacto del anillo más adelante en la historia.
- [ ] Decidir dónde aparece la **llave trasera del mausoleo** y dónde se lleva en el cuerpo (lo define Julián). Con la llave, «Intentar abrir la puerta» de `muralla_izquierda` tiene que abrirla.
- [ ] Definir qué otras decisiones importantes dan fragmentos de memoria. «Lo que dejaste ir» sigue siendo provisorio; los de los goblins los nombró Julián.
- [ ] Definir otros tipos de decisión y sus colores (ya existe «Determinante», en rojo).
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
  - `muralla_izquierda` (intentar abrir la puerta): una puerta trabada que no se abre.
  - `muralla_derecha`: a lo lejos, golpes, choque de espadas y gritos lejanos.
  - `muralla_casco`: un pie que pisa algo de metal.
  - `muralla_silencio`: golpes, espadas y gritos que se intensifican y se cortan de golpe.
  - `muralla_pelea` y sus salidas: zarpazos, el casco que se abolla o suena como una campana, la madera que se parte, la espada que se clava, el chillido de la criatura, el chapoteo al hundirse en el agua y el ronroneo.
  - Las muertes: un cuerpo que cae al barro.
- [ ] Reconvertir la música a 128 kbps (hoy está a 320 kbps; la del Capítulo I pesa 7,6 MB). Hace falta ffmpeg o una herramienta online.
- [ ] El sonido de fragmento (`SFX_TRACKS.fragment`) está descargado pero desactivado.

### Ideas para más adelante
- [ ] Guardar el progreso del jugador.
- [ ] Pasar cada capítulo a su propio archivo cuando haya más de uno.
