---
title: "Ejercicio 2: Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación pueden requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo suele consistir en localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, descubrir qué archivo falta y volver a modificar el código.

El modo agente de Copilot puede ayudarte con este proceso.

El modo agente funciona de forma autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige los errores que detecta. En este ejercicio, usarás el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres son parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica archivos pertinentes y coordina cambios.
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas que genera Copilot antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de modificar el sitio, explóralo para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar juegos y sus detalles. La aplicación es un único proyecto Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el mapeo objeto-relacional (ORM) Drizzle; no hay un backend de API separado ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las interfaces del modelo de objetos del documento (DOM).

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo Astro. Ejecútala en tu GitHub Codespace:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un aviso con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost del terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor web del codespace.

### Explora el sitio web

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando sean relevantes.

La implementación inicial funciona, pero queremos añadir nuevas capacidades. Empieza por revisar el backlog de incidencias que se creó automáticamente al configurar el proyecto.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos del terminal e invocar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Para preparar el taller se usó Claude Sonnet 4.5, por lo que se incluye en estas instrucciones para obtener resultados relativamente coherentes. Como los modelos de lenguaje grandes (LLM) son probabilísticos, tus sugerencias pueden diferir de los ejemplos. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot también prioriza las incidencias según su utilidad para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto sobre cómo ampliar el sitio: `.github/instructions/ui.instructions.md`.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices para ampliar la funcionalidad:
   - información general sobre la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones para tipos de archivo concretos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar instrucciones, dividir conjuntos extensos en otros más pequeños y ofrecer ejemplos.

## Implementa el filtrado

Para completar los filtros, habrá que realizar al menos tres cambios:

- añadir o ajustar la lógica restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos con la interfaz de filtros.

El modo agente de Copilot puede realizar estas tareas. Pídele que implemente la funcionalidad.

1. Continúa con la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista desplegable de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que el modelo seleccionado sigue siendo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Actualiza el sitio para añadir filtros por editorial y categoría según los requisitos de la incidencia correspondiente del backlog de GitHub. Ya existe un helper de editoriales del ejercicio anterior; consérvalo y ajústalo si hace falta mientras completas la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en marcha, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: helpers de la capa de datos, interfaz y pruebas. A continuación, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer cada cambio; no es necesario que elijas mientras el agente siga trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot Chat pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas podrían fallar durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

Has usado el modo agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. En este ejercicio has aprendido cómo:

- el modo agente explora el proyecto, identifica archivos pertinentes y coordina cambios.
- el modo agente de Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos en el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa las habilidades de este ejercicio y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo funcionará la paginación y si el usuario podrá elegir el tamaño de página.
- proporciona a Copilot el contexto necesario para implementar la solución que necesitas.
- itera con GitHub Copilot, pidiendo cambios y proporcionando contexto. Es el flujo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
