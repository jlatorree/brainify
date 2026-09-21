# brainify

Un skill que convierte cada proyecto en un **segundo cerebro**: una carpeta que es a la
vez vault de Obsidian y grafo de conocimiento. Para que dejes de acumular archivos sueltos
y puedas preguntarle a tu proyecto qué sabe, qué decidiste y qué falta. Funciona en
**Claude Code**.

## La idea en 30 segundos

Piensa en cada proyecto como un **cerebro propio**. Tiene seis cosas:

- 📥 **El inbox:** donde sueltas todo (apuntes, notas de reunión, PDFs). Se vacía al
  procesarse.
- 🧠 **El conocimiento:** notas atómicas, una idea por nota, siempre conectadas. Incluye
  las **decisiones** (con su porqué) y las **preguntas abiertas**.
- 📚 **Las fuentes:** una ficha por paper o página web, con su cita lista para usar.
- 📦 **Los entregables:** informes, diagramas, presentaciones. Cada uno dice de qué notas
  y fuentes nació.
- 🕸️ **El grafo** ([Graphify](https://github.com/Graphify-Labs/graphify)): el mapa de
  conexiones que Claude consulta en vez de releerlo todo.
- 🤖 **El guía** (Claude): corre cada comando por ti y te cuenta en simple qué hizo.

Y un flujo, siempre en la misma dirección:

> **Sueltas algo en el inbox o conversas → se vuelve notas conectadas → las preguntas se
> gradúan en conocimiento o decisiones → los entregables citan sus fuentes → el grafo se
> actualiza solo.**

Eso es todo. Lo demás son detalles que brainify maneja por ti.

## Por qué funciona así (la filosofía)

El conocimiento no vive en los documentos: vive en las **conexiones** entre ellos. Una
nota suelta es un dato; conectada a otras, empieza a ser una idea. brainify existe para que
esas conexiones no se pierdan, y para que se puedan consultar.

1. **Enlazar es construir.** Cada nota se conecta con otras. Graphify convierte esas
   conexiones en un grafo, y Claude responde consultando el grafo en vez de leer todo.
2. **Tú no programas; Claude sí.** No hay comandos que aprender. brainify corre todo y te
   explica qué hizo, dónde quedó y qué sigue.
3. **Toda afirmación con su fuente.** Cada dato de un informe lleva **doble cita**: el link
   real, para verificarlo, y la ficha de la fuente, para que quede conectado en el grafo.
4. **Nada se pierde.** Lo procesado se mueve, no se borra. Antes de reordenar hay un
   respaldo, y todo orden se puede deshacer.
5. **Barato por diseño.** Ponerse al día es un solo comando; la lectura cara (PDFs,
   imágenes) se hace solo cuando hace falta.

## Cómo se usa (es conversacional)

Abres Claude Code en la carpeta del proyecto y escribes **brainify**. Después le hablas con
frases normales:

| Dices | brainify |
| --- | --- |
| "ponme al día" | resume el estado, las decisiones recientes, las preguntas abiertas y el inbox |
| "procesa el inbox" | convierte apuntes y notas de reunión en notas conectadas, y deja el inbox vacío |
| "guarda esto" | guarda lo que surgió en la conversación como una nota conectada |
| "qué preguntas siguen abiertas" | lista las preguntas por prioridad y gradúa las resueltas |
| "investiga la literatura sobre..." | lanza el deep research de Claude Code y guarda fichas e informe con doble cita |
| "haz un informe de..." / "dónde guardo este entregable" | lo arma o lo ubica, crea su ficha y lo conecta |
| "ordena esta carpeta" | reparte archivos sueltos en la estructura sin romper enlaces |
| "añadí notas nuevas" | pone el grafo al día con lo que escribiste en Obsidian |

También puedes invocarlo por nombre: `/brainify` (o `/brainify:brainify` si lo instalaste
como plugin).

## Los tres modos

brainify detecta el estado de la carpeta y actúa según corresponda:

| Estado | Modo | Qué hace |
| --- | --- | --- |
| Carpeta vacía | **Configurar** | Crea la estructura, el `CLAUDE.md` del proyecto y la conexión con Graphify. Si falta Graphify, lo instala (con tu OK). |
| Carpeta con avance previo | **Ordenar** | Hace inventario, te propone un plan y, **con un solo OK**, respalda, mueve todo a su lugar y corrige cada enlace de Obsidian. Se puede deshacer. |
| Proyecto que ya usa brainify | **Ponerse al día** | Te resume inbox, preguntas, decisiones y el estado del grafo en unas líneas. |

## Instalación

Necesitas **Claude Code en Mac**. Lo demás (Graphify y sus extras para PDF y Office) lo
instala brainify la primera vez, pidiéndote un "sí".

> ℹ️ **Importante:** los comandos que empiezan con `/` (como `/plugin …`) se **escriben
> dentro del chat de Claude Code**, no en la terminal. Lo que sí va en la terminal es
> `npx …`.

### Paso 0 opcional: si trabajas en Obsidian

Los skills de Obsidian de kepano ayudan a que el markdown (frontmatter, wikilinks) salga
nativo de Obsidian. En el **chat** de Claude Code escribe
`/plugin marketplace add kepano/obsidian-skills` (o en la terminal,
`npx skills add kepano/obsidian-skills`). Es una mejora, no un requisito.

### Claude Code

**Desde el chat**, por marketplace (escribe estas dos líneas en el chat, no en la terminal):

    /plugin marketplace add jlatorree/brainify
    /plugin install brainify@brainify

> ℹ️ Tras instalar por marketplace, **reinicia tu sesión de Claude Code** antes de usar
> brainify por primera vez: un plugin recién instalado queda en disco pero no aparece como
> skill hasta abrir una sesión nueva.

**O desde la terminal** (una línea):

```sh
npx skills add jlatorree/brainify
```

Queda instalado a nivel de usuario, así que está disponible en **todas** tus carpetas (no
hay que reinstalar por proyecto).

> 🔒 Mientras el repo sea privado, la instalación solo funciona en cuentas de GitHub con
> acceso a él.

### Cowork y Claude.ai

No está probado. brainify necesita correr Graphify y Python en tu computador, algo que hoy
solo está garantizado en Claude Code.

## Actualizar a una versión nueva

Un plugin de marketplace **no se actualiza solo por defecto**: el clon local del
marketplace no se refresca hasta que se lo pides. Por eso conviene:

- **Recomendado, una sola vez:** en el chat abre `/plugin` → pestaña **Marketplaces** →
  **brainify** → activa **auto-update**. Desde entonces, cada versión nueva llega sola al
  abrir una sesión.
- **Manual:** en el chat de Claude Code,

      /plugin marketplace update brainify
      /plugin update brainify@brainify
      /reload-plugins

  El primer comando es el clave: refresca el clon local. Sin él, `/plugin update` dice
  "ya estás al día" aunque no lo estés.

**Por npx:** vuelve a correr `npx skills add jlatorree/brainify` en la terminal.

## La estructura (para cuando quieras el detalle)

brainify crea esto en cada proyecto:

```
00_inbox/                 lo que sueltas: se vacía al procesarse
01_knowledge/             conocimiento: una idea por nota
02_decisions/             decisiones, su porqué y alternativas descartadas
03_open_questions/        una pregunta por archivo (archive/ para las ya graduadas)
04_sources/literature/    una ficha por paper (+ su PDF)
04_sources/web/           una ficha por fuente web
05_deliverables/          reports/, diagrams/, data/, presentations/ (cada uno con su ficha)
06_exports/               copias finales para compartir
CLAUDE.md                 hace que cada sesión en la carpeta arranque con brainify
.brainify/                respaldos, inbox ya procesado y registros (oculto en Obsidian)
```

### La distinción que importa: nota, fuente y entregable

- **Una nota** (`01` a `03`) es lo que *sabes*: una idea, una decisión o una pregunta. Está
  viva: se amplía con el tiempo.
- **Una fuente** (`04`) es lo que *leíste*: una ficha por paper o web, con su cita y sus
  hallazgos. Los papers los lee el deep research de Claude Code; brainify guarda la ficha.
- **Un entregable** (`05`) es lo que *produjiste* para alguien: dice de qué notas y
  fuentes nació. La copia que envías va a `06_exports/`.

**¿Y la bandeja de entrada?** Es solo de paso: todo lo que entra se procesa en notas y el
inbox queda vacío. Los originales se guardan en `.brainify/`, fuera de la vista.

### El grafo, y por qué brainify gasta poco

- **Sincronizar el grafo es gratis:** Graphify registra notas y conexiones en segundos, sin
  IA. Las notas que escribe brainify ya llevan sus conexiones.
- **La lectura profunda** (la IA lee PDFs, imágenes y Office) se usa solo cuando hace falta.
- **Ponerse al día es un solo comando** que resume todo en unas 10 líneas, en vez de leer
  reportes o notas.
- **El inbox y los PDFs de papers quedan fuera del grafo:** el inbox se procesa en notas, y
  cada paper ya está representado por su ficha. Nada se lee dos veces.

## Qué incluye el skill

Estructura de plugin: el skill vive en `skills/brainify/`, con su manifiesto en
`.claude-plugin/`.

| Archivo | Rol |
| ------- | --- |
| `skills/brainify/SKILL.md` | El skill: vocabulario, estructura y los ocho flujos. Corto, porque se carga en cada uso. |
| `skills/brainify/references/configurar.md` | Configurar una carpeta nueva (instala Graphify si falta). |
| `skills/brainify/references/ordenar.md` | Ordenar una carpeta con avance previo sin romper enlaces. |
| `skills/brainify/references/investigar.md` | Encargo al deep research y cómo guardar su resultado. |
| `skills/brainify/references/entregables.md` | Dónde vive cada entregable, fichas, PDFs y versiones. |
| `skills/brainify/references/problemas.md` | Qué hacer cuando Graphify falla. |
| `skills/brainify/templates/` | Plantillas de nota, ficha de paper, ficha web, ficha de entregable, `CLAUDE.md` y `.graphifyignore`. |
| `skills/brainify/scripts/brainify.py` | Estado del proyecto, índice de notas y orden sin romper enlaces (inventario, respaldo, mover, verificar, deshacer). Python 3, sin dependencias. |
| `.claude-plugin/{plugin,marketplace}.json` | Manifiestos para instalar vía plugin/marketplace. |

---

*Usa [Graphify](https://github.com/Graphify-Labs/graphify) como motor del grafo de
conocimiento. Inspirado en [Many Brains](https://github.com/jlatorree/many-brains).*
