---
title: "Lección 5: Personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks, personaliza el formato de sus resultados y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desarrollar software no consiste solo en escribir código. Ya has validado manualmente que el código funciona y has usado archivos de instrucciones para asegurarte de que sigue los estándares del equipo. ¿Y las pruebas, el lint y las demás tareas de integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son una buena opción. Ayudan a Copilot a ejecutar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son carpetas que contienen un archivo Markdown llamado `SKILL.md`. El archivo incluye frontmatter con un nombre y una descripción, una presentación de lo que hace la habilidad e indicaciones sobre cuándo debe utilizarse. La carpeta también puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` dentro de un proyecto, para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que el equipo de Tailspin Toys ha creado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Observa que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de modificarla

Puedes invocar habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Observa el informe final.

## Personaliza el informe

Mejora el informe para que indique qué se ha ejecutado, si se ha superado y qué han comunicado realmente las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Al terminar, indica cada comando que se haya ejecutado y si se ha completado correctamente, ha fallado o se ha bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta las proporciona. Indica el siguiente paso para cada error o bloqueo y nunca describas una comprobación omitida o incompleta como superada.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin que tengas que reiniciar la conversación. Comprueba cómo funciona la nueva sección.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Usa el mismo prompt de antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar la habilidad y la ejecutaste.

Ese cambio acompañará a los filtros en la PR de la funcionalidad. A continuación, permitirás a Copilot interactuar directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son recursos de consulta, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas en navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
