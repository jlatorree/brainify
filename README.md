# brainify

Un skill de **Claude Code** que convierte cada proyecto en un **segundo cerebro**: una
carpeta que es a la vez vault de Obsidian y grafo de conocimiento. Para dejar de acumular
archivos sueltos y poder preguntarle a tu proyecto qué sabe, qué decidiste y qué falta.

## La idea en 30 segundos

Cada proyecto es un **cerebro propio** con seis partes:

- 📥 **Inbox:** donde sueltas todo (apuntes, reuniones, PDFs). Se vacía al procesarse.
- 🧠 **Conocimiento:** notas atómicas y conectadas, incluidas **decisiones** y **preguntas abiertas**.
- 📚 **Fuentes:** una ficha por paper o web, con su cita lista.
- 📦 **Exports:** tus entregables, cada uno con las notas y fuentes de las que nació.
- 🕸️ **Grafo** ([Graphify](https://github.com/Graphify-Labs/graphify)): el mapa que Claude consulta en vez de releerlo todo.
- 🤖 **Guía** (Claude): corre cada comando y te cuenta en simple qué hizo.

> **Sueltas algo o conversas → se vuelve notas conectadas → las preguntas se gradúan en
> conocimiento o decisiones → los entregables citan sus fuentes → el grafo se actualiza.**

## La filosofía

El conocimiento vive en las **conexiones**, no en los documentos.

1. **Enlazar es construir:** cada nota se conecta; Claude responde consultando el grafo.
2. **Tú no programas:** brainify corre todo y te explica qué hizo.
3. **Toda afirmación con su fuente:** doble cita, el link real y la ficha en el grafo.
4. **Nada se pierde:** se mueve, no se borra; todo orden tiene respaldo y se deshace.
5. **Barato por diseño:** ponerse al día es un comando; la lectura cara, solo si hace falta.

## Cómo se usa

Abre Claude Code en la carpeta del proyecto, escribe **brainify** y habla normal:

| Dices | brainify |
| --- | --- |
| "ponme al día" | resume estado, decisiones, preguntas abiertas e inbox |
| "procesa el inbox" | convierte apuntes y reuniones en notas conectadas |
| "guarda esto" | guarda lo que surgió como nota conectada |
| "qué preguntas siguen abiertas" | las lista por prioridad y gradúa las resueltas |
| "investiga la literatura sobre..." | deep research con fichas e informe con doble cita |
| "haz un informe de..." | lo arma o lo ubica en exports, con su ficha |
| "ordena esta carpeta" | reparte archivos sueltos sin romper enlaces |
| "añadí notas nuevas" | pone el grafo al día |

Según la carpeta, brainify **configura** (vacía), **ordena** (con avance previo, con un
solo OK y reversible) o te **pone al día** (ya usa brainify).

## Instalación

Necesitas **Claude Code en Mac**; Graphify lo instala brainify la primera vez, con tu OK.
En el **chat** de Claude Code (no en la terminal):

    /plugin marketplace add jlatorree/brainify
    /plugin install brainify@brainify

Luego **abre una sesión nueva**: el plugin recién instalado aparece como skill desde ahí.

## Actualizar

Activa el **auto-update** una vez: `/plugin` → **Marketplaces** → **brainify**. O a mano:

    /plugin marketplace update brainify
    /plugin update brainify@brainify
    /reload-plugins

## La estructura

```
00_inbox/                 lo que sueltas: se vacía al procesarse
01_knowledge/             una idea por nota
02_decisions/             decisiones, su porqué y alternativas descartadas
03_open_questions/        una pregunta por archivo (archive/ para las graduadas)
04_sources/               literature/ (fichas de papers + PDFs) y web/
05_exports/               entregables: reports/, diagrams/, data/, presentations/
CLAUDE.md                 hace que cada sesión arranque con brainify
.brainify/                respaldos, inbox procesado y registros (oculto)
```

- **Nota** (`01`–`03`): lo que *sabes*. Viva, se amplía.
- **Fuente** (`04`): lo que *leíste*, con su cita y hallazgos.
- **Export** (`05`): lo que *produjiste*, con una ficha que dice de qué nació.

Sincronizar el grafo es gratis (sin IA); la lectura profunda de PDFs e imágenes se usa
solo cuando hace falta, y el inbox y los PDFs de papers quedan fuera del grafo.

## Qué incluye

| Archivo | Rol |
| --- | --- |
| `SKILL.md` | El skill: vocabulario, estructura y flujos. |
| `references/` | Configurar, ordenar, investigar, entregables y problemas con Graphify. |
| `templates/` | Nota, ficha de paper, web y entregable, `CLAUDE.md` y `.graphifyignore`. |
| `scripts/brainify.py` | Estado, índice y orden sin romper enlaces. Python 3, sin dependencias. |
| `.claude-plugin/` | Manifiestos del plugin y del marketplace. |

---

*Motor del grafo: [Graphify](https://github.com/Graphify-Labs/graphify). Inspirado en
[Many Brains](https://github.com/jlatorree/many-brains).*
