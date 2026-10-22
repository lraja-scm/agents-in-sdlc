---
title: "Ejercicio 3: Prueba la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de cambios (PR, por sus siglas en inglés), debes confirmar que funciona en el navegador. En vez de probarla manualmente, conectarás el **servidor Playwright basado en Model Context Protocol (MCP)** y dejarás que Copilot controle un navegador real. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente los filtros en un navegador.
- publicarás la rama y abrirás una solicitud de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es más útil cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite que Copilot se comunique con herramientas y servicios externos mediante un protocolo estándar.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje de gran tamaño (LLM) y las herramientas, incluidas las de los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente con los servicios externos. El servidor gestiona la comunicación entre el agente y las herramientas externas, como API existentes o herramientas locales. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización de navegadores mediante Playwright, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye este servidor `github`, que permitió a Copilot leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor GitHub MCP remoto][remote-github-mcp-server], por lo que no hace falta instalarlo localmente. VS Code se conecta mediante HTTP y la primera vez que Copilot usa una herramienta te autenticas con GitHub.

## Añade el servidor Playwright MCP

Añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, lo que se necesita en un codespace sin escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **bajo demanda**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece encima de la entrada `playwright` para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma la confianza para que se inicie.

Este paso es opcional. Si no inicias el servidor manualmente, VS Code lo iniciará y te pedirá que confirmes la confianza cuando Copilot necesite las herramientas.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] permite que Copilot controle un navegador real: puede abrir una página, navegar, aplicar filtros y resumir el resultado. Así puedes comprobar rápidamente el comportamiento sin salir de la conversación.

El servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, de forma parecida a las tecnologías de asistencia. Así, una comprobación funcional también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe los filtros:

```text
Usa el servidor Playwright MCP para abrir un navegador en la aplicación que se ejecuta en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Borra el filtro y aplica uno de editorial. Confirma que la lista se actualiza para mostrar los juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados respetan ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante el servidor Playwright MCP, seguirá los pasos y te comunicará los resultados. Compara su informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más información o solicita que corrija el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP puede que tenga que descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de cambios

Después de confirmar que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisarla.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crea la solicitud de cambios

Hay varias formas de crear una solicitud de cambios, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. También puedes pedirle a Copilot que la cree: puede localizar la incidencia y asociarla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en la lista desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con los filtros por categoría y editorial. Crea una solicitud de cambios para la rama actual y asóciala a la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, puede pedirte que te autentiques.
5. Observa cómo Copilot busca las incidencias, localiza la adecuada y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud de cambios, pero **no la combines todavía**.

## Resumen y pasos siguientes

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo amplían sus servidores las herramientas de Copilot.
- añadiste el servidor Playwright MCP al espacio de trabajo.
- lo usaste para probar manualmente los filtros en un navegador antes de entregar el cambio.
- publicaste la rama y abriste una solicitud de cambios.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP ofrece otras formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
