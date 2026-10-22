---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones del repositorio e instrucciones para rutas concretas.
- generarás una primera versión de la capa de datos para filtrar editoriales con las instrucciones actuales.
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado aplica el nuevo estándar.
- confirmarás en `main` la actualización de las instrucciones y el trabajo de filtrado, para que Copilot pueda utilizar las nuevas directrices en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares. Copilot es probabilístico, no determinista. El objetivo es observar la tendencia después de actualizar las instrucciones, no obtener una salida idéntica palabra por palabra.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su contenido.

Usarás archivos de instrucciones para que Copilot disponga de la información necesaria para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a entender tu estilo de programación y tus requisitos. Puedes indicar convenciones, bibliotecas y los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en **cada** solicitud del repositorio. Debe incluir información general del proyecto, como la tecnología, una descripción, prácticas recomendadas y otras directrices.
- Los archivos `.github/instructions/*.instructions.md`, que puedes crear para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En los IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las siguientes sugerencias de edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también admite otros formatos de archivos de instrucciones, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Procedimientos recomendados para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo para organizar archivos de instrucciones:

- Mantén `copilot-instructions.md` centrado en la información general del proyecto, como una descripción, la estructura y los estándares globales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Escribe en lenguaje natural y mantén las directrices claras. Incluye ejemplos de código correcto e incorrecto.

No existe un único modo correcto de crear archivos de instrucciones o de usar IA. La experimentación te ayudará a descubrir qué funciona en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Este proyecto incluye instrucciones para [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece una opción distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para obtener plantillas o ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye el repositorio: hay un archivo principal `copilot-instructions.md` y otros archivos `*.instructions.md` para varias tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las instrucciones anidadas para **GitHub Actions Workflows**.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, coincide con los archivos de pruebas TypeScript, como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` recoge los estándares del proyecto, pero aún no exige documentación en el código. A continuación añadirás reglas para los comentarios TSDoc y los comentarios de cabecera.

## Crea una rama para los cambios

Crea una rama para separar los cambios de este ejercicio.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando sean relevantes.

Envía primero un prompt con el archivo de instrucciones actual. Después de actualizarlo, volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ir el helper.
3. En el selector de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar los cambios a los archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales y sus nombres e identificadores. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, a menudo en el helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un comentario de cabecera.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se lo hayas pedido. No hay problema: el objetivo es observar la mejora en la coherencia tras actualizar las instrucciones.

## Añade un estándar del repositorio

`.github/copilot-instructions.md` contiene información general para Copilot. Añade una directriz general para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Aún no incluye reglas para la documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Cada función exportada debe tener un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones o de cualquier código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Puedes crear instrucciones generales en `.github/copilot-instructions.md` o instrucciones para lenguajes, tipos de archivo o tareas concretos en archivos `*.instructions.md`. La directriz sobre comentarios TSDoc es un estándar general del proyecto.

## Vuelve a enviar el prompt y observa el cambio

Vuelve a enviar el prompt después de actualizar las instrucciones para comprobar cómo influye el nuevo estándar en el código generado.

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto de la conversación.
2. Vuelve a abrir `src/lib/publishers.ts` para indicar a Copilot qué archivo debe usar.
3. Envía el mismo prompt que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales y sus nombres e identificadores. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real que se usará en el ejercicio siguiente.

## Confirma los cambios de instrucciones y sube la rama

Los archivos de instrucciones se gestionan en el control de código fuente como cualquier otro archivo.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que las instrucciones y los cambios del helper estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra cambios complementarios (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de cambios

Crea una PR que resuelva la incidencia del backlog sobre las instrucciones. Copilot puede localizarla y abrir la PR con las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo. La primera vez que Copilot use una de ellas, VS Code puede pedirte que inicies sesión. Sigue las indicaciones.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que localice la incidencia relacionada con las instrucciones y cree una PR desde la rama actual. Indica que incluya tanto las instrucciones como el helper de editoriales:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que la PR cierra esa incidencia y menciona que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa la operación y selecciona **Allow** si procede.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para tener el repositorio actualizado.
   ```

7. Cuando Copilot solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa cada operación y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de cambios con GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- explorar las instrucciones del repositorio y las instrucciones específicas de rutas.
- enviar un prompt de generación de código y observar el resultado con las instrucciones actuales.
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto aplica el nuevo estándar.
- confirmar en `main` la actualización de instrucciones y el helper, para que el ejercicio siguiente pueda utilizarlos.

A continuación, aplicarás estas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad en el código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
