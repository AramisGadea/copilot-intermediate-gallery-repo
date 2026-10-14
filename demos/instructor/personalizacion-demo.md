# Guion del instructor · Adaptación del 6 de octubre de 2026

## Usar prompts, instrucciones y Plan Agent

**Objetivo:** ejecutar una personalización y reconocer su efecto. **Ya existe:** `.github/prompts`, `.github/copilot-instructions.md` y `.github/agents/Plan.agent.md`. El tramo MCP continúa en la Demo 6 del manual local; en GitHub, usa [`../github-mcp-issue-to-pr-demo.md`](../github-mcp-issue-to-pr-demo.md) para no crear dos issues diferentes.

### Paso 1 · Identificar sesión, modelo y consumo

**Abre:** Chat de VS Code. Muestra el modelo y el tipo de sesión seleccionados. Abre el indicador de uso de Copilot que ofrece tu instalación o la configuración de uso de la cuenta.

**Explica:** «Esta es la cuenta y la experiencia con las que ejecutamos el ejercicio». La guía local habla de Premium Requests; contextualiza lo que muestre tu cuenta con la lámina 28 del manual local, que distingue ese régimen de AI Credits y fecha sus cifras.

**Comprueba:** lees la unidad y el periodo que la interfaz muestra, sin atribuir el mismo límite a todas las cuentas. **Si el panel no aparece:** continúa con la personalización y usa la fuente de la lámina para la explicación; no deduzcas consumo desde la longitud de la respuesta.

### Paso 2 · Ejecutar un prompt del repositorio

**Abre:** `.github/prompts/generate-mock-photo-data.prompt.md` y `src/lib/mock-photo-data.ts`. Revisa que el prompt pide entradas compatibles con el tipo existente.

**Haz:** en una sesión **Local** compatible con prompt files, escribe `/` y selecciona `generate-mock-photo-data`; añade `3` y ejecuta. El texto equivalente es:

```text
/generate-mock-photo-data 3
```

**Comprueba:** propone tres entradas con IDs únicos y campos del modelo. Si produce texto sin editar el archivo, pide aplicar solo esas entradas después de revisarlas. Las URL placeholder no generan imágenes reales. **Explica:** «El archivo reutiliza una tarea; sus resultados siguen necesitando revisión».

**Si no aparece:** comprueba el tipo de sesión. La documentación actual distingue prompt files de Local y las sesiones Agent Host que no los utilizan. Como alternativa, adjunta el archivo y solicita seguir sus instrucciones para tres entradas; di que estás utilizando contexto explícito, no ejecutando el comando reutilizable. Referencia: [prompt files](https://code.visualstudio.com/docs/agent-customization/prompt-files).

### Paso 3 · Seleccionar el agente personalizado del repo

**Abre:** `.github/agents/Plan.agent.md`. Muestra su `name: Plan Agent`, herramientas e instrucciones. El campo `focusArea` del ejemplo no está documentado; la especialización debe expresarse en el cuerpo o la descripción.

**Haz:** selecciona **Plan Agent** en la lista de agentes descubiertos del workspace y envía:

```text
Planifica una página para crear galerías. Usa los patrones actuales del repo.
Entrega overview, requisitos, pasos de implementación y pruebas.
Distingue lo que se puede simular con mocks de lo que requeriría un backend.
En este ejercicio produce solo el plan.
```

**Comprueba:** el resultado es un plan; no has seleccionado por error el rol Plan integrado. **Si no aparece:** abre la configuración/diagnóstico de agentes, comprueba la raíz del workspace y el archivo. Puedes explicar el archivo sin afirmar que se activó. Referencia: [agentes personalizados](https://code.visualstudio.com/docs/agent-customization/custom-agents).

### Paso 4 · Relacionar instrucciones y resultado

**Abre:** `.github/copilot-instructions.md`, ruta correcta de tu copia. Muestra una regla concreta de componentes reutilizables y pide:

```text
Relaciona el plan con tres reglas de .github/copilot-instructions.md.
Indica qué componentes existentes reutilizarías y dónde están.
Si encuentras contradicciones entre reglas y código, descríbelas antes de editar.
```

**Comprueba:** menciona archivos reales, como los componentes de layout. **Explica:** «La instrucción general da contexto del proyecto; el agente define el rol; el prompt describe esta tarea». **Si falla:** adjunta el archivo y examina descubrimiento de instrucciones antes de agregar más reglas.

### Paso 5 · Crear instrucciones específicas para las pruebas

**Duración:** 5–7 minutos. **Ya preparado:** `demos/instructor/templates/gallery-tests.instructions.md.example`. **Se crea durante la práctica:** `.github/instructions/gallery-tests.instructions.md`. La plantilla termina en `.example` para que puedas enseñar su activación de forma explícita; todavía no es una instrucción descubierta en la ubicación del proyecto.

**Abre:** la plantilla y el Explorador de VS Code. Crea la carpeta `.github/instructions` si falta y un archivo `gallery-tests.instructions.md`; copia el contenido completo de la plantilla y guárdalo. Si el archivo ya existe de un ensayo anterior, revísalo y reutilízalo. El recorrido no necesita reemplazar tus personalizaciones.

**Muestra:** `applyTo` y sus patrones de archivos de pruebas. Distingue reglas generales de proyecto de reglas para una tarea concreta. El archivo específico complementa las instrucciones generales; resuelve cualquier contradicción en los textos en vez de asumir que el archivo más específico siempre gana.

**Haz:** abre las personalizaciones de Chat y comprueba que aparece la nueva instrucción para la sesión seleccionada. En una sesión nueva con capacidad de editar, pide:

```text
Prepara src/components/gallery/GalleryGrid.test.tsx para comprobar Load More
acumulativo y la combinación de tags y búsqueda. Antes de crear archivos,
inspecciona package.json y la configuración de pruebas. Si falta el runner,
indica qué falta y entrega los casos propuestos sin instalar ni crear tests.
Si ya existe la configuración de la demo de testing, crea solo este archivo
usando esa configuración. Separa lo propuesto de lo ejecutado.
```

**Comprueba:** en el proyecto inicial se detiene al detectar que no hay runner. Esa respuesta es un resultado válido: la instrucción evitó presentar un comando inexistente como listo. Si ya hiciste la Demo 8, revisa el archivo generado y sus imports antes de ejecutarlo. Busca las referencias de instrucciones o el diagnóstico de la interacción para comprobar si se incorporó el archivo. Que aparezca en el catálogo no demuestra que influyera en esta respuesta.

**Si no se carga:** revisa raíz, patrón, nombre y tipo de sesión. Puedes adjuntar el archivo manualmente para continuar, indicando que esa variante demuestra uso explícito del contenido y deja pendiente su descubrimiento automático. **Explica:** «Una regla útil cambia una decisión verificable: revisar el runner antes de fabricar una prueba».

#### Contenido completo para crear la instrucción

Copia este contenido en la ruta indicada en el paso 5; coincide con la plantilla del repo.

```markdown
---
name: Reglas para pruebas de la galería
description: Usar al crear o modificar pruebas de GalleryGrid y GalleryPage en este workshop.
applyTo: "**/*.test.tsx,**/*.test.ts"
---
# Pruebas de la galería

- Inspecciona package.json y la configuración antes de proponer comandos. Si no existe runner o script test, indícalo y describe la preparación necesaria.
- Usa el runner ya configurado; en el recorrido de Jest del workshop, sigue esa configuración sin introducir un segundo runner.
- Prueba comportamiento observable con React Testing Library; prioriza roles y nombres accesibles. Si falta un nombre, informa la limitación.
- GalleryGrid importa mockPhotos: usa un mock del módulo si necesitas datos deterministas; no inventes una prop photos.
- Conserva tags OR, tags AND búsqueda, búsqueda sin distinguir mayúsculas, likes por ID y Load More acumulativo.
- Prueba el reinicio de currentPage en GalleryPage, donde vive ese estado; no lo atribuyas al hijo.
- Separa pruebas propuestas, creadas y ejecutadas. Aporta el comando y el resultado real solo cuando lo ejecutes.
- Evita snapshots extensos y selectores basados únicamente en clases de Tailwind.
```

### Paso 6 · Inspeccionar la skill preparada para tu galería

**Duración:** 3 minutos. **Abre:** `.github/skills/revisar-galeria/SKILL.md` y `references/contrato-galeria.md` dentro de esa misma carpeta. Ambos se agregaron en esta adaptación. `SKILL.md` es singular; `SKILLS.md` no es el archivo de entrada de este ejemplo.

**Muestra tres partes:** nombre y propósito en el encabezado, procedimiento en el cuerpo y contrato en el recurso enlazado. El contrato describe tu código: OR entre tags, AND con búsqueda, likes por ID y carga acumulativa. Separa esos criterios didácticos de la documentación oficial sobre el formato.

**Explica:** «Las instrucciones establecen convenciones; esta skill reúne un procedimiento de revisión y el material que necesita para aplicarlo». El agente personalizado de planificación se elige por su rol; la skill se invoca para esta tarea. Son mecanismos complementarios y no requieren que un SKILL.md se convierta en un agente de la lista.

**Comprueba:** `name: revisar-galeria` coincide con el nombre de la carpeta y el recurso relativo existe. El encabezado lleva `disable-model-invocation: true`: elegimos una invocación manual para controlar el momento de la demo. Fuente de formato e invocación: [skills de VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills).

#### Contenido de la skill preparada

Estos dos archivos ya están en el repo. Se incluyen aquí para consultar el procedimiento incluso si guardas únicamente el HTML.

**SKILL.md:**

```markdown
---
name: revisar-galeria
description: Revisa cambios de la galería de este workshop para detectar regresiones en filtros, búsqueda, likes y Load More. Úsala al revisar GalleryGrid o GalleryPage y sus componentes extraídos.
disable-model-invocation: true
---
# Revisar el contrato de la galería

Produce una revisión basada en el código disponible. No edites la aplicación ni instales dependencias durante esta revisión.

1. Lee `src/components/gallery/GalleryGrid.tsx`, `src/app/gallery/page.tsx` y `src/lib/mock-photo-data.ts`. Si se extrajo PhotoCard, inspecciona su implementación y punto de uso.
2. Determina qué revisar: el diff indicado por el usuario o, si no existe diff, el estado actual. No inventes una rama base ni afirmes comparar cambios en una carpeta sin historial Git.
3. Lee [el contrato y los casos](references/contrato-galeria.md). Contrasta cada comportamiento con la implementación; señala los cambios deliberados de contrato como decisiones que requieren confirmación del instructor.
4. Sigue el flujo padre → props → cálculo → render → interacción. Verifica dónde viven los likes, dónde se reinicia currentPage y qué parte es solo simulada.
5. Separa defectos sustentados, riesgos que necesitan ejecución y preferencias. Para cada hallazgo aporta archivo, fragmento o línea real, consecuencia y corrección mínima propuesta.
6. Describe las comprobaciones manuales pendientes. Solo ejecuta comandos o pruebas si el usuario lo solicita y existe la configuración necesaria. Nunca declares un resultado exitoso que no observaste.

## Formato de salida

- Alcance: archivos leídos y base de comparación, o «revisión del estado actual».
- Tabla: criterio | evidencia en código | resultado (conservado, cambiado, no comprobado).
- Hallazgos priorizados con evidencia y propuesta mínima.
- Comprobaciones ejecutadas y resultados; después, comprobaciones pendientes.
- Conclusión acotada: qué se puede sostener con la evidencia disponible.

Si faltan archivos, identifica cuáles y limita la conclusión. Usa español. Una ausencia de hallazgos no demuestra ausencia de errores.
```

**references/contrato-galeria.md:**

```markdown
# Contrato didáctico de la galería

Punto de partida: copia local proporcionada para el workshop, revisada el 6 de octubre de 2026. Este contrato describe esa implementación, no una especificación oficial de GitHub ni el resultado de pruebas ejecutadas.

| Criterio | Comportamiento de partida | Caso para comprobar |
|---|---|---|
| Tags | OR entre tags seleccionados mediante `some`; sin tags no se restringe por tag | Seleccionar dos tags; una foto que coincide con cualquiera puede permanecer |
| Búsqueda | Coincidencia parcial sin distinguir mayúsculas en título, tags o fotógrafo | Buscar un texto existente con mayúsculas y minúsculas |
| Combinación | Resultado de tags AND resultado de búsqueda | Un texto coincidente no debe anular el filtro de tags |
| Load More | Acumula desde índice 0 hasta `currentPage * limit` | Caso hipotético: 14 coincidencias, limit=6, páginas 1/2/3 muestran 6/12/14 |
| Cambio de filtro | GalleryPage reinicia currentPage a 1 | Cargar más y luego cambiar búsqueda o tag |
| Likes | Set local por ID; segundo clic deshace; mockPhotos no se muta | Marcar, desmarcar, filtrar y volver mientras el padre siga montado |
| Datos | Photo y mockPhotos se importan de src/lib/mock-photo-data.ts | No asumir prop photos ni API de fotos que no existen |
| Límites | Detalles y carga son simulados; no hay persistencia de likes | No prometer conservación después de recargar la página |

Para cantidades reales usa el dataset que esté abierto. Las 14 coincidencias del ejemplo son hipotéticas; no las presentes como un conteo medido. Si el instructor agregó mocks, recalcula el total.

Una extracción de PhotoCard debe conservar callbacks, estado en el padre, animación y uso del tipo Photo existente. Las mejoras de accesibilidad son propuestas distintas de una refactorización puramente estructural.
```

### Paso 7 · Invocar la skill y evaluar su revisión

**Duración:** 4–6 minutos. **Abre:** una sesión nueva con un agente que pueda leer el proyecto. Escribe `/`, localiza `revisar-galeria` y selecciónala. Ejecuta:

```text
/revisar-galeria Revisa el estado actual de GalleryGrid y GalleryPage.
Entrega la tabla de criterios con evidencia. No edites ni ejecutes pruebas.
Distingue lo observado en el código de lo que necesita comprobarse en la app.
```

**Comprueba:** la salida identifica archivos leídos y diferencia evidencia estática de ejecución. Debe reconocer que GalleryGrid importa mocks, que la página reinicia currentPage y que Load More acumula. No debe reportar una suite aprobada. Revisa además la actividad de lectura del recurso enlazado; que el agente diga «usé la skill» por sí solo es evidencia insuficiente.

**Si hiciste PhotoCard:** cambia el alcance a «revisa el diff de extracción de PhotoCard» y proporciona un diff real de tu rama. En la carpeta exportada sin `.git`, usa revisión del estado actual o adjunta el diff de tu editor; no pidas comparar contra un main inexistente.

**Si el comando no aparece:** revisa la ubicación y el catálogo de personalizaciones; abre una sesión nueva después de guardar. Como contingencia, adjunta SKILL.md y su contrato y pide seguir ese procedimiento. Declara que ejecutaste el procedimiento mediante adjuntos y que falta comprobar la invocación nativa.

**Di al cerrar:** «El valor de este paquete está en repetir los mismos criterios y exigir evidencia. El resultado sigue siendo una revisión que debemos juzgar». Si no hay hallazgos, recorre un criterio con el grupo y muestra cómo el código lo satisface.

### Paso 8 · Ubicar AGENTS.md sin confundirlo con un agente

**Duración:** 2 minutos; ampliación opcional. **Abre:** `demos/instructor/templates/AGENTS.md.example`, el archivo `.github/copilot-instructions.md` existente y `Plan.agent.md`.

**Explica sobre los archivos reales:**

| Archivo | Pregunta que resuelve en esta práctica |
|---|---|
| copilot-instructions.md | ¿Qué convenciones generales debe conocer Copilot sobre el proyecto? |
| gallery-tests.instructions.md | ¿Qué reglas adicionales aplican al preparar estas pruebas? |
| Plan.agent.md | ¿Qué rol, instrucciones y herramientas tendrá el agente que selecciono? |
| revisar-galeria/SKILL.md | ¿Qué procedimiento reutilizable invoco para revisar la galería? |
| AGENTS.md | ¿Qué guía compartida quiero ofrecer a agentes compatibles? |

**Haz:** compara la plantilla con las reglas existentes. Deja el ejemplo como referencia si no necesitas compartir instrucciones entre agentes. Si decides activarlo en una copia de práctica, consolida las reglas y guarda `AGENTS.md` en la raíz; comprueba su descubrimiento con la sesión elegida. No basta con renombrar Plan.agent.md: tienen funciones diferentes.

**Comprueba:** el estado entregado conserva el ejemplo sin un AGENTS.md activo nuevo. **Si hay contradicciones:** registra las reglas en conflicto y define una redacción coherente antes de activarlo. Fuente para instrucciones y AGENTS.md: [instrucciones de VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions).

#### Ejemplo opcional de guía compartida

Este contenido se entrega como `AGENTS.md.example`; su activación es el ejercicio opcional descrito arriba.

```markdown
# Guía compartida de la galería

Ejemplo opcional para agentes compatibles. Antes de activarlo en la raíz, consolidar las reglas compartidas con .github/copilot-instructions.md y resolver contradicciones; no depender de una precedencia universal.

- Esta aplicación de workshop usa Next.js, TypeScript y datos mock. Lee package.json para conocer versiones y comandos reales.
- Inspecciona GalleryGrid, GalleryPage y mock-photo-data antes de modificar el flujo de fotos.
- Conserva filtros, búsqueda, likes y Load More acumulativo, salvo que la tarea pida cambiar ese contrato.
- Distingue la subida simulada de una integración remota real.
- Describe qué cambió, qué comprobaste y qué queda pendiente. No presentes pruebas propuestas como ejecutadas.
- Mantén los cambios dentro de la tarea acordada y comunica contradicciones entre instrucciones y código.
```

### Paso 9 · Cerrar o continuar con MCP

**Haz:** revisa los datos agregados y el plan. Decide qué resultados conservar en tu rama de ensayo. Para el recorrido principal continúa con [crear el issue mediante MCP](../github-mcp-issue-to-pr-demo.md), después de preparar su destino.

**Variantes opcionales del repo:** comparar dos modelos con la misma tarea y contexto en sesiones nuevas; ejecutar `generate-new-ui` sobre la tabla de admin; generar un prompt de tests para `src/components/ui/cards/FeatureCard.tsx`. Elige una variante y revisa sus resultados antes de sumar otra.

**Cierre:** vuelve a la lámina 53 del manual local, o a las láminas 43–46 para explicar cada formato. En GitHub, continúa con la [demo de MCP](../github-mcp-issue-to-pr-demo.md).
