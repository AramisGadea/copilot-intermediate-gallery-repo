# Entradas del cliente

Restricciones que condicionan la arquitectura. Pregúntalas antes de proponer. Si el usuario pide avanzar con el perfil por defecto, usa la columna correspondiente y **declara cada supuesto** en la salida.

El perfil por defecto describe un estudio fotográfico pequeño que comparte galerías con clientes externos. No es el perfil de todos los clientes: sirve para que una demo pueda avanzar sin bloquearse.

## 1. Datos y privacidad

| Entrada | Por qué importa | Valor por defecto |
|---|---|---|
| Clasificación de las fotografías | Determina cifrado, aislamiento y control de acceso | Confidencial de cliente, no regulada |
| ¿Contienen datos personales o biométricos? | Activa requisitos legales de tratamiento | Sí: retratos de personas identificables |
| Residencia geográfica exigida | Condiciona regiones y replicación | Una única región, sin exigencia contractual |
| Retención | Condiciona niveles de almacenamiento y ciclo de vida | Indefinida mientras el cliente esté activo |
| Derecho de supresión | Exige borrado verificable y propagación a caché y copias | Requerido a petición del cliente |

## 2. Identidad

| Entrada | Por qué importa | Valor por defecto |
|---|---|---|
| Quién es el fotógrafo | Define la identidad interna y el acceso a administración | Personal interno, pocos usuarios |
| Quién es el cliente final | Define la identidad externa | Usuario externo sin cuenta corporativa |
| ¿Hay federación corporativa? | Condiciona el proveedor de identidad | No |
| ¿Se comparten galerías con invitados? | Exige enlaces temporales o identidad de invitado | Sí, mediante acceso temporal revocable |

## 3. Aislamiento y red

| Entrada | Por qué importa | Valor por defecto |
|---|---|---|
| ¿Se admite exposición pública? | Determina si puede haber ingreso desde Internet | Sí para el portafolio público |
| ¿Se exige red privada? | Cambia hosting, almacenamiento y conectividad | No |
| ¿Inspección del tráfico saliente? | Exige salida controlada y filtrado | No |

## 4. Cumplimiento y gobierno

| Entrada | Por qué importa | Valor por defecto |
|---|---|---|
| Marcos aplicables | Condiciona controles obligatorios | Ninguno formal; buenas prácticas de protección de datos |
| Auditoría exigida | Determina registro de accesos y operaciones destructivas | Registro de acceso y de borrado |
| Política de secretos | Determina almacén de secretos e identidad gestionada | Sin secretos en código ni en variables del cliente |
| ¿Claves gestionadas por el cliente? | Cambia el modelo de cifrado y su operación | No: claves gestionadas por la plataforma |

## 5. Operación

| Entrada | Por qué importa | Valor por defecto |
|---|---|---|
| Disponibilidad esperada | Condiciona redundancia y coste fijo | Horario laboral, sin compromiso formal |
| RTO y RPO | Condicionan respaldo y replicación | Horas, no minutos |
| Volumen de fotos y de tráfico | Condiciona almacenamiento y entrega | Decenas de miles de fotos, tráfico bajo |
| Picos | Condicionan escalado y encolado | Picos al entregar una sesión a un cliente |
| Presupuesto orientativo | Condiciona SKU y topología | Ajustado |
| ¿Coste fijo o por consumo? | Decide entre instancias reservadas y serverless | Preferencia por consumo |

## 6. Equipo

| Entrada | Por qué importa | Valor por defecto |
|---|---|---|
| Experiencia con contenedores | Condiciona el modelo de hosting | Baja |
| PaaS frente a serverless | Condiciona el procesamiento asíncrono | Preferencia por servicios gestionados |
| Madurez de CI/CD | Condiciona entornos y promoción | GitHub Actions disponible |
| Quién opera la plataforma | Condiciona cuánta operación se puede asumir | El propio equipo de desarrollo |

## Cómo usar estas entradas

- Una entrada sin confirmar **no** se convierte en una decisión cerrada: la decisión asociada debe presentar alternativas.
- Cambiar una sola entrada puede invalidar varias decisiones. Refleja esa dependencia en la columna «qué entrada la cambiaría» de la tabla de decisiones.
- Los valores por defecto son una hipótesis de trabajo del workshop, no una recomendación para un cliente real.
