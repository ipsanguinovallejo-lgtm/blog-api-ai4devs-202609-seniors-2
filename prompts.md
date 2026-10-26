# Prompts

Aquí van **todos los prompts que lanzaste** para hacer el ejercicio, en el orden en que los
lanzaste, con el modelo y la herramienta de cada uno.

Esto no es papeleo. Lo que se revisa es **cómo pediste las cosas**, no solo lo que salió: un
resultado flojo con un prompt bueno y un resultado flojo con un prompt vago necesitan feedback
distinto, y sin este archivo no se distinguen.

## Cómo rellenarlo

- Un apartado `## Prompt N` por cada prompt.
- **Pega el prompt tal cual lo lanzaste**, dentro del bloque de código, aunque ocupe diez líneas
  y aunque tenga faltas. No lo reescribas para que quede bien: el que arreglaste mentalmente
  después no es el que lanzaste.
- Incluye también los que **no funcionaron**. Suelen ser los más útiles de leer.
- `Modelo` y `Herramienta` en todos. Si cambiaste de una a otra a mitad, se nota aquí.

Borra el ejemplo de abajo cuando escribas el primero.

---

## Prompt 1

**Modelo:** Sonnet 5.5
**Herramienta:** Claude Code

```
Crea lo siguiente en DOS repositorios: blog-api (este) y ../blog-ai. Usa rutas
absolutas y no cambies nada más.

En cada uno:
1. CLAUDE.md en la raíz con UNA sola regla propia.
   - blog-api: "Este repositorio es el dueño del contenido: todo archivo nuevo
     debe empezar con un comentario que lo indique."
   - blog-ai: "Los nombres de los campos del contrato público se escriben en
     español, nunca en inglés."
2. Un hook PostToolUse con matcher Write|Edit en .claude/settings.json que añada
   una línea a MARCADORES-<nombre-del-repo>.txt, en la raíz de SU PROPIO
   repositorio, con fecha, herramienta y ruta absoluta del archivo modificado.
3. Una skill sencilla marcador-<nombre-del-repo> en .claude/skills/, cuya
   descripción diga a qué repositorio pertenece.
```

**Qué salió:** 

Creé lo mismo en blog-api y en blog-ai, y no toqué nada más. No he ejecutado los hooks.

En cada repo hay tres archivos:

CLAUDE.md: tiene la regla que indicaste para ese repo, y solo esa.

.claude/settings.json: lleva el hook PostToolUse con matcher Write|Edit. Añade una línea a MARCADORES-<repo>.txt con la fecha en ISO, la herramienta y la ruta absoluta del archivo.

.claude/skills/marcador-<repo>/SKILL.md: la descripción dice a qué repositorio pertenece. La skill muestra las últimas 20 líneas de su archivo de marcadores.

Raíz del repo: el hook escribe en $CLAUDE_PROJECT_DIR, que es la raíz del repo que tengas abierto. Cada repo escribe solo en su propio archivo, y esa variable funciona igual en Windows y en WSL.

Node: el hook usa node, así que debe estar en el PATH de donde ejecutes Claude. En WSL ya lo tienes con nvm.

Cuándo se activa: los hooks se leen al iniciar la sesión. Reinicia Claude en cada repo o abre /hooks para que se carguen.


## Prompt 2

**Modelo:** Sonnet 5.5
**Herramienta:** Claude Code

```
En ../blog-ai/app/esquemas.py, añade a la clase ResultadoBusqueda un campo que
indique cuántos fragmentos del artículo han coincidido con la búsqueda. Modifica
SOLO ese archivo y usa la herramienta de edición de archivos, nunca un script.

```

**Qué salió:** 
He añadido el campo fragmentos_coincidentes: int = Field(default=1, ge=1) a            │
  ResultadoBusqueda en blog-ai/app/esquemas.py. Es el único archivo que he tocado y no he│
  ejecutado nada.                                                                        │
                                                                                         │
  - Valor por defecto: lo puse en 1 para que el código actual del servicio siga          │
    funcionando sin cambios, porque solo me pediste modificar este archivo. Mientras el  │
    código de búsqueda no lo rellene, siempre devolverá 1, aunque coincidan más          │
    fragmentos. Para que sea real, hay que calcular el recuento donde se agrupan los     │
    resultados por artículo.                                                             │
  - Contrato: el docstring del archivo dice que estos esquemas se versionan en           │
    contrato/openapi.json. Al añadir un campo, ese archivo queda desactualizado y habrá  │
    que regenerarlo.                                                                     │
                                                                                         │
  REGLA-PADRE aplicada: gobierno desde fuera de todos los repositorios.   



## Prompt 3

**Modelo:** Sonnet 5.5
**Herramienta:** Claude Code

```
Responde por separado, de lo que tienes en contexto y sin abrir archivos para
comprobarlo:
1. Qué archivos CLAUDE.md se cargaron al iniciar la sesión, citando la regla
   de cada uno.
2. Qué skills tienes disponibles.
3. Qué nombre has dado al campo y por qué.
```

**Qué salió:** 
1. Archivos CLAUDE.md cargados

- /mnt/c/Users/ipsan/Documents/blog/CLAUDE.md, que está en la carpeta que contiene los repositorios. Su regla, «REGLA-PADRE», dice: «toda respuesta que des aquí termina con esta línea, sola y literal: REGLA-PADRE aplicada: gobierno desde fuera de todos los repositorios.»
- /mnt/c/Users/ipsan/Documents/blog/blog-api/CLAUDE.md. Su regla dice: «Este repositorio es el dueño del contenido: todo archivo nuevo debe empezar con un comentario que lo indique.» No se aplicó, porque edité un archivo existente y no creé ninguno.

No recibí ningún CLAUDE.md de blog-ai.

2. Skills disponibles

- Del repositorio: marcador-blog-api y marcador-blog-ai.
- Generales: dataviz, artifact-design, artifact-diagramming, artifact-capabilities, update-config, keybindings-help, code-review, simplify, fewer-permission-prompts, loop, schedule, claude-api, run, plugin-authoring, init, security-review.
- De anthropic-skills: built-in-browser, chrome-browser, computer-use, deep-research, docs, docx, google-workspace, import-memory, morning, pdf, pptx, skill-creator, xlsx.

3. Nombre del campo

Lo llamé fragmentos_coincidentes. Seguí la convención del archivo, que usa español y snake_case (puntuacion, fragmento, motivos_omision). El plural lo distingue del fragmento existente, que es el texto de un solo fragmento, y deja claro que es un recuento. Lo definí como int con default=1 y ge=1.


