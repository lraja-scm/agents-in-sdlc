---
title: "Lección 1: Instala GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con el entorno y localiza la incidencia de filtrado del backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. Para empezar, instala la herramienta con una utilidad que probablemente ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- localizarás la incidencia de filtrado con el servidor GitHub MCP integrado.

## Escenario

Tu equipo empieza a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI integra esta capacidad en el terminal, donde ya trabajan muchos desarrolladores. En esta lección lo instalarás y autenticarás para usarlo durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y que cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o una posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación mediante código de dispositivo y autoriza Copilot CLI.
3. Copilot CLI muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace, puede que ya tengas una sesión iniciada con GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está todo listo.

## Familiarízate con el entorno

Los comandos que escribas en el prompt normal se ejecutan directamente en tu Codespace. Cuando Copilot CLI está abierto, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Introduce `/model`, usa las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Introduce `/help` para ver los comandos de la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz a Copilot una pregunta sencilla para comprobar que funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Introduce `/mcp list` y confirma que el servidor GitHub MCP integrado está disponible.
6. Pide a Copilot que localice la incidencia de filtrado:

   ```plaintext
   Usa GitHub MCP para encontrar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Dame su URL y un breve resumen. No hagas ningún cambio.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio breve.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea un worktree automáticamente. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el atajo del taller

Copilot CLI suele pedir permiso antes de usar herramientas. En este taller, volverás a iniciarlo con `--yolo`, un atajo que has autorizado para eliminar las solicitudes de aprobación dentro del Codespace y centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual (VM), y nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que usarás en lecciones posteriores para trabajar con incidencias y solicitudes de cambios. El Codespace limita el acceso al equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, ejecutarás `copilot --resume` para volver a la conversación de la funcionalidad y a su rama.

## Resumen y pasos siguientes

¡Enhorabuena! En esta lección:

- instalaste GitHub Copilot CLI con npm.
- te autenticaste con tu cuenta de GitHub.
- confiaste en el repositorio del taller y probaste una conversación.
- localizaste la incidencia de filtrado con el servidor GitHub MCP integrado.

A continuación, [harás tu primer cambio específico][next-lesson] y pedirás a Copilot CLI que muestre las valoraciones con estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
