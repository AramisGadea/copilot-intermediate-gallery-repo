# Demo: Testing con GitHub Copilot — de Cero a Suite Funcional

¡Bienvenido al demo de generación de tests con GitHub Copilot! Partiremos de un proyecto sin ninguna infraestructura de testing y llegaremos a una suite funcional con tests reales ejecutándose en verde, usando Copilot para cada paso del camino.

---

## Lo que Aprenderás
Al finalizar este demo, podrás:
- [ ] Configurar Jest y React Testing Library en un proyecto Next.js 15 con un solo prompt
- [ ] Generar tests unitarios para lógica de filtrado y estado de componentes
- [ ] Generar tests de interacción para componentes con drag & drop
- [ ] Pedirle a Copilot que evalúe su propia cobertura e identifique casos edge
- [ ] Entender cuándo Copilot es suficiente y cuándo el criterio humano es necesario

**Tiempo Estimado:** 15-20 minutos

---

## Punto de Partida

Abre `package.json` y verifica que **no existe ningún test runner** en el proyecto. No hay `jest`, no hay `@testing-library`, no hay script `test`.

Este es el punto de partida más honesto: un proyecto real de Next.js 15 con TypeScript, sin un solo test escrito.

```json
"scripts": {
  "dev": "next dev --turbopack",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```

No añadas Jest a la baseline antes de esta demo: instalarlo y revisar los
cambios generados es parte intencional del ejercicio.

**Mensaje para la audiencia:** El tiempo que más se pierde en testing no es escribir los tests — es configurar el entorno. Vamos a resolver eso en un prompt.

---

## 🛠️ Paso 1: Configura el Entorno de Testing

Con Copilot Chat en modo **Agent**, escribe el siguiente prompt:

```markdown
Este proyecto es Next.js 15 con TypeScript y React 19. No tiene ningún test runner configurado.

Configura Jest con React Testing Library para este proyecto. Necesito:
- Instalar las dependencias necesarias
- Crear el archivo de configuración de Jest compatible con Next.js 15
- Añadir el script "test" en package.json
- Crear un archivo de setup para React Testing Library
- Un test de humo mínimo que confirme que el entorno funciona

Usa la configuración oficial recomendada por Next.js para Jest.
```

### Qué observar
- Copilot instala `jest`, `jest-environment-jsdom`, `@testing-library/react`, `@testing-library/jest-dom` y los tipos necesarios
- Crea `jest.config.ts` con el transformer de Next.js
- Crea `jest.setup.ts` con el import de `@testing-library/jest-dom`
- Actualiza `package.json` con el script `test`

Una vez aplicados los cambios, ejecuta:

```bash
npm test
```

**Resultado esperado:** El test de humo pasa en verde. El entorno está listo.

---

## 🧪 Paso 2: Tests de Lógica de Filtrado — `GalleryGrid`

El componente `GalleryGrid.tsx` tiene la lógica más rica del proyecto:
- Filtro por tags (`selectedTags`)
- Filtro por texto (`searchQuery`) sobre título, tags y fotógrafo
- Paginación con `currentPage` y `limit`

Pide los tests con este prompt:

```markdown
Genera tests unitarios para el componente GalleryGrid en `src/components/gallery/GalleryGrid.tsx`.

Cubre estos escenarios:
1. Renderiza el número correcto de fotos con el límite por defecto
2. Filtra correctamente por un tag seleccionado
3. Filtra correctamente por searchQuery en el título
4. Filtra por searchQuery en el nombre del fotógrafo
5. La búsqueda es case-insensitive
6. Con selectedTags y searchQuery combinados, aplica ambos filtros
7. Con array de fotos vacío (mockPhotos = []), muestra 0 resultados
8. El botón de like alterna correctamente el estado liked/unliked

Usa React Testing Library. Mockea `mockPhotos` desde `@/lib/mock-photo-data` para tener datos controlados en los tests.
```

### Qué remarcar con la audiencia
- Copilot mockea el módulo de datos para que los tests no dependan de los datos reales
- Los tests siguen el patrón `describe / it` legible
- El caso edge de array vacío suele olvidarse al escribir tests manualmente

**Resultado esperado:** 8+ tests generados. Ejecuta `npm test` y verifica que pasan.

---

## 🔄 Paso 3: Tests de Estado — Toggle de Likes

El método `toggleLike` es lógica de estado pura: añade el id si no existe, lo elimina si ya está. Ideal para mostrar tests de comportamiento sin complejidad de renderizado.

```markdown
Genera tests específicos para el comportamiento del toggle de likes en GalleryGrid:

1. Al hacer click en el botón de like, la foto queda marcada como liked
2. Al hacer click de nuevo, la foto vuelve a estado not-liked
3. Hacer like a una foto no afecta el estado de las demás
4. El estado de likes se mantiene al cambiar el filtro activo
```

**Resultado esperado:** Tests de interacción claros que documentan el comportamiento esperado del componente.

---

## 📤 Paso 4: Tests de Interacción — `UploadZone`

`UploadZone.tsx` usa `react-dropzone` y simula progreso de subida con `setInterval`. Es un buen caso para mostrar que Copilot sabe manejar dependencias del browser y timers.

```markdown
Genera tests para el componente UploadZone en `src/components/upload/UploadZone.tsx`.

Cubre:
1. Renderiza la zona de drop con el mensaje correcto
2. Acepta archivos al hacer drop y los muestra en la lista
3. No acepta más archivos que el límite definido en maxFiles
4. Después de la subida simulada, el archivo muestra status 'success'
5. El botón de eliminar remueve el archivo de la lista

Ten en cuenta que:
- URL.createObjectURL debe ser mockeado (no existe en jsdom)
- El progreso de subida usa setInterval — usa jest.useFakeTimers() para controlarlo
```

### Qué remarcar con la audiencia
- Copilot identifica automáticamente las dependencias del browser que hay que mockear
- `jest.useFakeTimers()` permite controlar el tiempo sin esperas reales en los tests
- Este tipo de setup manual es exactamente donde más tiempo se pierde sin Copilot

**Resultado esperado:** Tests de interacción para el componente de upload, con los mocks correctos.

---

## 🔍 Paso 5: Copilot Evalúa su Propia Cobertura

Este es el cierre más potente de la demo. Una vez que los tests existen, pídele a Copilot que los critique:

```markdown
Revisa los tests que acabamos de generar para GalleryGrid y UploadZone.

Actúa como un senior engineer haciendo revisión de cobertura:
- ¿Qué casos edge importantes no están cubiertos?
- ¿Hay algún escenario de error que deberíamos testear?
- ¿Qué tests añadirías tú antes de hacer merge de un PR con estos componentes?
- ¿Hay algún test frágil que podría dar falsos positivos?
```

### Qué observar con la audiencia
- Copilot señala casos que no se pidieron: estados de error, accesibilidad, comportamiento con props undefined
- Identifica tests frágiles que dependen de implementación interna en lugar de comportamiento
- Demuestra que Copilot no es solo ejecutor de tareas, también puede razonar sobre calidad

**Mensaje clave:** *Copilot no reemplaza tu criterio sobre qué testear — elimina la fricción de escribirlo.*

---

## 🏃 Paso 6: Ejecuta la Suite Completa

```bash
npm test -- --coverage
```

Muestra el reporte de cobertura en la terminal. La audiencia ve:
- Cuántos tests se generaron en total
- El porcentaje de cobertura alcanzado
- Qué líneas quedan sin cubrir (y por qué podría ser aceptable)

**Resultado esperado:** Suite en verde con cobertura visible. De 0 tests a cobertura real en menos de 20 minutos.

---

## 🧠 Puntos de Mensaje para la Audiencia

- El mayor coste del testing no es escribir los tests, es la inercia de empezar. Copilot elimina ese bloqueo.
- Los tests generados documentan el comportamiento esperado igual que los escritos a mano.
- Copilot es especialmente útil en el setup inicial y en los casos edge que se olvidan bajo presión.
- El criterio humano sigue siendo necesario: decidir *qué* testear y revisar que los tests realmente validan lo que dicen que validan.
- Un agente personalizado (como el `TestWriter` de la demo anterior) puede codificar las convenciones de testing de tu equipo para resultados aún más consistentes.

---

## 💡 Prompts Adicionales para Explorar

Si el tiempo lo permite, prueba alguno de estos:

```markdown
# Generar tests para los datos mock
Valida que todos los objetos en mockPhotos cumplen la interfaz Photo: tienen id, url, title y tags como string[].
```

```markdown
# Tests de accesibilidad
Revisa GalleryGrid con @testing-library/jest-dom y añade assertions de accesibilidad: roles ARIA correctos en los botones de like y el modal de foto.
```

```markdown
# Test de regresión para el issue #1
El issue #1 propone añadir filtro por orientación. Genera tests que definan el comportamiento esperado ANTES de implementar la funcionalidad (TDD).
```

---

## ✅ Lista de Verificación de Completitud

Marca cada ítem al completarlo:

- [ ] Verifiqué que el proyecto no tenía tests antes de empezar
- [ ] Configuré Jest y RTL con un solo prompt
- [ ] Ejecuté el test de humo inicial en verde
- [ ] Generé y ejecuté tests para la lógica de filtrado de GalleryGrid
- [ ] Generé tests para el toggle de likes
- [ ] Generé tests de interacción para UploadZone con mocks correctos
- [ ] Pedí a Copilot una revisión crítica de la cobertura
- [ ] Ejecuté `npm test --coverage` y revisé el reporte

---

## 🚀 ¿Qué Sigue?

Esta demo cierra el ciclo completo de desarrollo asistido por IA del workshop:

1. **MCP** creó el issue con los requisitos
2. **Coding Agent** generó el PR con la implementación
3. **Testing** valida que lo que se implementó funciona correctamente

Para llevar esto más lejos en tu equipo:
- Crea un agente `TestWriter` en `.github/agents/` con las convenciones de testing de tu proyecto
- Añade `npm test` al flujo de CI para que los tests corran en cada PR automáticamente

👉 **[Volver al índice de demos](./README.md)**
