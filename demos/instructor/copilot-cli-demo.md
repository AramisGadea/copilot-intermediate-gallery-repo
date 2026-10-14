# Guion del instructor · Adaptación del 6 de octubre de 2026

## Investigar y planificar desde Copilot CLI

**Entorno:** terminal, Copilot CLI. **Láminas:** 84–87. **Duración principal:** 5–7 minutos, con CLI instalado y autenticado antes del workshop. **Ampliación opcional:** 5–8 minutos para implementar y comprobar el contador. **Objetivo:** mostrar cómo el agente usa archivos locales para investigar una tarea y proponer un cambio acotado. Los prompts y criterios siguientes se diseñaron para tu galería; esta guía se agrega como `demos/instructor/copilot-cli-demo.md`.

**Punto de partida:** misma copia de la galería que usaste en VS Code. No hace falta un repositorio remoto para leer estos archivos; para enseñar un diff de Git utiliza un clon con historial. La mejora será mostrar «Mostrando X de Y» de forma permanente, conservando el comportamiento de Load More. Si ya la implementaste en otro ejercicio, usa una copia de ensayo con el punto de partida o revisa la solución existente.

### Paso 1 · Preparar CLI antes de la sesión

**Abre:** una terminal. Comprueba si ya tienes la aplicación:

```sh
copilot --version
```

**Si no está instalada:** en macOS con Homebrew preparado, la instalación oficial ofrece:

```sh
brew install --cask copilot-cli
```

Como alternativa, la instalación mediante npm requiere **Node.js 22 o posterior**:

```sh
node --version
npm install -g @github/copilot
```

El devcontainer y el recorrido local usan Node 22. Aun así, prepara CLI antes
de presentar y elige una sola vía de instalación. Fuente:
[instalación oficial de Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli).

**Comprueba:** anota la versión real y abre una sesión de ensayo para completar el inicio de sesión. **Si falla:** resuelve la instalación o políticas antes de impartir; conserva una transcripción de un ensayo real. No cambies silenciosamente a `gh copilot`: este guion utiliza la aplicación `copilot`.

### Paso 2 · Abrir la sesión en la carpeta de la galería

**Haz:** abre la carpeta donde clonaste el repositorio:

```sh
cd /ruta/al/copilot-intermediate-gallery-repo
copilot
```

Si usas un clon de ensayo, sustituye la ruta por la de ese clon. Comprueba la carpeta que muestra la sesión. Responde al diálogo de confianza para esta carpeta según el alcance del ejercicio; si necesitas autenticarte, sigue `/login`. Abre `/help` para ver los comandos disponibles en tu versión.

**Explica:** «El directorio de trabajo determina qué proyecto estamos investigando. La cuenta y las políticas determinan qué funciones podemos utilizar». **Comprueba:** el agente puede localizar `src/components/gallery/GalleryGrid.tsx`. **Si falla:** corrige la carpeta antes de enviar prompts; una respuesta sobre otro proyecto no sirve como resultado de esta demo.

### Paso 3 · Investigar Load More con archivos explícitos

**Haz:** envía dentro de Copilot CLI:

```text
Explica cómo funciona Load More en
@src/components/gallery/GalleryGrid.tsx y @src/app/gallery/page.tsx.
Consulta @src/lib/mock-photo-data.ts cuando lo necesites.
Cita dónde se acumulan las fotos y dónde se reinicia currentPage.
Con 14 coincidencias hipotéticas, limit=6 y currentPage=2, ¿cuántas fotos
se muestran? No edites archivos ni ejecutes la aplicación.
```

**Abre:** la actividad que muestre la lectura de los archivos y el fragmento citado. **Comprueba:** 12 fotos en ese caso hipotético, acumuladas desde cero; el reset del estado lo hace la página al cambiar filtros. No lo confundas con «traer la segunda página desde una API»: los datos son mocks importados.

**Explica:** «Podemos seguir el mismo contrato del código desde otro cliente. El terminal no elimina la necesidad de aportar contexto y revisar la respuesta». **Si falla:** señala el archivo y pide recalcular desde `slice(startIndex, endIndex)`; no avances a implementación con un diagnóstico incorrecto.

### Paso 4 · Planificar una mejora con criterios observables

**Haz:** usa Shift+Tab para seleccionar el modo de planificación, comprobando el indicador visible. Envía:

```text
Planifica mostrar siempre “Mostrando X de Y” en GalleryGrid, incluso cuando
no queden más fotos. X es displayedPhotos.length, Y es filteredPhotos.length.
Con cero resultados debe mostrar 0 de 0. Evita duplicar el contador que ya
existe dentro del bloque de Load More. Conserva la condición del botón,
el slice acumulativo, tags OR, tags AND búsqueda y estado de likes.
No cambies props públicas, mocks, rutas ni dependencias. Entrega ubicación,
cambio mínimo y casos de comprobación. Todavía no implementes.
```

**Comprueba:** el plan ubica el contador fuera de la condición que oculta el bloque de Load More; conserva el botón y sus condiciones. No debe añadir paginación de servidor, estados redundantes ni una segunda fuente del total. Si propone `mockPhotos.length` como Y, presenta un filtro con menos coincidencias para mostrar por qué esa decisión rompería el contrato.

**Explica:** «El contrato permite detectar un error en el plan antes de tocar código». **Si no localizas el modo:** consulta la ayuda de tu instalación y mantén la petición explícita de análisis; indica qué modo usaste realmente. Referencia de inicio, archivos con @ y planificación: [uso oficial de Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/overview).

### Paso 5 · Cerrar el recorrido principal con evidencia

**Haz:** enseña los archivos citados, el cálculo explicado y el plan obtenido. Pregunta al grupo qué criterio usaría para comprobar el contador al filtrar. Guarda la respuesta del ensayo con fecha y versión para poder consultarla.

**Comprueba:** puedes explicar por qué no hay implementación ni pruebas ejecutadas todavía. El resultado principal es un análisis contrastado y un plan revisado. **Di:** «La interfaz cambió; mantenemos evidencia, alcance y revisión». Regresa a la lámina 87 del manual local, o a la 84 para volver al concepto de CLI. Desde GitHub, vuelve al [índice del instructor](README.md).

**Si hubo un bloqueo:** resume el paso alcanzado y muestra el respaldo que sí preparaste. No presentes una transcripción como una ejecución en vivo. Continúa con el siguiente tema sin instalar herramientas durante el tiempo de explicación.

### Extensión opcional · Implementar y comprobar el contador

**Antes de empezar:** conserva el estado inicial en tu rama o copia de ensayo. Cambia al modo de ejecución compatible y envía:

```text
Implementa únicamente el plan revisado del contador permanente en GalleryGrid.
Mantén las condiciones del botón Load More y elimina el contador duplicado
si lo mueves. No instales dependencias ni ejecutes pruebas inexistentes.
Al terminar, enumera el archivo cambiado y las comprobaciones pendientes.
```

**Haz:** revisa cada acción que requiera aprobación en CLI y comprueba que corresponde al cambio. En un clon, abre el diff del archivo; en el export, usa la comparación de tu editor contra la copia inicial. Inicia la galería con el entorno ya preparado y visita `/gallery`.

**Comprueba manualmente:**

| Escenario | Resultado esperado |
|---|---|
| Apertura, sin filtros | X coincide con tarjetas visibles; Y con total de fotos filtradas |
| Pulsar Load More | X aumenta sin perder tarjetas anteriores |
| Cargar todas | El contador sigue visible; el botón desaparece si no quedan fotos |
| Buscar un texto sin coincidencias | 0 de 0 y estado vacío coherente |
| Cambiar tag después de cargar más | Primera tanda del nuevo resultado, sin alterar la lógica de tags |
| Marcar y desmarcar un like | Conserva el comportamiento anterior |

**Si falla:** registra acción, valor esperado y observado; pide corregir solo esa desviación. Si había errores previos de arranque, resuélvelos durante el ensayo y no los atribuyas automáticamente al contador. **Cierre:** diferencia el plan, el diff y la observación de la app; cada uno demuestra algo distinto.
