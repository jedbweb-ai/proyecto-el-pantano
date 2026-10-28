# El Pantano — Reglas para Claude

Juego narrativo en HTML, CSS y JS, sin build. Todo el juego vive en `el-pantano-capitulo-1.html`; las ilustraciones van en `img/` y el audio en `audio/`, cada carpeta con su README.

Publicado con GitHub Pages desde `main`: https://jedbweb-ai.github.io/proyecto-el-pantano/

Hablale a Julián en español.

## Antes de empezar
- Leé `ESTADO.md`: tiene lo hecho, las herramientas para escribir la historia y la lista de pendientes.
- Al terminar un cambio, actualizá `ESTADO.md` (marcá lo hecho y sumá lo nuevo).

## Objetos del inventario
- Los lugares del cuerpo donde puede ir cada objeto (`fits` en `ITEMS`) los define Julián, no se deducen de lo que sería físicamente posible. Si suma un objeto sin decir dónde va, preguntale antes de definir `fits`.
- Un objeto con un solo lugar se pone directo y avisa con `takeText` (por ejemplo «Te pusiste el pantalón viejo»), sin preguntar dónde llevarlo.

## Historia
- Los fragmentos de memoria salen solo de decisiones importantes (`memory` en la opción). Sus nombres son provisorios hasta que Julián los defina.
- Un tipo de decisión nuevo se agrega en `CHOICE_TYPES` con su color y su ícono; después hay que actualizar las instrucciones del juego.

## Flujo de cada cambio
1. Probá el cambio en un navegador con un servidor local (por ejemplo `npx serve` o `python -m http.server`). Si creaste un `.claude/launch.json` temporal, borralo después.
2. Si el navegador está oculto, las animaciones CSS se frenan: desactivalas en la prueba antes de medir opacidades o sacar capturas.
3. Preguntá «¿Hago el commit y lo publico en GitHub?» y hacé commit y push solo cuando Julián diga que sí, un cambio a la vez.
4. **En la PC:** se trabaja en `main`. Después del push, esperá el build de Pages y confirmá que la página publicada tiene el cambio. Informá el hash del commit.
5. **En una sesión en la nube:** trabajá en una rama y abrí un PR. No lo unas a `main` sin el OK de Julián; al unirlo, Pages publica solo y hay que confirmar el build igual que en la PC.
