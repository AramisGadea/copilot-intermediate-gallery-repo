# Ampliaciones para el instructor — 6 de octubre de 2026

Guías desarrolladas para esta copia local del workshop. Complementan las nueve guías originales de `demos/`. Las instrucciones del producto se apoyan en fuentes oficiales enlazadas; los prompts, criterios y tiempos son propuestas didácticas del instructor.

## Abrir el material

El HTML está fuera del repositorio. Si conservas ambos elementos juntos en tu
computadora, abre `Manual-interactivo-GitHub-Copilot-Intermediate.html#demos`.
En GitHub esa ruta local no está disponible: usa estas guías Markdown y toma
las menciones a láminas solo como referencias para la copia local del manual.

| Recorrido | Archivo | Láminas |
|---|---|---|
| Contexto y evidencia | [contexto-demo.md](contexto-demo.md) | 27, 32, 34 |
| Instrucciones, agente y skill | [personalizacion-demo.md](personalizacion-demo.md) | 43–46, 53 |
| Issue, implementación y Code Review | [revision-pr-demo.md](revision-pr-demo.md) | 17, 20, 53, 73, 82 |
| Copilot CLI | [copilot-cli-demo.md](copilot-cli-demo.md) | 84–87 |

## Qué está listo y qué se crea durante la práctica

- Ya estaban en tu repo: `.github/copilot-instructions.md`, `.github/agents/Plan.agent.md` y los prompt files de `.github/prompts`.
- Agregado y listo para descubrir: [.github/skills/revisar-galeria/SKILL.md](../../.github/skills/revisar-galeria/SKILL.md) y su contrato. La invocación es manual mediante `/revisar-galeria`; comprueba descubrimiento y lectura en tu sesión real. La ubicación correcta, desde este README, se encuentra dos niveles arriba, en la raíz del repo.
- Plantilla de instrucciones: [gallery-tests.instructions.md.example](templates/gallery-tests.instructions.md.example). Se copia durante la demo a `.github/instructions/gallery-tests.instructions.md`.
- Ejemplo opcional: [AGENTS.md.example](templates/AGENTS.md.example). No hay un nuevo AGENTS.md activo en la raíz. Antes de activarlo, consolidar reglas con las instrucciones generales.
- La aplicación, package.json, configuración de MCP, agente Plan y guías originales conservan su contenido. Los cambios de código, los tests y los recursos remotos se producen al ejecutar las prácticas correspondientes.

## Secuencia breve para el ensayo

1. Arranca y observa la galería con la preparación del HTML.
2. Compara las dos consultas de contexto; guarda las respuestas y archivos usados.
3. Selecciona Plan Agent y revisa el plan. Crea la instrucción específica siguiendo la plantilla.
4. Invoca la skill y contrasta su tabla contra el código. No necesita Jest ni una instalación adicional de paquetes.
5. Prepara CLI antes del workshop. La vía npm y el devcontainer usan Node 22.
6. En un clon con el código publicado, prepara el issue, sesión remota y PR. Solicita Code Review desde Reviewers cuando esté disponible.
7. Conserva resultados reales del ensayo y registra lo pendiente. La preparación documental no confirma ejecución con tu cuenta.

## Para repetir

Usa una rama o copia de ensayo con el estado inicial. Si ya creaste las instrucciones, revísalas en lugar de sobrescribirlas. La skill se entrega lista: para enseñar su creación puedes abrir sus dos archivos y explicar su contenido, sin fingir que se generaron en vivo. No borres cambios previos para recuperar la línea base.

El catálogo principal del HTML distingue demos principales y opcionales. Sus tiempos son orientativos y necesitan ensayo; no agregues todos los recorridos completos encima de la agenda de cuatro horas.
