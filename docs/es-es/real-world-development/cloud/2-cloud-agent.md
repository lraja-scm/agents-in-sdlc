---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Pocas organizaciones están libres de deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizarse o funcionalidades que siguen en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una PR preliminar para que un desarrollador la revise. Así, el equipo puede delegar tareas repetitivas y centrarse en trabajos de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno para generar código.
- garantizar que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio no añadieron documentación. Como primer paso, el equipo quiere que se añadan comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar la búsqueda de juegos. Cada página de detalles debería mostrar otros juegos de la misma categoría para que la gente pueda seguir explorando. No se necesita una implementación pulida; basta con algo que permita probar y evaluar la experiencia de usuario. Aunque la tarea está pendiente, hay otras incidencias de mayor prioridad.

Ambas tareas pueden quedar relegadas y son buenas candidatas para el agente de GitHub Copilot en la nube. Este puede trabajar de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, más tarde, revisa el resultado.

## Presentación del agente de GitHub Copilot en la nube

[El agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una PR en borrador para llevar el seguimiento del trabajo, prepara el entorno y empieza a trabajar. Puedes consultar la sesión durante su ejecución o cuando termine. Cuando la solución esté lista para revisión, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no es mágico: no puedes saltarte la planificación. Hasta las tareas que parecen sencillas pueden tener matices; por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. La IA generativa no cambia los principios básicos del desarrollo de software.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio sobre instrucciones personalizadas, conoces `.github/copilot-instructions.md`; si no, puedes consultarlo ahora. Antes de asignar tareas al agente, revisa los archivos de instrucciones del repositorio para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, busca en la PR comentarios TSDoc, convenciones TypeScript y comentarios de cabecera; esas directrices proceden de los archivos de instrucciones.

## Configura el entorno de desarrollo del agente en la nube

Crear código requiere un entorno y, normalmente, scripts de configuración para asegurarse de que todo está listo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`. Se ejecuta antes de que empiece el trabajo y proporciona herramientas y dependencias. El flujo ya está configurado para este laboratorio con Node.js, las dependencias del proyecto, los navegadores de Playwright para pruebas de extremo a extremo y la base de datos SQLite migrada y sembrada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos que necesiten *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que lo ejecute Copilot.

## Añade documentación

Muchos proyectos tienen información obsoleta o carecen de documentación. Esta deuda técnica ralentiza el trabajo y dificulta el mantenimiento o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta incidencia es apropiada para el agente en la nube. En un ejercicio posterior volverás para revisar su trabajo.

1. Abre el repositorio en github.com en una nueva pestaña del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Falta documentación en el código` en el campo **Title**.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios donde aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR recién generada, que tendrá un título parecido a `[WIP]: Falta documentación en el código`. Si no aparece, espera un minuto o dos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando su entorno (ejecuta el flujo de trabajo anterior), analizando el proyecto y definiendo el enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR y localiza el aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot explora el código, analiza el estado del proyecto, planifica y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecha esta capacidad para asignarle otra tarea y, mientras trabaja, añade una funcionalidad a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

El agente en la nube permite repartir el trabajo: tú puedes ocuparte de una tarea mientras el agente trabaja en otra. Asigna al agente la tarea de añadir una sección con juegos relacionados.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Muestra juegos relacionados en la página de detalles` en **Title**.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario (UX) y hacer algunas pruebas de aceptación. Estos son los requisitos:

- Añade un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego dado, sin incluir ese juego.
- Muestra una sección «Juegos relacionados» en la página de detalles que use el helper.
- Gestiona el caso en que un juego no tenga juegos relacionados.
- Crea pruebas unitarias para el nuevo helper.
- Antes de crear la PR, comprueba que se superan todas las pruebas.
    ```

7. Selecciona **Create**.
8. En el panel derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás un emoji 👀 en el primer comentario de la incidencia, que indica que Copilot está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software; no hace falta supervisarlo continuamente. Revisa el resultado cuando termine. Mientras tanto, explora cómo crear y usar agentes personalizados.

## Resumen y pasos siguientes

En este ejercicio has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona: corregir deuda técnica, crear funcionalidades o migrar código a otro framework.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. [El agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de configuración del agente][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
