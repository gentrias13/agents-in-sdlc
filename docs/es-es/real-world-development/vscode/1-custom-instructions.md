---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta o hay información de fondo que Copilot debe conocer, asegúrate de que pueda acceder a ella. Hay varias formas de proporcionar contexto específico a Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que orientan la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones limitados a rutas.
- generarás la primera parte de los datos para el filtrado (un método auxiliar para editoriales) con las instrucciones actuales.
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.
- volverás a enviar el mismo mensaje y observarás cómo el código generado aplica el nuevo estándar.
- confirmarás los cambios de las instrucciones y el método auxiliar de filtrado en `main` para que Copilot utilice las pautas actualizadas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que hayas definido. Copilot es probabilístico. El objetivo es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe incluir pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su contenido.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor tu estilo de programación y tus requisitos. Te permiten orientar las sugerencias y los fragmentos de código que genera. Puedes especificar las convenciones de programación, las bibliotecas e incluso el tipo de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes a Copilot Chat o CLI, como la pila tecnológica, una descripción del trabajo, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md`, que permiten crear directrices para tareas o tipos de archivo concretos, como TypeScript o Astro, o para crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat; no se usan para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Esta lección no cubre en detalle cómo crear archivos de instrucciones, pero el proyecto de ejemplo muestra un enfoque representativo. En general:

- Limita `copilot-instructions.md` a las directrices generales del proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares de programación globales.
- Utiliza archivos `*.instructions.md` para dar instrucciones específicas sobre tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Utiliza lenguaje natural. Redacta pautas claras e incluye ejemplos del código que se debe y no se debe generar.

No existe una única manera correcta de crear archivos de instrucciones, del mismo modo que no existe una única forma correcta de utilizar la IA. La experimentación te permitirá descubrir qué funciona mejor para el proyecto y el equipo.

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

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas para **GitHub Actions Workflows**. Todas ellas se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un patrón glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript; por ejemplo, los que cumplen el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio. Así se pueden dividir instrucciones extensas en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentar el código. En los pasos siguientes añadirás reglas para incluir comentarios TSDoc y un encabezado de comentarios en cada archivo.

## Crear una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para tus cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo dependen del ejercicio; se indicarán cuando sean importantes.

Para observar el efecto de las instrucciones personalizadas, primero envía un mensaje con el archivo de instrucciones actual. Más adelante, actualizarás el archivo y volverás a enviar el mismo mensaje para observar la diferencia.

1. Cierra las pestañas del editor que tengas abiertas para que Copilot utilice únicamente el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse el método auxiliar.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un método auxiliar de acceso a datos que devuelva una lista de todas las editoriales con su nombre e identificador. Aplica los cambios en el archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo en el archivo auxiliar y sus pruebas.
6. Observa que el método auxiliar propuesto es una función tipada que recibe primero un cliente `db` y devuelve una matriz tipada de editoriales. Esta estructura procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Fíjate en que al código propuesto **le faltan** comentarios TSDoc y un encabezado de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, quizá añada comentarios de documentación incluso sin que se lo pidas. Si ocurre, no pasa nada: el objetivo es observar la *mejora de coherencia* después de actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

Como ya se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade una pauta a los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero todavía no contiene pautas para documentar el código; por eso el método auxiliar generado no incluía comentarios de documentación.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Cada función exportada debe tener un comentario TSDoc que describa su propósito, sus parámetros y el valor que devuelve.
   - Antes de las importaciones o de cualquier otro código, añade al archivo un bloque de comentarios que explique su propósito.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en la raíz del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la pauta de comentarios de documentación que acabas de añadir.

## Volver a enviar el mensaje y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto de la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo mensaje** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un método auxiliar de acceso a datos que devuelva una lista de todas las editoriales con su nombre e identificador. Aplica los cambios en el archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Métodos auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información sobre editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con sus identificadores y nombres.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real que se utilizará en el siguiente ejercicio.

## Confirmar las instrucciones y publicar la rama

Los archivos de instrucciones son recursos del repositorio como cualquier otro y se gestionan con el mismo sistema de control de versiones.

1. Abre una ventana de terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba en el terminal que los cambios de las instrucciones y del método auxiliar están presentes:

   ```bash
   git status
   ```

3. Prepara y confirma el cambio de instrucciones y todos los archivos que Copilot haya modificado para crear la base del método auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios vinculada a la incidencia sobre los estándares de documentación del backlog. Pide a Copilot que localice la incidencia, cree una PR que la cierre y la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub en el espacio de trabajo, así que no hace falta preparar nada. La primera vez que Copilot utilice una herramienta, VS Code quizá te pida iniciar sesión en GitHub. Sigue las indicaciones. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que localice la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indica que la PR debe describir tanto las actualizaciones de instrucciones como la base del nuevo método auxiliar para editoriales:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una nueva PR desde la rama actual e indica que la PR cierra esa incidencia. Añade también que hemos creado la base del método auxiliar de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a localizar la incidencia y a crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa la operación y selecciona **Allow** si procede.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a la rama `main` local:

   ```plaintext
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para que el repositorio esté actualizado.
   ```

7. Cuando Copilot te pida permiso para realizar acciones en GitHub y ejecutar comandos de shell, revisa la operación y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has utilizado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto aplica el nuevo estándar.
- confirmar los cambios de instrucciones y la base del método auxiliar en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones personalizadas y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
