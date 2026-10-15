---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo del taller has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le has pedido que documente el código, implemente una función para mostrar juegos relacionados que el equipo de diseño pueda revisar e incorpore mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al utilizar IA generativa. Siempre hay que revisar el código generado y seguir el proceso normal de DevOps. Antes de habilitar la revisión para el resto del equipo, revisarás las propuestas de GitHub Copilot para documentar el código, mostrar juegos relacionados y mejorar la accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido límites de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que utiliza para su código.
- El agente se ejecuta en GitHub Actions, en un entorno independiente y temporal.
- Las acciones de los flujos de trabajo de GitHub requieren la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que ha generado el agente de Copilot en la nube: la que añade documentación al código. La revisarás con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, quizá veas un aviso que indica que GitHub Copilot está bloqueado por un firewall. Es **normal**, ya que Copilot tiene un acceso limitado a los recursos externos, incluidas las llamadas a servidores MCP. Si quieres, puedes [configurar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en una pestaña nueva de github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la PR titulada algo parecido a **Add missing documentation** o un título más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando en la tarea, la PR mostrará la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos. Mientras tanto, puedes descansar o repasar lo aprendido hasta ahora.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye nuevos comentarios TSDoc y otra documentación. Los cambios exactos pueden variar.

    Comprueba si se han añadido comentarios TSDoc, se han respetado las convenciones de TypeScript y se han creado encabezados de comentarios. Estos cambios siguen los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando termines la revisión, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un aviso de que algunos flujos de trabajo esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente. Puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

Trabajar con Copilot en una PR no es una interacción unidireccional. Puedes mencionarlo en los comentarios de la solicitud o en comentarios insertados en el código, igual que harías con otra persona del equipo. Copilot verá esos comentarios e iniciará otra sesión para responder. Como los resultados no son deterministas, no hay un mensaje concreto que debas utilizar. Puedes pedirle, por ejemplo:

- Que añada a cada archivo de código un encabezado breve que describa su contenido.
- Que añada comentarios TSDoc a los archivos TypeScript y Astro.
- Que cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario solicitando que se actualice la documentación generada y menciona a **@copilot**. Puedes utilizar una de las ideas anteriores u otra sugerencia.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Botón Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado, aparecerá una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Acción Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot ha generado para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o más descriptivo.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Comprueba si hay un aviso de que algunos flujos de trabajo esperan aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente. Puede tardar unos minutos.
9. **Opcional:** Puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre el terminal de Codespaces y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Igual que antes, puedes pedir al agente de Copilot en la nube que realice cambios mediante comentarios nuevos en la pestaña **Conversation**. Por ejemplo, puedes cambiar el número de juegos relacionados o pedir encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisarás las funciones de accesibilidad implementadas por el agente personalizado de accesibilidad. Esta PR debe incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste en Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando en la tarea, la PR mostrará la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - Los controles para cambiar entre los distintos modos.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios de CSS o de estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - El código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un aviso de que algunos flujos de trabajo esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente. Puede tardar unos minutos.
10. **Opcional:** Puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal de Codespaces y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba en el navegador los controles de alto contraste y modo claro. Comprueba que funcionan y que mantienen la preferencia al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo prácticas recomendadas de accesibilidad. Si detectas algún problema o posible mejora, puedes mencionarlo en un comentario con **@copilot**, igual que hiciste en las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puede encargarse de incidencias reales del backlog mientras tú haces otras tareas. Para practicar, crea algunas incidencias para tu repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para que las personas que apoyan el proyecto manifiesten su interés en la página de detalles de cada juego.
- Implementar paginación en la lista de juegos.
- Añadir validación de entradas y gestión de errores a los métodos de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para comprobar cómo influyen en los resultados del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Has revisado las solicitudes de incorporación de cambios del agente e iterado sobre ellas**, mencionando a `@copilot` para pedir cambios y aprobar flujos de trabajo.

## Revisión y próximos pasos

Has completado el recorrido del agente en la nube. Los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y la delegación opcional a la nube con `/delegate`.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

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
