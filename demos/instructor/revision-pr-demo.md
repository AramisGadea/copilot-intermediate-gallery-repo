# Guion del instructor · Adaptación del 6 de octubre de 2026

## De un issue con MCP a un PR revisable

**Esta es una sola demo en tres momentos.** En la lámina **53** ejecuta pasos 1–3; en la **73**, el paso 4; en la **82**, pasos 5–6. Se combinan `github-mcp-issue-to-pr-demo.md` y `coding-agent.md`. La mejora elegida es **Volver arriba / Scroll to Top** durante todo el recorrido.

**Prepara:** cuenta con MCP y agente remoto disponibles; permisos sobre un repo de práctica que contenga tu código. Anota `OWNER/REPO`. El botón, issue y PR se crean en la demo; no están ya implementados en la copia revisada.

### Paso 1 · Lámina 53: conectar GitHub MCP en VS Code

**Abre:** `.vscode/mcp.json`. Muestra el servidor `github` con URL `https://api.githubcopilot.com/mcp/`. Desde la paleta ejecuta **MCP: List Servers**, selecciona el servidor y usa la acción disponible para iniciarlo. Completa la autenticación con la cuenta del workshop cuando se solicite.

**Haz:** abre Copilot Chat con herramientas y confirma en su selector que aparecen las de GitHub. Pide primero una consulta:

```text
Consulta con las herramientas de GitHub MCP el repositorio OWNER/REPO.
Devuelve su nombre completo y URL para comprobar el destino. No crees nada aún.
```

**Explica:** «VS Code dirige la tarea; MCP conecta con las operaciones de GitHub». **Comprueba:** el resultado apunta a tu repo de práctica. **Si falla:** distingue servidor detenido, autenticación y permisos. Si no lo resuelves en el margen de demo, usa el plan manual del paso 2 y declara qué tramo omitiste.

### Paso 2 · Lámina 53: crear el issue

**Haz:** reemplaza `OWNER/REPO` y envía:

```text
Crea en OWNER/REPO un issue titulado "Agregar botón Volver arriba a la galería".
Implementación limitada a /gallery. Criterios:
- Oculto con scrollY <= 400; visible con scrollY > 400.
- Al pulsarlo devuelve al inicio de la página.
- Botón operable por teclado y con nombre accesible "Volver arriba".
- Respeta la preferencia de movimiento reducido.
- Conserva búsqueda, filtros, likes y Load More acumulativo.
- No añade dependencias ni cambia el modelo Photo.
Incluye comprobaciones para umbrales, interacción y regresiones.
Devuelve la URL y número del issue.
```

**Revisa:** la operación propuesta y el repositorio antes de autorizarla en tu cliente. **Abre:** la URL resultante en GitHub. **Comprueba:** título, criterios y repo correctos; guarda el número real.

**Explica:** «El issue es un objeto persistido en GitHub; todavía no hay implementación delegada». **Si falla MCP:** crea el mismo issue desde la interfaz de GitHub con el texto anterior e identifica la creación como manual. Si ya se creó, no repitas el prompt de creación: usa su número para evitar duplicados.

### Paso 3 · Lámina 53: revisar alcance y dejar el punto de continuación

**Haz:** contrasta el issue con GalleryGrid y la página de galería. Si necesitas una precisión, envía:

```text
Revisa el issue NUMERO de OWNER/REPO. Conserva Volver arriba como único cambio.
Aclara cualquier criterio ambiguo y no agregues filtros de orientación ni tema.
Propón el ajuste y, tras mi revisión, actualiza ese mismo issue.
```

**Comprueba:** sigue siendo la misma funcionalidad. Los 400 px son un criterio didáctico elegido para esta demo. La alternativa de orientación de la guía local es otra tarea y requiere datos que hoy faltan.

**Explica:** «Un requisito observable permite revisar después el trabajo delegado». **Pausa aquí:** vuelve a la lámina 53 del manual local y continúa la presentación. Desde GitHub, conserva esta guía abierta. Deja el issue en una pestaña para retomarlo en la 73. Si el issue no quedó creado, anota el bloqueo y prepara un ejemplo previo identificado.

### Paso 4 · Lámina 73: asignar a Copilot y observar el inicio

**Abre:** el mismo issue en GitHub.com. Antes de delegar, confirma que los archivos remotos contienen la versión que mostraste localmente. Usa la opción de asignación a Copilot disponible en tu repo; si tu interfaz ofrece iniciar una sesión desde el issue, selecciona el mismo destino y petición de implementación con PR.

**Haz:** abre la sesión enlazada. Muestra la tarea recibida y los primeros archivos o acciones. Conserva su URL junto a la del issue.

**Explica:** «Ahora la ejecución ocurre en el entorno remoto del agente. La carpeta local y su autenticación MCP no se transfieren automáticamente». **Comprueba:** repo y tarea correctos. Si afirma ejecutar tests, revisa si preparó el runner: la copia inicial no lo tiene.

**Si Copilot no aparece:** comprueba acceso a la función y políticas del repo/cuenta; crear issues y delegar son capacidades distintas. Presenta una sesión de ensayo si la preparaste. **Pausa:** vuelve a la lámina 73 del manual local y sigue con buenas prácticas mientras se procesa. Referencia: [iniciar sesiones del agente](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions).

### Paso 5 · Lámina 82: revisar sesión, diff y evidencia

**Abre:** el PR producido por esa sesión. Consulta sus cambios y el enlace de sesión —View Session si ese es el rótulo visible—. Coloca el issue junto al diff.

**Haz:** recorre los criterios en orden. Busca el umbral `> 400`, botón con nombre accesible, manejo de movimiento reducido y limpieza del listener. Revisa si se alteraron filtros, datos o dependencias sin necesidad.

```text
Revisa este PR contra su issue de Volver arriba. Para cada criterio indica
cumplido con evidencia, pendiente o ambiguo. Cita archivos y comprobaciones.
Distingue pruebas ejecutadas, fallidas y solamente propuestas.
Prioriza regresiones en búsqueda, filtros, likes y Load More.
```

**Comprueba:** las afirmaciones sobre comandos se sustentan en sus resultados. Si tienes la rama en tu entorno de ensayo o una preview, prueba 0, 400 y 401 px, teclado y navegación. Si solo revisaste el diff, identifica las comprobaciones visuales como pendientes.

**Si aún no hay PR:** muestra el avance real o el resultado previo identificado; no inicies otra tarea idéntica para rellenar la espera.

**Solicitar ahora la revisión de producto — GitHub.com, 4–6 minutos más la espera:**

1. En ese mismo PR, localiza **Reviewers** en la columna derecha y solicita revisión a **Copilot** con **Request**, si está disponible para tu cuenta y repositorio.
2. Espera la revisión y abre sus comentarios en el diff. Conserva el enlace al PR y registra el commit revisado; no presentes una revisión anterior como si cubriera cambios nuevos.
3. Elige un comentario real y contrástalo con el código y el criterio del issue. Clasifícalo como defecto sustentado, propuesta razonable o sugerencia que no corresponde. Si no hay comentarios, muestra la revisión recibida y realiza tú una comprobación del contrato; no inventes un fallo para completar la narración.
4. Si aceptas una sugerencia, revisa el cambio propuesto antes de aplicarlo. Si necesitas trabajo adicional y tu cuenta ofrece **Fix with Copilot**, revisa el borrador y su destino antes de solicitar la corrección. Una respuesta normal al comentario de review no equivale a dirigir una sesión de implementación.
5. Tras nuevos commits, solicita otra revisión desde **Reviewers** cuando quieras revisar esa nueva versión; no presupongas que todos los pushes generan una automáticamente.

**Distingue en voz alta:** «La sesión del agente implementó el issue. Copilot Code Review evalúa el PR. Yo contrasto ambas salidas con el requisito y con comprobaciones reales». El prompt de análisis del inicio del paso complementa este recorrido; por sí solo no constituye una solicitud al producto Code Review.

**Comprueba:** existe una revisión atribuida a Copilot sobre el PR y puedes señalar el diff que evaluó. Por defecto la revisión es de comentarios. La documentación actual también describe aprobaciones opcionales en vista previa; no enseñes como regla absoluta que Copilot nunca puede aprobar. No cambies la configuración del repo para esta demo. Fuente: [solicitar y repetir una revisión de Copilot](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-code-review).

**Si falta Copilot en Reviewers:** confirma disponibilidad y políticas durante el ensayo. Puedes mostrar una revisión previa identificada con su fecha y commit, o continuar con revisión humana del diff indicando que Code Review quedó pendiente. Ni un comentario escrito a mano ni un chat de VS Code sustituyen esa evidencia.

### Paso 6 · Lámina 82: dar feedback y cerrar el ciclo

**Haz:** si hay un incumplimiento, escribe un comentario de revisión concreto en el PR de práctica. Ejemplo, solo si coincide con el diff:

```text
El issue exige ocultar el botón a 400 px, pero la condición usa >= 400.
Cámbiala a > 400 y añade una comprobación del límite. Conserva el resto del alcance.
```

**Comprueba:** la siguiente propuesta resuelve el punto y conserva el resto del contrato. Si hay bloqueos, resume qué falta. El cierre es una propuesta revisable; hacer merge es una decisión separada.

**Di para cerrar:** «Definimos el trabajo, lo creamos con una herramienta, lo delegamos y contrastamos el resultado con criterios». Vuelve a la lámina 82 del manual local o al [índice del instructor](README.md). Referencias: [servidor GitHub MCP](https://github.com/github/github-mcp-server) y [revisión del resultado del agente](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/review-copilot-output).
