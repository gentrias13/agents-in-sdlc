---
title: "Ejercicio 4: Gestión y supervisión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente de Copilot en la nube tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como [la página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes ver todas las tareas del agente que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones que ha realizado e incluso dirigir una sesión que sigue activa.

En esta lección:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo decidió añadir también un modo claro. Como ya se estaban actualizando los estilos del sitio y añadiendo controles de alternancia, pareció razonable incluir esta función. Dirigirás el trabajo del agente para pedirle que añada el modo claro junto con el modo de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Comprueba el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberían aparecer las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar los juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot. Por ejemplo:
   - Obtener el código del repositorio.
   - Preparar el entorno de Copilot.
   - Configurar servidores MCP.
   - Realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados.

## Dirigir el agente en la nube

Ya has visto las tareas activas. Ahora pedirás a Copilot que añada un control para alternar al modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título exacto dependerá del nombre que haya utilizado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de preparar el entorno y ha empezado a trabajar. Lo notarás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    Mientras trabajamos en un modo de alto contraste, añadamos también un modo claro. Debe haber un control para alternar a este modo y permitir que las personas seleccionen cómo quieren ver el sitio.
    ```

    ![Tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo reconoce y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot actualizará la tarea para incorporar la nueva solicitud. Como en el ejercicio anterior, continuará cuando haya terminado el paso que estaba realizando al enviar el mensaje.

Esto tardará unos minutos. Puedes aprovechar la pausa para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente de GitHub Copilot en la nube. Desde aquí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para cambiar su alcance o proporcionarles más instrucciones.

Has aprendido:

- a utilizar el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- a redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [revisar las solicitudes de incorporación de cambios que ha creado e iterar sobre ellas][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
