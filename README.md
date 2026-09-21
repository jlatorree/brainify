# brainify

Plugin de [Claude Code](https://claude.com/claude-code) que convierte una carpeta en el
cerebro de un proyecto complejo: un vault de [Obsidian](https://obsidian.md) que además
es un grafo de conocimiento de [Graphify](https://github.com/Graphify-Labs/graphify).

Está pensado para quien no programa: brainify corre los comandos por ti y te explica
en simple qué hizo.

## Qué hace

- **Configura la carpeta sola** la primera vez que lo invocas: crea la estructura de
  carpetas, el `.graphifyignore`, el `CLAUDE.md` del proyecto y la conexión con Graphify.
- **Ordena el avance que ya tenías:** si la carpeta ya tiene notas, subcarpetas, PDFs,
  imágenes o canvas, los reparte en la estructura sin romper los enlaces de Obsidian.
  Antes hace un respaldo, y todo se puede deshacer.
- **Te pone al día** al empezar cada sesión: estado del proyecto, decisiones recientes,
  preguntas abiertas y lo que falta procesar.
- **Procesa el inbox,** incluidas las notas de reunión, en notas atómicas enlazadas.
- **Guarda el conocimiento** que surge conversando ("guarda esto").
- **Gestiona las preguntas abiertas** y las gradúa a conocimiento o decisiones cuando
  se resuelven.
- **Investiga literatura académica** con un protocolo de validez, impacto y actualidad,
  y cita cada afirmación con su link.
- **Ubica e indexa entregables** generados por otros skills: informes, diagramas,
  exceles y presentaciones.

## La estructura que crea

```
<tu proyecto>/
├── 00_inbox/             capturas rápidas y notas de reunión (archive/ para lo procesado)
├── 01_knowledge/         conocimiento: una idea por nota, siempre enlazada
├── 02_decisions/         decisiones y su porqué
├── 03_open_questions/    preguntas abiertas (archive/ para las resueltas)
├── 04_sources/           literature/ (papers) y web/ (fuentes web)
├── 05_deliverables/      reports/, diagrams/, data/ y presentations/
├── 06_exports/           copias finales para compartir
└── CLAUDE.md             hace que cada sesión en la carpeta arranque con brainify
```

## Instalación

Dentro de Claude Code, escribe estos dos comandos (uno a la vez):

```
/plugin marketplace add jlatorree/brainify
```

```
/plugin install brainify@brainify
```

Lo que brainify necesita y **él mismo instala la primera vez** (te pide un "sí" antes):

- [Graphify](https://github.com/Graphify-Labs/graphify) (paquete `graphifyy`), con
  soporte de PDF y Office.
- [uv](https://docs.astral.sh/uv/), el instalador de Graphify. Si no está, brainify lo
  instala con [Homebrew](https://brew.sh).

## Uso

1. Abre Claude Code en la carpeta de tu proyecto (nueva o con avance).
2. Escribe **brainify**.

Después puedes pedirle cosas con frases normales:

| Dices | brainify |
|---|---|
| "ponme al día" | resume el estado, las decisiones recientes y las preguntas abiertas |
| "ordena esta carpeta" | reparte los archivos sueltos en la estructura sin romper enlaces |
| "procesa el inbox" | convierte capturas y notas de reunión en notas enlazadas |
| "guarda esto" | crea una nota atómica enlazada con lo que se habló |
| "qué preguntas siguen abiertas" | lista las preguntas pendientes por prioridad |
| "investiga la literatura sobre..." | busca, selecciona y ficha papers, y redacta un informe citado |
| "dónde guardo este entregable" | lo ubica, crea su ficha y lo mete al grafo |

## Qué hay en el plugin

```
plugins/brainify/skills/brainify/
├── SKILL.md                  instrucciones del skill
├── templates/                plantillas de notas, fichas, CLAUDE.md y .graphifyignore
└── scripts/ordenar.py        herramienta para ordenar sin romper enlaces (Python 3, sin dependencias)
```

## Créditos

Graphify es un proyecto de [Graphify Labs](https://github.com/Graphify-Labs/graphify).
brainify lo usa como motor del grafo de conocimiento.
