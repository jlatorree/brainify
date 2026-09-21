# Deep research de literatura

Claude investiga; Graphify almacena y conecta.

**Regla de oro:** cada afirmación del informe lleva **doble cita**, y cada referencia
viene de una fuente que abriste: su DOI responde 302 en doi.org. Si no encuentras
evidencia, dilo ("no encontré evidencia sólida sobre..."). Los papers, autores, DOIs
y citas textuales salen solo de fuentes verificadas.

**Terminado cuando:** cada paper elegido tiene su ficha, cada DOI respondió 302, cada
afirmación del informe tiene doble cita, el informe está en `05_deliverables/reports/`
y el grafo está sincronizado.

## Protocolo de selección

- **Validez (filtro eliminatorio):**
  - Peer-reviewed o venue reputado. Los preprints entran solo marcados como
    `peer_reviewed: preprint` y señalados como tales en el informe.
  - Fuente primaria antes que citas de segunda mano.
  - Método y tamaño de muestra anotados.
  - Journals predatorios fuera (señales: aceptación exprés, sin indexación en Scopus,
    Web of Science o DOAJ, editorial dudosa).
  - Retracciones y correcciones revisadas (el título junto a "retraction", y
    Retraction Watch).
- **Impacto y actualidad:** lo más citado Y lo más reciente (últimos ~5 años),
  comparando citas por año de antigüedad para no castigar a los papers nuevos. Los
  seminales más antiguos entran si son clave, marcados `tipo_trabajo: fundacional`.
- **Relevancia:** que responda una pregunta o decisión real del proyecto.
- **Equilibrio:** si hay evidencia en contra o debate abierto, entra también.

**Dónde buscar:** Google Scholar, Semantic Scholar, journals y bases con DOI, con tus
herramientas de búsqueda web. Apoyos gratuitos, sin clave:

- Semantic Scholar (año, citas, venue, DOI y PDF abierto). Sin clave suele responder
  "Too Many Requests"; si pasa, espera y reintenta o sigue con la búsqueda web:
  `https://api.semanticscholar.org/graph/v1/paper/search?query=<tema>&fields=title,year,citationCount,venue,externalIds,openAccessPdf&limit=10`
- Crossref (metadatos oficiales; `is-referenced-by-count` cuenta citas registradas en
  Crossref, que suelen ser menos que en Google Scholar):
  `https://api.crossref.org/works?query=<tema>&rows=10&select=DOI,title,issued,is-referenced-by-count,container-title,type`
- DOI válido (302) o inexistente (404):
  `curl -s -o /dev/null -w "%{http_code}" https://doi.org/<DOI>`

## Pasos

1. **Encuadre.** Aclara la pregunta de investigación y a qué pregunta o decisión del
   proyecto sirve. Revisa qué ya existe: `BRAIN nombres` (fichas en `04_sources/`) y,
   si hace falta, una consulta sobre el tema.
2. **Lista corta.** Aplica el protocolo y presenta una tabla (paper, año, citas,
   venue, por qué entra). Por defecto, entre 5 y 10 papers núcleo. Pide visto bueno
   antes de fichar, salvo que el usuario haya dicho que avances sin parar.
3. **Fichas:** una por paper en `04_sources/literature/apellido-anio-palabra-clave.md`
   con `templates/literature-note.md`. Resume solo lo que leíste: si solo accediste
   al abstract, `leido: solo abstract`. Fuentes web no académicas: ficha en
   `04_sources/web/sitio-tema.md` con `templates/web-source.md`.
4. **Texto completo al grafo, solo para los papers que sustentan una decisión o que
   el usuario pida** (hasta 3 por investigación): la lectura profunda de un paper
   completo cuesta miles de tokens, y la ficha ya lleva lo esencial al grafo.
   - `graphify add <url-del-pdf> --dir 04_sources/literature` (o `--dir 04_sources/web`).
     Sin `--dir` lo guarda en `./raw`, fuera de la estructura.
   - La página de un artículo (por ejemplo `arxiv.org/abs/...`) trae solo el abstract;
     el texto completo viene del link directo al PDF de acceso abierto
     (`arxiv.org/pdf/...`). Solo acceso abierto.
   - El archivo traído recibe un nombre automático (por ejemplo `arxiv_1706_03762.md`):
     enlázalo desde la ficha en `texto_completo`.
   - Si el usuario tiene el PDF, guárdalo junto a la ficha con el mismo nombre base.
5. **Informe** en `05_deliverables/reports/informe-<tema>-<AAAA-MM-DD>.md`: pregunta,
   resumen ejecutivo, hallazgos, evidencia en contra o debates, implicaciones para el
   proyecto (con wikilinks a decisiones y preguntas), límites de la revisión y lista
   de fuentes. Cada afirmación con doble cita:

   ```
   Las pymes abandonan los procesos de alta largos ([Pérez, 2023](https://doi.org/10.xxxx/yyyy); [[perez-2023-onboarding-pymes]]).
   ```
6. Si la investigación responde una pregunta abierta, gradúala (flujo 4).
7. Sincroniza. Si trajiste textos completos o PDFs, lectura profunda al final.
8. **Entrega:** resumen en el chat (máximo 5 viñetas), ruta del informe y lista de
   fichas creadas.
