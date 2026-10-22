---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones a medida que se realizan. Puedes revisar cada diff en línea, aceptar o rechazar cambios y orientar la conversación con mensajes de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios de archivo propuestos.
- guiarás al agente durante la sesión para añadir un modo claro junto con el modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación, guiarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos del terminal.
3. Cuando el agente proponga una edición, revisa el diff:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada un modo claro

Cuando el modo de alto contraste esté listo, amplía la misma conversación para añadir el modo claro. El agente conservará el contexto de su trabajo anterior, incluidos los componentes y estilos modificados y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía este mensaje:

    ```plaintext
    ¡Buen trabajo! Ahora añade un modo claro junto al modo de alto contraste. Sigue el mismo patrón: guarda la opción en el almacenamiento local y permite acceder a ella desde la misma zona de la interfaz. El usuario debe poder activar el modo claro y el de alto contraste de forma independiente.
    ```

2. El agente revisará los cambios anteriores y propondrá las modificaciones para añadir el modo claro.
3. Revisa cada edición nueva y selecciona **Keep** o **Undo** para aceptarla o descartarla.
4. Si el agente toma una dirección que no quieres, por ejemplo, si intenta reorganizar la interfaz de los controles, detenlo con el botón **Stop** junto al campo de entrada y envía una aclaración.

> [!TIP]
> Guiar una sesión en curso suele ser más rápido que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes («simplifica las hojas de estilo en cascada (CSS) y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («integra el modo claro en el panel de configuración existente, sin añadir un tercer control») o ampliar el alcance («añade también atajos de teclado para ambos modos»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa los diff. Confirma que siguen las prácticas de accesibilidad del agente personalizado: atributos de **Accessible Rich Internet Applications (ARIA)**, navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y guiar una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste mensajes de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste o rechazaste ediciones individuales.
- guiaste la sesión con mensajes de seguimiento.

En la [lección siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás las modificaciones realizadas por el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
