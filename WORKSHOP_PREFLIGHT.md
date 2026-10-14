# Preflight del workshop

Completa esta lista antes de la sesión. No presentes una comprobación como
ejecutada si solo revisaste la documentación.

## Requerido para el recorrido local

- Node.js 22 LTS y npm.
- VS Code actualizado.
- Extensiones **GitHub Copilot** y **GitHub Copilot Chat**.
- Una licencia activa de GitHub Copilot y sesión iniciada en VS Code.
- Acceso de lectura y escritura a una copia del repositorio.

```bash
node --version
npm --version
npm ci
npm run lint
npm run build
npm run dev
```

Visita `/`, `/gallery`, `/upload` y `/admin`. La aplicación usa datos mock:
la subida solo crea previsualizaciones y simula el progreso; no envía archivos
a un servidor. La baseline tampoco incluye Jest ni un script `npm test`.

## Requerido solo para demos remotas

| Demo | Preparación | Alternativa si no está disponible |
|---|---|---|
| GitHub MCP | VS Code compatible con MCP, OAuth completado y permisos para crear issues | Redactar el issue en Chat y crearlo manualmente |
| Copilot Spaces | Acceso habilitado en la cuenta u organización y fuentes accesibles | Mostrar un Space ensayado o ejecutar los mismos prompts en Chat con archivos adjuntos |
| Coding Agent | Repositorio habilitado y permiso para asignar issues a Copilot | Revisar una sesión/PR preparado o explicar el flujo con el issue sin delegarlo |
| Copilot CLI | CLI instalada y autenticada antes de la sesión | Ejecutar los prompts equivalentes en Copilot Chat |

Prueba cada ruta remota con la misma cuenta y organización del workshop. Las
políticas, planes, permisos y disponibilidad del servicio pueden variar.

## Opcional

- Extensiones de ESLint, Prettier y Tailwind CSS (el devcontainer las incluye).
- Copilot CLI para el módulo de instructor.
- Acceso de escritura a una organización para compartir Spaces.

## Registro del ensayo

Anota fecha, versiones observadas, comandos ejecutados y resultados. Conserva
un issue, Space, sesión o PR de respaldo para cada demo remota y evita crear
recursos reales durante la explicación si los permisos no fueron verificados.
