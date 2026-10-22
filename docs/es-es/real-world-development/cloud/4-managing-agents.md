---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado tres tareas al agente de GitHub Copilot en la nube para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas con PR abiertas, explorar las operaciones realizadas y guiar una sesión en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar más funcionalidad.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decide que también sería buen momento para añadir un modo claro. Como ya se están modificando los estilos del sitio y añadiendo opciones de tema, pedirás al agente que incluya el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube

Consulta el estado de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberían aparecer las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener una copia del código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios (PR), situado a la derecha. Permite consultar la PR y los archivos modificados.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre elegido por Copilot y del estado de la tarea.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Ya que estamos trabajando en el modo de alto contraste, añade también un modo claro. Debe haber un control para este modo, que permita a los usuarios seleccionar el modo de visualización que prefieran.
    ```

    ![Tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma el mensaje y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot actualizará la tarea. Incorporará la nueva solicitud cuando termine el paso que estaba ejecutando al recibir el mensaje.

El proceso tardará varios minutos. Puedes aprovechar para repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En este ejercicio has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas del agente de GitHub Copilot en la nube. Puedes hacer seguimiento de las tareas activas y terminadas, revisar el trabajo y redirigir una sesión en curso.

Has explorado estos conceptos:

- exploraste Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar más funcionalidad.

Mientras Copilot termina de implementar las funciones de accesibilidad, [iterarás sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
