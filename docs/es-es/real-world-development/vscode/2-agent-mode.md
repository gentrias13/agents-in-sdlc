---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos en una aplicación suelen requerir actualizar varios archivos y realizar operaciones como ejecutar pruebas. El flujo de un desarrollador normalmente implica localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a actualizarlo. Y así sucesivamente.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía dentro del IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo utilizar el modo agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente trabaja en tu IDE y permite recibir comentarios e interactuar con rapidez. El agente en la nube trabaja de forma asíncrona como un compañero del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- dejar que el modo agente de Copilot explore el proyecto, encuentre los archivos pertinentes y coordine los cambios.
- utilizar el modo agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente antes de incorporarlos al código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior añadiste un método auxiliar para editoriales. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una aplicación única de Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle ORM. No hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los métodos reutilizables de acceso a datos se encuentran en `src/lib/`, y las interacciones se implementan con pequeños bloques `<script>` de Astro, limitados al ámbito correspondiente y basados en las API estándar del DOM.

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. En GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal en Codespaces con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. En el terminal, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` para abrir el sitio web.

> [!NOTE]
> En un codespace, al seleccionar un enlace localhost en el terminal de Codespaces, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio en ejecución, explora sus funciones principales:

- **Página principal**: muestra los juegos de mesa, con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para empezar una conversación limpia. El modo y el modelo dependen del ejercicio; se indicarán cuando sean importantes.

La implementación inicial del sitio funciona, pero queremos añadir capacidades nuevas. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un conjunto de incidencias. Pide a GitHub Copilot que las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos en el terminal y utiliza herramientas.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no prefieren ningún modelo en particular. Al crear el taller se utilizó Claude Sonnet 4.5, por lo que se incluye en estas instrucciones. Así se espera que las sugerencias de código sean relativamente coherentes y se ofrezca una experiencia adecuada. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de las que se muestran aquí. Es normal y previsible.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot quizá utilice un comando MCP diferente, pero aun así debería completar la tarea.

3. Envía este mensaje a Copilot para que consulte el backlog:

   ```plaintext
   Muéstrame las incidencias pendientes de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot también ha priorizado las incidencias según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales para añadir funcionalidad, incluidas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para distintos tipos de archivo, como:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar las directrices, dividir instrucciones complejas en partes más pequeñas y manejables, y señalar plantillas y ejemplos.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, hay que realizar al menos tres cambios distintos en la aplicación:

- Añadir o completar la lógica de filtrado de la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas del comportamiento de filtrado.
- Actualizar la página que enumera los juegos para añadir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y comprobar que se superan antes de incorporar los cambios al código base. El modo agente de Copilot puede encargarse de todo esto. Vamos a añadir la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de ejemplo para seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Envía este mensaje para pedir a Copilot que implemente la funcionalidad descrita en la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para permitir filtrar por editorial y categoría, según los requisitos de la incidencia de GitHub correspondiente del backlog. Ya existe un método auxiliar para editoriales del ejercicio anterior, así que consérvalo y mejóralo según sea necesario para completar el trabajo restante de la capa de datos, la interfaz y las pruebas. Comprueba que se superan todas las pruebas antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar la capa de datos, la interfaz y las pruebas. A continuación, empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos, según la complejidad de la tarea y del código. Durante el proceso, quizá aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando Copilot termine, verás la opción **Keep** o **Undo** para cada cambio, así que no hace falta seleccionarlos mientras trabaja.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se detenga por momentos e incluso que algunas pruebas fallen. No pasa nada: Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio web y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y próximos pasos

En este ejercicio has utilizado el modo agente de GitHub Copilot para añadir nuevas capacidades al sitio web de Tailspin Toys. Has aprendido que:

- el modo agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos.
- el modo agente puede explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- hay que revisar los cambios y las pruebas que genere el modo agente antes de incorporarlos al código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crece la lista de juegos, será necesario añadir paginación. Utiliza lo que has aprendido en este ejercicio y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones del proyecto.
- Decide cómo debería funcionar la paginación; por ejemplo, si el usuario puede seleccionar el tamaño de página o si será fijo.
- Al redactar el mensaje, proporciona a Copilot las pautas necesarias para conseguir la implementación que quieres.
- Quizá debas iterar con GitHub Copilot, pedirle cambios y añadir contexto. Este es el flujo de trabajo normal al utilizar Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
