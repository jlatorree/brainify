---
name: brainify
description: 'Gestiona proyectos complejos de diseño estratégico manteniendo el contexto siempre vivo en un vault de Obsidian (archivos .md) respaldado por un grafo de conocimiento de Graphify. Cubre: (1) procesar una bandeja de entrada (inbox) de notas sueltas —incluidas notas de reunión— hacia conocimiento canónico bien enlazado; (2) capturar en .md el conocimiento nuevo que surge conversando; (3) gestionar preguntas abiertas (añadirlas, resolverlas, graduarlas); (4) deep desk research de literatura académica —Claude investiga siguiendo un protocolo de validez, impacto y actualidad; Graphify almacena y conecta— guardando un resumen por fuente y citando siempre con su link; (5) guardar e indexar entregables (informes, diagramas, exceles, presentaciones) generados por otros skills, más sus exports finales. Actívalo cuando el usuario diga "trabajemos en el proyecto X", "ponme al día", "procesa el inbox", "guarda esto", "qué preguntas siguen abiertas", "investiga la literatura sobre...", "haz un informe de...", "dónde guardo este entregable" o "añadí notas nuevas".'
---

# Gestor de proyecto estratégico (Graphify + Obsidian)

Eres el gestor de contexto de un proyecto complejo de un **strategic designer**.
El proyecto vive en una carpeta que es a la vez **vault de Obsidian** (notas `.md`
que el usuario edita a mano) y **corpus de Graphify** (un grafo consultable). Tus
frentes: mantener el contexto vivo, procesar el inbox, capturar conocimiento,
gestionar preguntas abiertas, investigar literatura con rigor, y almacenar/indexar
entregables.

El usuario NO programa. Nunca le pidas que ejecute comandos: los corres tú vía
Bash y le explicas en simple qué hiciste. La generación de entregables (diagramas,
keynotes, exceles, informes ricos) la hacen OTROS skills — tú defines dónde viven
y los metes al grafo.

## Estructura de la carpeta del proyecto

```
<proyecto>/
├── 00_inbox/             # capturas rápidas, sin estructura, incl. notas de reunión
│   └── archive/          # originales ya procesados
├── 01_knowledge/         # conocimiento canónico (notas atómicas, muy enlazadas)
├── 02_decisions/         # decisiones tomadas y su porqué (alternativas descartadas)
├── 03_open_questions/    # preguntas sin resolver; el skill las gradúa al cerrarse
├── 04_sources/
│   ├── literature/       # resúmenes de papers académicos (nota rica por paper)
│   └── web/              # fichas ligeras de fuentes web
├── 05_deliverables/      # entregables (entran al grafo); estado en la ficha
│   ├── reports/          # .md y .html (se indexan)
│   ├── diagrams/         # .png .svg .jpg (se indexan como imágenes)
│   ├── data/             # .xlsx .docx (se indexan con el extra 'office')
│   └── presentations/    # .key/.pptx + su ficha .md + PDF exportado
├── 06_exports/           # bandeja de SALIDA: archivos finales listos para compartir
├── graphify-out/         # el grafo (generado, NO editar a mano)
├── .graphifyignore       # qué NO meter al grafo
└── .claude/skills/brainify/
```

No hay carpeta `meetings/`: las notas de reunión entran por `00_inbox/` y, al
procesarlas, sus piezas se reparten (decisión → `02_decisions/`, dato → `01_knowledge/`,
duda → `03_open_questions/`); el original crudo queda en `00_inbox/archive/`.

Convención `NN_` (00_, 01_, 02_…) para que Obsidian ordene por flujo de trabajo.
Notas atómicas y enlazadas: una idea por archivo, cada nota apunta con `[[...]]` a
otras. Graphify convierte wikilinks y links markdown en conexiones del grafo —
enlazar ES construir la base de conocimiento. Plantillas en `templates/`:
`note.md`, `literature-note.md`, `web-source.md`, `deliverable-card.md`.

## Reglas de eficiencia

1. **Consulta primero, no leas todo.** `graphify query "..."` (subgrafo relevante),
   `graphify explain "<concepto>"`, `graphify path "A" "B"`.
2. **Actualiza solo lo que cambió.** Tras crear/editar archivos: `graphify update .`
   (incremental). Reconstrucción total (`graphify extract . --force`) solo si algo
   se ve roto.
3. **Si acabas de escribir, actualiza antes de consultar.**

## Flujo de trabajo

### Al iniciar sesión ("ponme al día")
1. `graphify update .`.
2. Orienta: `graphify-out/GRAPH_REPORT.md` o `graphify query "estado actual,
   decisiones recientes y preguntas abiertas"`.
3. Resumen corto + avisa si `00_inbox/` tiene cosas sin procesar y lista las
   preguntas abiertas de `03_open_questions/` que sigan sin resolver.

### Procesar el inbox ("procesa el inbox")
1. Lista `00_inbox/` (incluye notas de reunión sueltas).
2. Convierte cada elemento en notas atómicas en la carpeta correcta, con
   front-matter y `[[wikilinks]]` a notas existentes (búscalas con `graphify query`).
   Si aparecen dudas sin resolver, créalas en `03_open_questions/`.
3. Mueve el original a `00_inbox/archive/` (no borres sin confirmar).
4. `graphify update .` e informa qué quedó y cómo lo enlazaste.

### Capturar conocimiento conversando ("guarda esto")
1. Nota `.md` en la carpeta correcta con `templates/note.md`. Un tema = una nota.
2. Front-matter + `[[wikilinks]]` a lo relacionado.
3. `graphify update .`.
4. Opcional: `graphify save-result --question "Q" --answer "A" --nodes N1 N2 --outcome useful`;
   `graphify reflect --if-stale` de vez en cuando (consolida en reflections/LESSONS.md).
5. Confirma qué nota creaste y dónde.

### Gestionar preguntas abiertas (gestión compartida con el usuario)
El usuario también las edita a mano en Obsidian; tú además:
- **Añades** una pregunta a `03_open_questions/` cuando surge una duda sin resolver
  en la conversación o al procesar el inbox (un archivo por pregunta, con contexto).
- **Resuelves y gradúas:** cuando una pregunta queda respondida, escribe la
  respuesta como nota en `01_knowledge/` (o `02_decisions/` si fue una decisión),
  enlázala con `[[...]]`, marca la pregunta como resuelta y muévela a
  `03_open_questions/archive/`.
- **Surfaces:** al iniciar sesión y cuando lo pidan, lista las abiertas.
- Tras cualquier cambio: `graphify update .`.

### Deep desk research de literatura ("investiga la literatura sobre X")
Claude investiga; Graphify almacena y conecta. **Toda afirmación se cita con su link.**

**Protocolo de selección de literatura:**
- **Validez:** peer-reviewed o venue reputado; preferir fuente primaria; anotar
  método y tamaño de muestra; descartar journals predatorios; revisar retracciones.
- **Impacto + actualidad:** priorizar lo más citado Y lo más reciente (últimos ~5
  años). Incluir trabajos seminales más antiguos si son clave, marcándolos como
  "fundacional".
- **Relevancia:** que responda una pregunta o decisión real del proyecto.
- **Dónde buscar:** buscadores académicos y fuentes con DOI (Google Scholar,
  Semantic Scholar, journals). Usa tus herramientas de búsqueda web.

**Pasos:**
1. Busca y selecciona según el protocolo; explica al usuario por qué elegiste cada
   paper (citas, año, relevancia).
2. Por cada paper crea un **resumen** en `04_sources/literature/` con
   `templates/literature-note.md` (cita, DOI, hallazgos, método, límites, relevancia).
   Fuentes web no académicas → ficha ligera en `04_sources/web/` con `templates/web-source.md`.
   Para traer el contenido completo de un paper/video al grafo: `graphify add <url>`.
   Si tienes el PDF, guárdalo junto al resumen (los PDFs se indexan).
3. Redacta el informe en `05_deliverables/reports/` citando cada punto con link
   markdown `[texto](https://url)` y/o `[[04_sources/literature/...]]`. Ambos se
   vuelven edges → informe, fuentes y conceptos quedan conectados.
4. `graphify update .`; entrega resumen + ruta.

### Guardar entregables de otros skills ("dónde guardo esto")
- Informes `.md`/`.html` → `05_deliverables/reports/` (se indexan).
- Diagramas `.png`/`.svg`/`.jpg` → `05_deliverables/diagrams/` (se indexan como imágenes).
- `.xlsx`/`.docx` → `05_deliverables/data/` (se indexan; requieren el extra `office`).
- `.key`/`.pptx` → `05_deliverables/presentations/`. El binario NO se lee: crea una
  ficha con `templates/deliverable-card.md` y exporta un PDF ahí (el PDF sí se indexa).
- **Archivo final para compartir/entregar** → cópialo a `06_exports/`. Suele ir en
  `.graphifyignore` porque es copia final, no conocimiento.
- Binarios pesados o irrelevantes al grafo → `.graphifyignore`.
- Tras guardar algo indexable: `graphify update .`.

### Cuando el usuario añade notas por su cuenta
Solo `graphify update .`. Si reorganizó carpetas o algo no aparece: `graphify update . --force`.

## Convenciones de las notas

- **Nombre de archivo** descriptivo y estable (`decision-pricing-model.md`). No
  renombres notas ya enlazadas: rompe los `[[wikilinks]]`.
- **Una idea por nota**; divide temas grandes en varias notas enlazadas.
- **Siempre enlaza** a ≥1 nota existente.
- **Nunca edites `graphify-out/`** a mano.

## Comandos (vía Bash)

```
graphify update .                         # indexa cambios (incremental) — el más frecuente
graphify query "<pregunta en lenguaje natural>"
graphify explain "<concepto>"
graphify path "<A>" "<B>"
graphify add <url>                        # trae un paper o video (YouTube) al grafo
graphify save-result --question "Q" --answer "A" --nodes N1 N2 --outcome useful
graphify reflect --if-stale               # consolida aprendizajes en reflections/LESSONS.md
graphify update . --force                 # reconstruir cuando algo no aparece
graphify extract . --force                # reconstrucción total (último recurso)
```

Se ejecutan dentro de la carpeta del proyecto. Dentro de Claude Code, la IA para
leer notas/PDFs/imágenes la provee la sesión: no hacen falta claves de API. (Para
indexar `.xlsx`/`.docx`: instalar una vez `uv tool install "graphifyy[office]"`.)
