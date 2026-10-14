---
name: proponer-arquitectura-azure
description: Propone una arquitectura objetivo en Azure para esta galería a partir de las capacidades evidenciadas en el código, las promesas de producto no implementadas y las restricciones del cliente (privacidad, identidad, aislamiento, cumplimiento, operación y equipo). Úsala después de auditar el repositorio y antes de generar infraestructura.
disable-model-invocation: true
---
# Proponer una arquitectura objetivo en Azure

Este repositorio **no tiene una arquitectura que descubrir**: no hay backend, API, base de datos ni autenticación. Por tanto no deduzcas una arquitectura existente. **Deriva los requisitos** desde las capacidades evidenciadas y desde las promesas de producto que hoy no están implementadas, y propón una arquitectura que las soporte.

Esta propuesta **no sustituye** una revisión formal de arquitectura, una evaluación de cumplimiento ni un dimensionamiento de costes. Decláralo en la salida.

## Fase 1 — Entradas del cliente (no la omitas)

Una arquitectura sin clasificación de datos ni requisitos de aislamiento es un catálogo de servicios, no una arquitectura.

1. Lee [las entradas del cliente](references/entradas-cliente.md).
2. Si el usuario **no** aportó las restricciones, **pregunta** antes de proponer. Agrupa las preguntas por bloque y hazlas de una sola vez; no interrogues de forma incremental.
3. Si el usuario responde «asume el perfil por defecto» o equivalente, usa los valores por defecto documentados y **declara explícitamente cada supuesto** en la salida.
4. Si el usuario aporta solo parte de las entradas, combina lo aportado con los valores por defecto y marca claramente el origen de cada una.

No pases a la fase 2 sin haber fijado las entradas, ya sea por respuesta del usuario o por perfil por defecto declarado.

## Fase 2 — Propuesta

1. Lee el código para fundamentar los requisitos: `src/app/upload/page.tsx`, `src/components/upload/UploadZone.tsx`, `src/app/admin/page.tsx`, `src/app/gallery/page.tsx`, `src/components/gallery/GalleryGrid.tsx`, los mocks de `src/lib/`, `next.config.ts` y `package.json`.
2. Si existe una auditoría previa del skill `auditar-capacidades-y-riesgos`, úsala como entrada y mapea sus hallazgos a decisiones de arquitectura.
3. Consulta [el catálogo de decisiones](references/catalogo-decisiones.md) y resuelve **cada** decisión aplicable, incluyendo las que el cliente no mencionó.
4. Cada requisito que propongas debe quedar anclado a evidencia del repositorio o a una entrada del cliente. Si no procede de ninguna de las dos, márcalo como supuesto propio.

### Decisiones que debes cubrir

- Hosting del frontend Next.js, según si se requiere renderizado en servidor, red privada o escalado a cero.
- Almacenamiento de las fotografías y su separación de los metadatos, incluidos niveles de acceso por antigüedad.
- Entrega y caché de imágenes, y cómo se concilia con las galerías privadas.
- Control de acceso a galerías privadas y de client review: identidad, autorización, emisión y revocación de accesos temporales.
- Pipeline de procesamiento y optimización: dónde se ejecuta, síncrono o asíncrono, encolado, reintentos y fallos.
- Validación y seguridad del upload: límite de tamaño real, verificación del contenido, antimalware y metadatos EXIF.
- Metadatos y persistencia: galerías, tags, copyright, likes y vistas.
- Secretos, identidad de carga de trabajo y configuración.
- Observabilidad, respaldo y recuperación.
- Entornos y promoción, enlazando con la práctica de IaC.

## Reglas de rigor

- **No inventes cifras.** No afirmes costes, SLA, cuotas ni límites de servicio. Indica qué consultar: la calculadora de precios de Azure, la documentación del servicio y los límites de suscripción vigentes.
- **No presentes una única opción como la correcta** cuando la elección dependa de una restricción que el cliente no ha confirmado. Expón las alternativas y el criterio que las separa.
- **Distingue siempre** entre lo evidenciado en el repositorio, lo propuesto por ti y lo supuesto por falta de entrada del cliente.
- **No generes plantillas de infraestructura** (Bicep, Terraform, ARM) salvo que el usuario lo pida explícitamente. La implementación corresponde al agente de IaC.
- **No modifiques el código de la aplicación** durante esta propuesta.
- Si el nombre de un servicio, un SKU o una capacidad pudo cambiar, señálalo como punto a verificar en la documentación vigente en lugar de afirmarlo.
- No declares que ejecutaste una prueba, un despliegue, una estimación o una herramienta si no ocurrió.
- Las promesas de la interfaz (optimización automática, galerías privadas, descarga controlada, SEO) son **requisitos a soportar**, no componentes existentes.

## Formato de salida

### 1. Entradas asumidas

Tabla con cada entrada, su valor y su origen: **aportada por el usuario**, **valor por defecto** o **supuesto propio**.

### 2. Requisitos derivados del repositorio

| Requisito | Evidencia (archivo y fragmento) | Origen |
|---|---|---|

Ancla cada requisito a código o a UI real. Este apartado es el que evita una arquitectura genérica.

### 3. Arquitectura propuesta

Descríbela **por flujos**, no como una lista de servicios: subida, procesamiento, consulta pública y consulta privada de cliente. En cada flujo indica dónde se valida, dónde se autoriza y dónde se persiste.

### 4. Tabla de decisiones

| Decisión | Opción recomendada | Alternativas consideradas | Criterio que inclinó la elección | Qué entrada del cliente la cambiaría |
|---|---|---|---|---|

La última columna es la más valiosa: demuestra que la arquitectura responde a restricciones, no a modas.

### 5. Variantes por perfil

Al menos una variante de **coste mínimo** y una de **aislamiento alto**, señalando qué cambia entre ambas y qué se sacrifica en cada caso.

### 6. Controles de seguridad y privacidad

Mapeados a los hallazgos de la auditoría previa, si existe; si no, derivados de las entradas del cliente.

### 7. Plan de adopción por etapas

Desde la demo actual hasta el destino, indicando qué se puede abordar sin bloquear al resto.

### 8. Fuera de alcance y relevo

Qué no cubre esta propuesta y qué debe verificarse. Indica explícitamente que la implementación de infraestructura corresponde al agente de Bicep descrito en `demos/custom-agent-bicep-demo.md`.

Usa español. Una propuesta no valida la arquitectura: requiere revisión de un arquitecto y una prueba de concepto antes de comprometerse con ella.
