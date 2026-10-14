# Catálogo de decisiones de arquitectura

Decisiones recurrentes para esta aplicación, con sus alternativas y el criterio que las separa. **No contiene costes, SLA, cuotas ni límites**: esos datos deben consultarse en la calculadora de precios de Azure y en la documentación vigente del servicio.

Cada decisión indica el punto de partida evidenciado en el repositorio para que la propuesta quede anclada al proyecto y no sea un diagrama genérico.

---

## D1. Hosting del frontend Next.js

**Punto de partida:** aplicación Next.js 15 con App Router; `npm run build` genera rutas estáticas en la baseline.

**Alternativas:** hosting gestionado para aplicaciones web con integración de CI/CD; plataforma de aplicaciones web administradas; plataforma de contenedores administrada.

**Criterios de elección:** si se necesita renderizado en servidor o solo contenido estático; si se exige red privada o integración con red virtual; si se requiere escalado a cero; experiencia del equipo con contenedores; número de entornos.

**Entradas que la cambian:** aislamiento y red, equipo, presupuesto.

---

## D2. Almacenamiento de fotografías y separación de metadatos

**Punto de partida:** no hay almacenamiento; `UploadZone` solo crea previsualizaciones locales con `URL.createObjectURL`.

**Alternativas:** almacenamiento de objetos para los binarios con un almacén separado para metadatos; almacenar binarios y metadatos en el mismo servicio.

**Criterios:** los binarios y los metadatos tienen patrones de acceso, tamaño y ciclo de vida distintos; separarlos permite aplicar niveles de acceso por antigüedad a las fotos sin penalizar las consultas de catálogo.

**A resolver siempre:** contenedores separados para originales y derivados; niveles de acceso según antigüedad; reglas de ciclo de vida; protección frente a borrado accidental; coherencia del borrado entre binario, metadatos y caché.

**Entradas que la cambian:** retención, derecho de supresión, residencia, volumen, RPO.

---

## D3. Entrega y caché de imágenes

**Punto de partida:** `src/lib/mock-photo-data.ts` contiene rutas locales de placeholder; `GalleryGrid` no las utiliza para cargar imágenes y muestra rectángulos con gradientes. La entrega real de fotografías es una capacidad objetivo, no un flujo implementado.

**Alternativas:** red de entrega de contenido delante del almacenamiento; servir desde el almacenamiento sin capa de entrega; servir a través de la aplicación.

**Criterios:** latencia y coste de salida frente a complejidad; **punto crítico:** una capa de caché mal configurada puede conservar y servir contenido privado. Las galerías privadas requieren que la autorización se evalúe antes de la caché o que el contenido privado no se cachee de forma compartida.

**Entradas que la cambian:** clasificación de datos, exposición pública, volumen, presupuesto.

---

## D4. Control de acceso a galerías privadas y client review

**Punto de partida:** la visibilidad es un `select` con `Public`, `Private`, `Client Review` y `Draft` en `src/app/upload/page.tsx`, sin autenticación ni autorización.

**Alternativas:** identidad para clientes externos con cuentas; acceso temporal mediante enlaces firmados con caducidad; combinación de ambas según el tipo de galería.

**Criterios:** si el cliente final acepta crear una cuenta; si se exige revocación inmediata; si el enlace puede reenviarse; trazabilidad de quién accedió.

**A resolver siempre:** la autorización se evalúa en el servidor, nunca ocultando controles en la UI; cómo se emite el acceso; cómo se revoca; qué caducidad tiene; qué ocurre con la caché y con las URLs ya emitidas al revocar.

**Entradas que la cambian:** identidad, invitados, auditoría, derecho de supresión.

---

## D5. Pipeline de procesamiento y optimización

**Punto de partida:** `/upload` promete «automatically resized and optimized»; no existe procesamiento. `sharp` está en `package.json`, pero su presencia no demuestra uso.

**Alternativas:** procesamiento asíncrono disparado por la llegada del archivo; procesamiento síncrono en la petición de subida; transformación en el momento de la lectura.

**Criterios:** el procesamiento síncrono bloquea la subida y falla con archivos grandes o lotes; el asíncrono exige estados intermedios visibles en la UI y gestión de fallos.

**A resolver siempre:** qué derivados se generan; dónde se guardan; cómo se encola el trabajo; reintentos y cola de mensajes fallidos; qué ve el usuario mientras el procesamiento no termina; qué ocurre si falla de forma definitiva.

**Entradas que la cambian:** volumen, picos, preferencia PaaS o serverless, presupuesto.

---

## D6. Validación y seguridad del upload

**Punto de partida:** `useDropzone` configura `accept` y `maxFiles`, pero **no** `maxSize`, mientras la interfaz anuncia «up to 10MB each». El progreso y el estado `success` son simulados con temporizadores.

**A resolver siempre:**

- límite de tamaño aplicado en servidor, coherente con el texto de la UI;
- límite de cantidad y límites por usuario contra abuso;
- validación del tipo real del archivo, no solo de la extensión ni del `accept`;
- tratamiento de SVG y de formatos con contenido activo;
- nombres de archivo seguros y no adivinables;
- almacenamiento fuera de rutas servidas como ejecutables;
- análisis antimalware si el contexto del cliente lo exige;
- metadatos EXIF: decidir si se conservan, se depuran o se separan, ya que pueden incluir geolocalización;
- respuesta clara ante archivos rechazados.

**Criterios:** la validación en cliente mejora la experiencia pero no es un control de seguridad; el control debe existir en el servidor.

**Entradas que la cambian:** clasificación de datos, datos personales, cumplimiento, volumen.

---

## D7. Metadatos y persistencia

**Punto de partida:** `src/lib/mock-admin-data.ts` y `src/lib/mock-photo-data.ts` son mocks; los likes viven en un `Set` local de `GalleryGrid`; los campos de galería, tags y copyright no se envían ni se guardan.

**Alternativas:** base de datos relacional; base de datos de documentos.

**Criterios:** el modelo tiene relaciones claras (galería, foto, cliente, tag) y consultas de filtrado por tag y texto, que ya existen en la UI; el volumen de escritura de likes y vistas es distinto al del catálogo y puede tratarse aparte.

**A resolver siempre:** dónde viven likes y vistas; cómo se mantiene la coherencia con el almacenamiento de binarios; qué se registra para auditoría.

**Entradas que la cambian:** equipo, volumen, auditoría, presupuesto.

---

## D8. Secretos, identidad de carga de trabajo y configuración

**Punto de partida:** no hay secretos ni variables de entorno en la baseline.

**A resolver siempre:** identidad gestionada entre servicios en lugar de cadenas de conexión; almacén de secretos para lo que no pueda resolverse con identidad; qué configuración puede exponerse al cliente y cuál no; rotación; separación de configuración por entorno.

**Entradas que la cambian:** política de secretos, cumplimiento, claves gestionadas por el cliente.

---

## D9. Observabilidad, respaldo y recuperación

**Punto de partida:** no hay instrumentación ni registro.

**A resolver siempre:** telemetría de aplicación y correlación entre subida y procesamiento; alertas sobre fallos del pipeline; registro de accesos a galerías privadas y de operaciones destructivas; **no registrar datos personales ni URLs firmadas**; respaldo de binarios y de metadatos de forma coherente; prueba de restauración.

**Entradas que la cambian:** RTO, RPO, auditoría, disponibilidad.

---

## D10. Entornos y promoción

**Punto de partida:** `GitHub Actions` está disponible; la baseline instala con `npm ci`.

**A resolver siempre:** cuántos entornos; aislamiento entre ellos; datos de prueba sin fotografías reales de clientes; infraestructura como código y su revisión; despliegue con identidad federada en lugar de credenciales de larga duración.

**Entradas que la cambian:** madurez de CI/CD, equipo, cumplimiento, presupuesto.

---

## Nota de vigencia

Los nombres de servicios, SKU y capacidades de Azure cambian. Antes de comprometer una decisión, verifica la documentación vigente y los límites aplicables a la suscripción del cliente. Este catálogo describe criterios de elección, no una recomendación de producto definitiva.
