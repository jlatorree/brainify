# Configurar un proyecto

Se sigue cuando la carpeta actual no tiene `CLAUDE.md` con la sección `## brainify`.
Todo lo haces tú; pides permiso solo donde se indica.

**Terminado cuando:** existen las carpetas de la estructura, `.graphifyignore`, y
`CLAUDE.md` con las secciones `## brainify` y `## graphify`; `BRAIN inventario` marca
0 archivos por ordenar (o solo los que el usuario decidió dejar); y el usuario recibió
el mensaje final.

1. **Carpeta.** El proyecto es la carpeta donde se abrió Claude Code (`pwd`). Si es la
   carpeta personal (`~`), el Escritorio, Documentos a secas o una carpeta del
   sistema, pregunta en qué carpeta vive el proyecto antes de crear nada.
2. **Graphify.** Corre `graphify --version`.
   - Si falta: pide OK en una línea. Con el OK, `uv tool install "graphifyy[pdf,office]"`
     y luego `graphify install`. Si tampoco hay `uv` y hay Homebrew (`brew --version`),
     antes `brew install uv` (con el mismo OK). Sin Homebrew, explica en simple que
     primero hay que instalarlo desde brew.sh y detente.
   - Si está, revisa los extras de PDF y Office:

     ```bash
     PY=$(head -1 "$(command -v graphify)" | sed 's/^#!//'); "$PY" -c "import pypdf, docx, openpyxl" 2>/dev/null && echo "extras OK" || echo "faltan extras"
     ```

     Si faltan, pide OK y corre `uv tool install --reinstall "graphifyy[pdf,office]"`
     y luego `graphify install`. Si dice que no, sigue y avísale que los PDFs, `.xlsx`
     y `.docx` quedarán fuera del grafo.
3. **Carpetas** (`mkdir -p` respeta lo que ya existe):

   ```bash
   mkdir -p 00_inbox 01_knowledge 02_decisions 03_open_questions/archive 04_sources/literature 04_sources/web 05_exports/reports 05_exports/diagrams 05_exports/data 05_exports/presentations
   ```
4. **`.graphifyignore`:** copia `templates/graphifyignore` del skill como
   `.graphifyignore`. Si ya existe, agrega al final solo las líneas que le falten.
5. **`CLAUDE.md`:** créalo desde `templates/CLAUDE.md` (nombre del proyecto = nombre de
   la carpeta; fecha de hoy). Si ya existe, agrega al final la sección `## brainify`
   de la plantilla y conserva todo lo demás.
6. **Graphify en cada sesión:** `graphify claude install`. Agrega la sección
   `## graphify` a `CLAUDE.md` y avisos automáticos en `.claude/settings.json`;
   repetirlo es seguro. Después quita el aviso que se dispara en cada lectura de
   archivo: en un vault de notas cuesta ~100 tokens por lectura y empuja a consultar
   el grafo antes de abrir notas que ya sabes que necesitas. El aviso de búsquedas
   (`grep`, `find`) se queda, porque ahí sí ahorra:

   ```bash
   python3 - <<'EOF'
   import json
   p = ".claude/settings.json"
   d = json.load(open(p))
   d["hooks"]["PreToolUse"] = [h for h in d["hooks"]["PreToolUse"] if h.get("matcher") != "Read|Glob"]
   json.dump(d, open(p, "w"), indent=2)
   EOF
   ```
7. **Avance previo:** `BRAIN inventario`. Si hay archivos por ordenar, sigue
   `references/ordenar.md` ahora.
8. **Grafo:** sincroniza. En una carpeta nueva responde "nothing to rebuild", y es
   normal: el grafo nace con la primera nota. Si hay PDFs, imágenes u Office, ofrece
   la lectura profunda diciendo cuántos archivos son.
9. **Mensaje final**, en pocas líneas, por ejemplo:

   ```
   Listo, este proyecto ya funciona con brainify:
   - Creé las carpetas: inbox, conocimiento, decisiones, preguntas, fuentes y exports.
   - Creé CLAUDE.md: cada sesión nueva en esta carpeta arrancará usando brainify y el grafo.
   - El grafo se crea con la primera nota y lo mantendré al día mientras trabajamos.
   Para empezar, deja apuntes o notas de reunión en 00_inbox y dime "procesa el inbox".
   ```

   Y pregunta en una línea de qué trata el proyecto (una o dos frases) para anotarlo
   en `CLAUDE.md`.
