---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan dificultades con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás cómo utilizar el agente de GitHub Copilot en la nube para:

- personalizar el entorno en el que genera código.
- garantizar que las operaciones se realizan de forma segura.
- comprender la importancia de definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere abordar parte de su deuda técnica. El equipo que desarrolló la primera versión del sitio dejó la documentación en mal estado; de hecho, verás que no existe. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la manera de descubrir juegos. Quiere mostrar en la página de detalles de cada juego otros títulos de la misma categoría para que la gente pueda seguir explorando. Todavía no hace falta una implementación pulida: solo quieren algo que les permita probar la experiencia de usuario y realizar pruebas de aceptación. Aunque esta tarea bloquea el trabajo, ahora mismo hay otras incidencias con mayor prioridad.

Este tipo de tareas se suele aplazar y es ideal asignárselo al agente de Copilot en la nube. Puede trabajar de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, más tarde, vuelve para revisar el trabajo y comprobar que todo esté correcto.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Puedes, por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea un borrador de solicitud de incorporación de cambios para registrar el progreso, prepara el entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras está en curso o después de que termine. Cuando la solución propuesta esté lista para revisar, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de delimitar bien las tareas

GitHub Copilot no hace magia: no puedes dejar de pensar en lo que necesitas. Incluso las operaciones aparentemente sencillas pueden tener cierta complejidad, así que [define cuidadosamente el alcance de las tareas que asignes al agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, puedes echarle un vistazo ahora. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones del repositorio sin modificarlos para que puedas observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente en la nube la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios de la solicitud de incorporación de cambios. Estos proceden de los archivos de instrucciones y volveremos a mencionarlos al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

Crear código suele requerir un entorno concreto y ejecutar scripts de configuración para comprobar que todo está listo. Lo mismo ocurre al asignar tareas a Copilot, que las realiza de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para configurar su entorno de trabajo. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller: proporciona Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna solo los permisos mínimos que necesitan los pasos. Copilot recibe su propio token para realizar operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, se necesita el permiso contents: read. Si no se clona el repositorio, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (toda la aplicación utiliza Astro y Drizzle/libSQL)
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

      # Migra y carga datos de prueba en la base de datos SQLite local para las compilaciones y pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo es parecido a cualquier otro flujo de trabajo de GitHub, pero hay dos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, así que esta es una incidencia ideal para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando ayuden a entender el código. Sin embargo, las actualizaciones recientes no han seguido este estándar. Hay que actualizar el código existente e incluir comentarios de documentación donde aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que acaba de generar Copilot. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera uno o dos minutos y vuelve a cargar la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot tiene previsto completar.
14. Desplázate por la cronología de la solicitud. Deberías ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que volver a cargar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve el problema: explora el código y averigua el estado del proyecto, se detiene a pensar y decidir un plan, y crea código.

El trabajo probablemente tardará varios minutos. Uno de los principales objetivos del agente de Copilot en la nube es permitirle trabajar de forma asíncrona mientras tú te ocupas de otras tareas. Aprovecharemos esa posibilidad para asignarle otra tarea y, mientras tanto, escribir código que añada funciones a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las principales ventajas del agente de GitHub Copilot en la nube es que permite dividir el trabajo: tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero podrías dedicarlo a otros trabajos. Vamos a asignársela al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para utilizar una plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir un método de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, excluyendo ese juego
   - Mostrar en la página de detalles una sección "Related games" que utilice el método
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo método
   - Comprobar que todas las pruebas se superan antes de crear la PR
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás unos 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de Copilot en la nube funciona de forma parecida a un ingeniero de software; no hace falta supervisarlo constantemente. Revisa el resultado cuando termine. Mientras tanto, vamos a crear y utilizar agentes personalizados.

## Resumen y próximos pasos

En este ejercicio has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona, ya sea para abordar la deuda técnica, añadir funcionalidades o migrar código de un framework a otro.

Has aprendido a:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realizan de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes crear y utilizar agentes personalizados. [El agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el agente de Copilot en la nube][cloud-agent-best-practices]

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
