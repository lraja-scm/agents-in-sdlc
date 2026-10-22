---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le pediste que añadiera documentación al código, creara una sección de juegos relacionados y desarrollara funciones de accesibilidad, como los modos de alto contraste y claro. Ahora revisarás los cambios y, si es necesario, pedirás a Copilot que los mejore.

### Escenario

Los principios básicos del diseño de software y DevOps no cambian con la llegada de la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisa las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

El agente de GitHub Copilot en la nube trabaja de forma asíncrona y sin supervisión directa. Para mantener la seguridad, se aplican algunas restricciones:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para trabajar.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno efímero e independiente.
- Un usuario debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de cambios (PR) creada por el agente en la nube: la documentación del código. Revisarás la PR con la interfaz habitual de GitHub.com.

> [!NOTE]
> Puede aparecer un aviso que indica que un firewall ha bloqueado GitHub Copilot. Es **lo esperado**, porque el acceso del agente en la nube a recursos externos, incluidos los servidores MCP, está limitado de forma predeterminada. Si lo necesitas, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye comentarios TSDoc y otra documentación. Los cambios exactos pueden variar.

   Busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si aparece un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR, como harías con otros miembros del equipo, o en comentarios insertados en el código. Copilot leerá los comentarios e iniciará otra sesión para atenderlos. Como los resultados no son deterministas, no hay un texto único que debas usar. Por ejemplo, puedes pedirle que:

- añada a cada archivo de código un comentario de cabecera que describa brevemente su contenido.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como lo harías con cualquier otro usuario. Puedes usar una de las ideas anteriores u otra sugerencia relacionada con la documentación.
2. Selecciona **View Session** para observar cómo Copilot realiza los cambios. Copilot iniciará una sesión nueva.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te satisfaga. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si aparece un indicador de flujos de trabajo pendientes de aprobación.
7. Si es así, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro se completarán correctamente. Puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad. Ve a tu Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama de Copilot, por ejemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la sección de juegos relacionados. Puedes seguir iterando sobre el trabajo del agente en la nube con comentarios nuevos en la pestaña **Conversation**. Por ejemplo, puedes pedir que se ajuste el número de juegos relacionados o que añada comentarios de cabecera y TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste en Mission Control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de alternancia de los modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de las Aplicaciones de Internet Enriquecidas Accesibles (ARIA), como sus etiquetas y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro se completarán correctamente. Puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar las funciones de accesibilidad. Abre un terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Confirma que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha guiado a Copilot para implementar estas funciones siguiendo prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionarlo en un comentario con **@copilot**, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando trabajo

El agente en la nube es útil para trabajar en incidencias reales del backlog mientras tú te ocupas de otras tareas. Para practicar, crea algunas incidencias en el repositorio y asígnalas a Copilot. Por ejemplo:

- crear un formulario de interés de patrocinadores en la página de detalles de un juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los métodos de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizadas** del repositorio para comprobar más adelante su efecto en el resultado del agente.
- **Asignaste incidencias al agente de GitHub Copilot en la nube** y observaste cómo configuraba el entorno, planificaba y ejecutaba el trabajo de forma asíncrona.
- **Creaste y usaste un agente personalizado** para accesibilidad, con modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Revisaste e iteraste sobre las PR del agente**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Repaso y pasos siguientes

Has completado el entorno del agente en la nube. Para seguir explorando, puedes probar los otros entornos:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y la opción de delegar tareas al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro dominio.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de configuración del agente][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
