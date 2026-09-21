# <Nombre del proyecto>

## brainify

Este proyecto se gestiona con el skill **brainify**. La carpeta es a la vez vault de
Obsidian y grafo de conocimiento de Graphify.

- **Al empezar cada sesión**, usa el skill brainify para ponerte al día: actualiza el
  grafo, revisa el inbox y lista las preguntas abiertas.
- **Para cualquier pregunta sobre el proyecto**, consulta primero el grafo
  (`graphify query "..."`) y abre solo las notas que señale.
- **Todo conocimiento nuevo** se guarda con las convenciones de brainify: una idea
  por nota, en la carpeta que corresponde y siempre enlazada con `[[wikilinks]]`.
- **Las notas no son código:** `graphify update .` solo registra notas y enlaces.
  Para que el grafo lea el contenido nuevo (notas, PDFs, imágenes), corre
  `/graphify . --update` al cerrar cada bloque de trabajo.
- **El usuario no programa:** corre tú los comandos y explícale en simple qué hiciste.

**Proyecto:** <nombre de la carpeta>
**De qué trata:** <una o dos frases; pídeselas al usuario si aún no están>
**Configurado con brainify:** <AAAA-MM-DD>
