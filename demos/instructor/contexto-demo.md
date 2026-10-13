# Guion del instructor · Adaptación del 6 de octubre de 2026

## Inspeccionar contexto y conservar evidencia

**Objetivo:** investigar una respuesta utilizando evidencia de la sesión. **Punto de partida:** Chat de VS Code con archivos del workshop. La guía local usa nombres antiguos de comandos; abajo se indican los actuales documentados. Los registros muestran interacciones, no una lectura exhaustiva del razonamiento interno del modelo.

### Paso 1 · Comparar dos peticiones con el mismo objetivo

**Duración:** 4–5 minutos. **Abre:** una sesión nueva en VS Code, con la misma carpeta, modelo y modo que usarás en la segunda consulta. Ten a mano GalleryGrid, GalleryPage y mock-photo-data. La finalidad es evaluar qué información permite juzgar una respuesta.

**Haz, consulta A:** envía una petición general, sin adjuntar archivos manualmente:

```text
Sugiere una mejora para Load More en esta galería. Explica el plan sin editar.
```

Guarda la respuesta. Anota qué archivos recuperó por su cuenta y qué supuso. Una sesión nueva puede recibir instrucciones del proyecto, contexto del editor o resultados de herramientas; «sin adjuntos» no significa «sin contexto». Si responde bien, úsalo como evidencia de recuperación útil, no como un problema para la demo.

**Haz, consulta B:** abre otra sesión con el mismo modelo y modo. Adjunta explícitamente `src/components/gallery/GalleryGrid.tsx`, `src/app/gallery/page.tsx` y `src/lib/mock-photo-data.ts`. Envía:

```text
Analiza Load More usando los tres archivos adjuntos. Propón mostrar siempre
un contador “Mostrando X de Y”, incluso después de cargar todas las fotos.
Conserva el slice acumulativo desde 0, tags OR, tags AND búsqueda y likes.
Y debe ser el total filtrado, no el total del dataset; X, las fotos visibles.
Con cero resultados, el contador debe ser 0 de 0. No cambies dependencias,
props públicas ni datos. Entrega ubicación del cambio, criterios y casos
manuales; cita evidencia del comportamiento actual. Todavía no edites.
```

**Comprueba con esta tabla:**

| Pregunta | Qué buscar en cada respuesta |
|---|---|
| ¿Leyó el código? | Referencias reales al cálculo de filteredPhotos y displayedPhotos |
| ¿Entendió Load More? | Conserva tarjetas anteriores al aumentar currentPage |
| ¿Delimitó la mejora? | Contador con total filtrado, independiente de que queden más fotos |
| ¿Se puede evaluar el plan? | Incluye cero resultados, carga parcial y carga completa |
| ¿Inventó algo? | No presupone una API ni una prop photos |

**Explica:** «Los archivos aportan evidencia; el contrato define qué conservar; los criterios permiten decidir si la propuesta sirve». La comparación cambia contexto y precisión de la tarea a la vez: es una demostración didáctica, no un experimento que pruebe cuánto mejora cada factor por separado.

**Si falla:** pide localizar el cálculo del contador existente. En la copia de partida su presentación está dentro de la condición de Load More; eso justifica la mejora elegida. Si tu versión ya muestra el contador siempre, registra que el objetivo ya está cumplido y analiza su contrato. No fuerces un cambio redundante. Fuente sobre incorporación de archivos: [contexto de Chat en VS Code](https://code.visualstudio.com/docs/chat/copilot-chat-context).

### Paso 2 · Crear una interacción pequeña y rastreable

**Abre:** una sesión nueva, adjunta GalleryGrid y envía:

```text
¿Los tags seleccionados se combinan con OR o con AND? Cita el fragmento
que lo demuestra y explica cómo se combina ese resultado con searchQuery.
No edites archivos.
```

**Comprueba:** cita `some` para tags y `matchesTags && matchesSearch` para la combinación. **Explica:** «Ahora tenemos una pregunta cuya respuesta podemos contrastar». **Si falla:** adjunta explícitamente el archivo y repite; conserva ambas respuestas para comparar contexto.

### Paso 3 · Abrir el diagnóstico de esa interacción

**Haz:** abre la paleta con Cmd+Shift+P o Ctrl+Shift+P. Ejecuta **Developer: Show Chat Debug View**, o usa **Show Chat Debug View** en el menú de Chat. Localiza la interacción del paso anterior y examina el contexto y los resultados de herramientas disponibles.

**Explica:** «Buscamos qué evidencia llegó a la petición y qué devolvieron las herramientas». **Comprueba:** aparece el archivo o fragmento relevante. **Si falla:** confirma la sesión seleccionada; agrega el archivo como contexto y genera una interacción nueva. No inventes un diagnóstico sobre datos que la vista no expone.

### Paso 4 · Conservar un registro que puedas volver a abrir

**Haz:** abre **Developer: Open Agent Debug Logs**. Selecciona la sesión y utiliza Export para guardar su JSON. Utiliza Import en esa misma vista para volver a inspeccionarlo.

**Comprueba:** se muestran los eventos de la sesión exportada. **Explica:** «Este JSON conserva un registro de diagnóstico; no sustituye el código ni garantiza reproducir una generación». **Si falla:** captura la evidencia visible e identifica qué parte no pudo exportarse.

La guía original también utiliza `Chat: Export Chat...` y `Chat: Import Chat...` para conversaciones. Si esos comandos aparecen en tu instalación, puedes demostrar el transcript por separado. No intercambies un archivo de conversación con un JSON de Agent Debug Logs.

Fuente de los pasos 3 y 4: [diagnóstico oficial de VS Code](https://code.visualstudio.com/docs/agents/agent-troubleshooting/chat-debug-view).

### Paso 5 · Variante de colaboración en GitHub.com

**Abre:** el repositorio de práctica en GitHub.com y su pestaña **Agents**. Si dispones de una sesión local sincronizada de este repo, abre su menú y las opciones de compartir. Selecciona compartirla para consulta de colaboradores cuando ese sea el alcance que quieres demostrar.

**Comprueba:** el destinatario puede consultar los prompts, respuestas y cambios de la sesión compartida, sin dirigirla. **Explica:** «Estamos compartiendo una sesión de agente vinculada al repositorio; esto es distinto del archivo de logs y del chat local». Las sesiones de agente en la nube ya son visibles para quienes tienen acceso al repo; no presentes ese acceso como una concesión nueva del enlace.

**Si no tienes una sesión sincronizada o no aparece la opción:** conserva esta parte como explicación y cierra con la evidencia del paso 4. La guía local habla de Share y Manage shared conversations en una conversación de GitHub.com. No presupongas que esa interfaz histórica aparece en tu cuenta ni que equivale al catálogo de sesiones actual.

**Cierre:** vuelve a la lámina 34 del manual local y resume contexto → respuesta → evidencia. Si consultas desde GitHub, continúa con el [índice del instructor](README.md). Referencia de esta variante: [compartir y administrar sesiones de agente](https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/manage-and-track-agents).
