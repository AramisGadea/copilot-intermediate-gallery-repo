# Contrato de capacidades y riesgos

Este repositorio es una aplicación didáctica de workshop. Varias capacidades son simuladas y no deben describirse como funcionalidades listas para producción. Este contrato describe la baseline observada en esta copia local, no una especificación oficial ni el resultado de pruebas ejecutadas.

| Capacidad | Implementación de partida | Expectativa de revisión |
|---|---|---|
| Upload de fotos | `UploadZone` genera un preview con `URL.createObjectURL` y marca `success` tras un `setTimeout` | No afirmar que existe almacenamiento ni respuesta de servidor |
| Progreso de subida | Incremento local con `setInterval`; «Upload complete» es un estado local | No presentarlo como progreso de red |
| Límite de 10 MB | Comunicado en el texto de la zona de arrastre | Comprobar si existe `maxSize` o validación equivalente |
| Cantidad de archivos | `maxFiles` por defecto 10 en `useDropzone` | Comprobar si hay límite equivalente en servidor |
| Formatos aceptados | `accept` de imágenes en el cliente | No confundir con validación de contenido en servidor |
| Optimización automática | Prometida por la interfaz de `/upload` | Buscar un flujo explícito de procesamiento; `sharp` instalado no demuestra uso |
| Galerías privadas y Client Review | Opciones de un `select` en `/upload` y etiquetas en los mocks de administración | No asumir autenticación ni autorización |
| Administración | `/admin` con stats y tabla desde `src/lib/mock-admin-data.ts` | Comprobar protección de acceso y manejadores de las acciones |
| Editar y eliminar galerías | Botones de icono sin manejador ni confirmación | Comprobar autorización, confirmación, errores y auditoría |
| Descargar y compartir | Botones visuales en `GalleryGrid` | Comprobar comportamiento real y control de acceso |
| SEO | Mencionado como capacidad de portafolio | Comprobar si existe configuración por galería |
| Guardar borrador y «Upload & Process» | Botones visuales en `/upload` | Comprobar persistencia, envío y manejo de errores |
| Configuración de subida (galería, visibilidad, tags, copyright) | Campos no controlados y sin envío | No afirmar que los valores se guardan ni se aplican |
| Likes | `Set` local por ID en `GalleryGrid` | No afirmar persistencia: se pierde al recargar |
| Load More | Acumulación local sobre los mocks | No presentarlo como paginación de servidor |
| Datos de galería y administración | Mocks locales en `src/lib/` | No presentarlos como datos de backend |
| Persistencia general | No se evidencia backend, API, base de datos ni autenticación | Decir «no evidenciado en el alcance revisado», no «no existe» |
| Accesibilidad | Botones de icono sin texto visible; diálogo de detalle propio | Comprobar nombres accesibles, teclado y foco |

Una funcionalidad simulada **no es por sí misma un defecto** cuando está claramente identificada como parte del workshop. Se convierte en hallazgo cuando la documentación o la interfaz la presenta como real, o cuando se propone llevarla a producción sin los controles necesarios.

La baseline del workshop no incluye test runner, backend ni subida persistente, y usa `npm ci` para instalar dependencias. No declares resultados de `npm run lint`, `npm run build`, `npm audit` u otras herramientas que no hayas ejecutado en la sesión.
