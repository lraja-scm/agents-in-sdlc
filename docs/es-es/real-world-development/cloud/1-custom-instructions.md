---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta, o si Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo qué debe hacer, sino también cómo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones del repositorio e instrucciones para rutas concretas.
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no ejecutarás un prompt antes y después para comparar resultados. El agente de GitHub Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar su efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las PR que genere.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene unas directrices para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su contenido.

Usarás archivos de instrucciones para que Copilot disponga de la información necesaria para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a entender tu estilo de programación y tus requisitos. Puedes indicar convenciones, bibliotecas y los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en **cada** solicitud del repositorio. Debe incluir información de ámbito general, como la tecnología, una descripción del proyecto, las prácticas recomendadas y otras directrices.
- Los archivos `.github/instructions/*.instructions.md`, que puedes crear para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En los entornos de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las siguientes sugerencias de edición.
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
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Al explorar este proyecto, encontrarás instrucciones para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece una opción distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente indicará cuándo usarla.
>
> Para obtener plantillas o ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y otros archivos `*.instructions.md` para varias tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las instrucciones anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, coincide con los archivos de pruebas TypeScript, como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Esto permite dividir instrucciones extensas en archivos reutilizables más pequeños y ofrecer ejemplos a Copilot. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` recoge los estándares del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de archivo.

## Añade un estándar del repositorio

Ahora añadirás una regla general para que la documentación figure como comentarios TSDoc y como un bloque de comentario al principio de los archivos. El agente en la nube usará esta directriz con las incidencias que le asignes en ejercicios posteriores.

Antes de editar, crea una rama para trabajar. El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia, pero también confirmarás los cambios de forma ordenada.

1. Desde el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade allí los estándares generales de programación.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que aún no incluye reglas para la documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Cada función exportada debe incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones o de cualquier código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto, puedes crear instrucciones en `.github/copilot-instructions.md` para definir directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas concretos. Las directrices generales del proyecto, como la regla de comentarios TSDoc, pertenecen al archivo del repositorio.

## Confirma, sube y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, Copilot partirá de `main`; por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda usarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una PR desde `custom-instructions-cloud` hacia `main` y combínala. Así, el agente en la nube podrá leer estas instrucciones en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama se usa para reproducir el flujo de trabajo de un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has añadido un estándar general del repositorio a `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las PR que genere el agente en la nube en el resto del taller.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
