---
name: auditar-capacidades-y-riesgos
description: Contrasta las capacidades que esta galería anuncia en UI y documentación con su implementación real, y señala los controles de seguridad, privacidad, accesibilidad y confiabilidad que harían falta antes de producción. Úsala al revisar upload, administración, galería o documentación de producto.
disable-model-invocation: true
---
# Auditar capacidades y riesgos del producto

Realiza una auditoría basada únicamente en la evidencia disponible en este repositorio. No edites la aplicación ni instales dependencias durante la auditoría. Esta revisión **no es un pentest ni una certificación de seguridad**: describe lo que el código evidencia y lo que queda pendiente de comprobar.

## Alcance inicial

Revisa, según corresponda al encargo:

- `src/app/page.tsx`, `src/app/upload/page.tsx`, `src/app/admin/page.tsx`, `src/app/gallery/page.tsx`
- `src/components/upload/UploadZone.tsx` y `src/components/gallery/GalleryGrid.tsx`
- los datos mock de `src/lib/`
- rutas API, middleware, autenticación, variables de entorno y configuración, **si existen**
- documentación que describa capacidades del producto (`README.md`, `COMPONENT_USAGE_GUIDE.md`, `demos/`)

Lee [el contrato de producto](references/contrato-producto.md) antes de emitir conclusiones.

## Procedimiento

1. Extrae las afirmaciones de producto de los textos de interfaz, nombres de botones, campos de formulario, mensajes de éxito y documentación.
2. Sigue cada capacidad por su flujo completo: **UI → evento → validación → autorización → operación → persistencia → respuesta**. Indica en qué eslabón se interrumpe.
3. Clasifica cada capacidad como **implementada**, **parcial**, **simulada**, **solo visual** o **no comprobable**.
4. Revisa al menos: upload, optimización automática, límite de archivos y de tamaño, datos mock, likes, Load More, administración, galerías privadas, client review, descarga, compartir, SEO, guardado de borradores y persistencia de la configuración de subida.
5. Evalúa los controles asociados a cada capacidad:
   - **Autenticación y autorización:** acceso a `/admin`, a galerías privadas y a las operaciones de editar, eliminar, descargar o compartir; si la visibilidad depende del servidor o solo de la UI.
   - **Uploads:** límite real de tamaño y de cantidad, validación en servidor además de `accept`, verificación del contenido real del archivo, nombres de archivo seguros, tratamiento de SVG y formatos activos, almacenamiento fuera de rutas ejecutables, manejo de archivos rechazados y límites contra abuso.
   - **Privacidad y exposición de datos:** quién puede ver cada galería, URLs públicas o predecibles frente a privadas, temporales o firmadas; metadatos EXIF; datos personales en nombres, tags o copyright; retención, eliminación y revocación de acceso.
   - **Operaciones destructivas:** autorización, diálogo de confirmación, identificación del elemento afectado, manejo de errores, recuperación y registro de auditoría.
   - **Confiabilidad:** mensajes de éxito sin respuesta del servidor, progreso simulado, errores ignorados y pérdida silenciosa de datos.
   - **Validación de entrada:** longitud, caracteres admitidos, normalización, validación en cliente y servidor, codificación de salida y errores visibles.
   - **Secretos y configuración:** credenciales en código o documentación, variables de entorno expuestas al cliente, logging de información sensible, cabeceras de seguridad y dependencias. No afirmes que una dependencia es vulnerable: recomienda `npm audit`, Dependabot o CodeQL como comprobación separada.
   - **Accesibilidad:** nombres accesibles de los botones de icono (ver, editar, eliminar, like, descargar, compartir, quitar archivo), navegación por teclado, foco y cierre del diálogo de detalle, anuncio del progreso y de los estados de error.
6. Distingue siempre entre:
   - **defecto demostrado:** hay evidencia concreta en el código;
   - **control ausente para producción:** sería necesario antes de desplegar;
   - **riesgo pendiente de ejecución:** requiere backend, despliegue o herramientas para comprobarse;
   - **limitación intencional del workshop:** simulación identificada honestamente.
7. No declares una vulnerabilidad explotable sin evidencia suficiente, ni afirmes que una prueba, comando o auditoría fue ejecutada si no ocurrió.

## Reglas específicas

- Un selector `Private` o `Client Review` no demuestra control de acceso.
- Un atributo `accept` no sustituye la validación en servidor, ni un texto «up to 10MB» demuestra que exista un límite aplicado.
- Un mensaje de éxito no demuestra persistencia.
- Un temporizador no representa progreso de red.
- Un botón sin manejador es una capacidad solo visual.
- Los datos mock no representan una API ni un backend.
- Ocultar o deshabilitar un control no equivale a autorizar una operación.
- El estado local de React no es persistencia: se pierde al recargar.
- No encontrar código no demuestra que un sistema externo no exista; indica que no está evidenciado en el alcance revisado.

## Formato de salida

### Alcance y limitaciones

Archivos leídos, base de comparación (diff indicado o «estado actual») y qué no pudo comprobarse.

### Matriz de capacidades

| Capacidad anunciada | Evidencia (archivo y fragmento) | Estado | Riesgo |
|---|---|---|---|

### Hallazgos priorizados

Para cada hallazgo: prioridad; archivo y evidencia real; promesa observada; comportamiento real; impacto para el usuario o el negocio; clasificación de certeza (del paso 6); corrección mínima; controles necesarios para producción.

### Recomendaciones separadas

1. Cambios de copy para describir honestamente la demo.
2. Cambios funcionales para completar la capacidad.
3. Controles de seguridad, privacidad y accesibilidad.
4. Comprobaciones que requieren ejecución o infraestructura.

### Comprobaciones pendientes

Lo que requiere ejecutar la aplicación, un backend o herramientas externas.

Usa español. No implementes las recomendaciones salvo que el usuario lo solicite. Una ausencia de hallazgos no demuestra que el producto sea seguro ni que esté listo para producción.
