---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una manera concreta o hay información de fondo que Copilot debe conocer, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] permiten orientar a Copilot para que entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- Explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones limitados a rutas.
- Añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás un mensaje antes y después. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que resulta más difícil mostrar el efecto de las instrucciones en tiempo real. Lo verás más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe incluir pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su contenido.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor tu estilo de programación y tus requisitos. Te permiten orientar sus sugerencias y los fragmentos de código que genera. Puedes especificar las convenciones de programación, las bibliotecas y hasta el tipo de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, pertinente para la mayoría de las solicitudes enviadas a Copilot Chat o CLI. Puede incluir la pila tecnológica, una descripción de lo que se está creando, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md`, que permiten crear directrices para tareas o tipos de archivo concretos. Por ejemplo, pueden incluir pautas para TypeScript o Astro, o instrucciones para crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat; no se usan para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Esta lección no cubre en detalle cómo crear archivos de instrucciones, pero el proyecto de ejemplo muestra un enfoque representativo. En general:

- Limita `copilot-instructions.md` a las directrices generales del proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares de programación globales.
- Utiliza los archivos `*.instructions.md` para dar instrucciones específicas sobre tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Utiliza lenguaje natural. Redacta pautas claras e incluye ejemplos del código que se debe y no se debe generar.

No existe una única manera correcta de crear archivos de instrucciones, del mismo modo que no existe una única forma correcta de utilizar la IA. La experimentación te permitirá descubrir qué funciona mejor para cada proyecto.

> [!TIP]
> Todos los proyectos que utilicen GitHub Copilot deberían disponer de un conjunto completo de archivos de instrucciones. Al explorar los archivos de este proyecto, verás directrices para varios tipos de tareas, como las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una forma distinta de hacerlo; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección de cada entorno lo explicará cuando corresponda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Todas ellas se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplica. En este caso, coincide con los archivos de prueba de TypeScript; por ejemplo, los que cumplen el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio. Esto permite dividir conjuntos de instrucciones extensos en archivos reutilizables más pequeños y señalar ejemplos que Copilot pueda seguir.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentar el código. En los pasos siguientes, añadirás reglas para incluir comentarios TSDoc y un encabezado de comentarios en cada archivo.

## Añadir un estándar para todo el repositorio

Ahora añadirás la única edición de este ejercicio: una regla general para que la documentación del código se incluya mediante comentarios TSDoc y un encabezado de comentarios por archivo. El agente en la nube la aplicará a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero aun así confirmarás los cambios de forma ordenada:

1. Desde el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade una regla a los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero todavía no incluye reglas de documentación en el código.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Cada función exportada debe tener un comentario TSDoc que describa su propósito, sus parámetros y el valor que devuelve.
   - Antes de las importaciones o de cualquier otro código, añade al archivo un bloque de comentarios que explique su propósito.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en la raíz del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, publicar y combinar las instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, Copilot creará una rama a partir de `main`; por tanto, los cambios de instrucciones deben llegar a `main` para que el agente los utilice.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente en la nube podrá leer estas instrucciones al trabajar en las incidencias que le asignes más adelante.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir los pasos de la rama y confirmar los cambios en `main`. El ejercicio utiliza una rama para imitar el flujo de trabajo habitual en un proyecto.

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se refleja ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones personalizadas y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
