---
title: "Lección 2: Añade las valoraciones con estrellas como primer cambio rápido"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revisarlo en un navegador reenviado y combinarlo en tu primera solicitud de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo combinarás en tu primera solicitud de cambios.

En esta lección:

- iniciarás una conversación centrada en una rama de funcionalidad.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación y comprobarás el resultado en un navegador reenviado.
- abrirás y combinarás tu primera solicitud de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que tienes seleccionados en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas permiten volver al mismo diálogo, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tus mensajes y las respuestas del agente, la actividad del agente mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para solicitar el cambio:

   ```plaintext
   Muestra `starRating` sobre 5 en las tarjetas de juegos de la página de listado. Si la valoración es `null`, muestra «Sin valoración». Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa el diff

Debes revisar todos los cambios generados por IA antes de combinarlos, aunque sean pequeños. Examina los cambios desde Copilot CLI.

1. Introduce `/diff` e inspecciona cada archivo modificado.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y «Sin valoración» cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla del diff.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, ofrece resultados probabilísticos, no deterministas. El código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica a un ejemplo.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para poder examinar la interfaz de usuario (IU) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda comprobar en el navegador el cambio de la valoración con estrellas. Dime la URL y deja el servidor en marcha.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y combina tu primera solicitud de cambios

Ya has creado la funcionalidad. Ahora crearás una solicitud de cambios (PR, por sus siglas en inglés) para combinar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones con estrellas con un mensaje de confirmación adecuado.
   ```

2. Introduce `/pr create`. Copilot CLI sube la confirmación existente al crear la PR y muestra la URL de la PR.
3. Abre la PR; para ello, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

¡Enhorabuena! Has entregado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación de Copilot centrada en una rama de funcionalidad.
- dirigiste al agente para que hiciera un pequeño cambio en las tarjetas de juegos.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación y comprobaste las valoraciones en un navegador reenviado.
- abriste y combinaste tu primera solicitud de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
