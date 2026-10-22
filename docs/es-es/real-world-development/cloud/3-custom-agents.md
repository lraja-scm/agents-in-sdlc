---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados en tareas o áreas concretas del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables, por ejemplo, un agente de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], un agente de seguridad o un agente de pruebas.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos puntos en común. Ambos suelen definirse mediante archivos Markdown y explican a la IA cómo realizar tareas. Una forma sencilla de diferenciarlos es considerar que el **agente personalizado** es quien ejecuta el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Para hacerlo, podría utilizar habilidades de flujo de trabajo de PR o de ejecución y gestión de pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera para encontrar el enfoque adecuado para tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que el tema oscuro actual resulta difícil de leer por el escaso contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para implementar la funcionalidad con rapidez.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y abre el archivo del agente personalizado:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en el frontmatter de los agentes personalizados.

3. Revisa las secciones siguientes, que contienen:
   - responsabilidades principales al generar código para un sitio accesible.
   - prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

Mission Control es el espacio central para trabajar con agentes: permite asignar tareas al agente en la nube, supervisarlas y proporcionar indicaciones adicionales. Asigna a Copilot la tarea de añadir el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Añade un modo de alto contraste al sitio web` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    Necesitamos un modo de alto contraste para el sitio. Debe haber un control para activarlo y desactivarlo. El ajuste debe guardarse en el almacenamiento local del navegador.
    ```

7. Selecciona **Create**.
8. En el panel derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Asignación del agente en la nube con el agente personalizado Accessibility seleccionado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y pasos siguientes

En este ejercicio has explorado los [agentes personalizados][custom-agents], asistentes especializados de GitHub Copilot para tareas y áreas concretas. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y guiar la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
