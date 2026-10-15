---
title: "Lección 9: Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras herramientas de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás algunos otros que pueden resultar útiles.

En esta lección:

- Utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Utilizarás `/model` para explorar los modelos disponibles.
- Aprenderás cómo `/share` permite exportar o compartir una sesión.
- Explorarás comandos opcionales para trabajar en paralelo con worktrees y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de manera opcional, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, pero también puedes inspeccionar o compactar el contexto manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y cómo se divide el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` de nuevo y compara el resultado. Si la conversación es breve, quizá no observes una diferencia importante.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Utiliza `/compact` para elegir el momento. Usa `/clear` o `/new` al cambiar a una tarea distinta y empezar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen capacidades diferentes y cada desarrollador tiene sus preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar el que quieras utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir aprendizajes ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, selecciona el destino adecuado, como `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones pueden incluir mensajes, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: distribuir tareas o delegarlas

Ya has completado el flujo principal del taller. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede repartir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear worktrees adicionales o iniciar trabajo remoto. Antes de probarlos, crea una tarea nueva y bien delimitada, y revisa los resultados siguiendo tu proceso habitual. Si quieres profundizar en el trabajo con agentes asíncronos, continúa con el [taller sobre el agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y conocer lo que sucede en segundo plano. En esta lección:

- Has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Has utilizado `/model` para explorar los modelos disponibles.
- Has aprendido cómo `/share` permite exportar o compartir sesiones.
- Has explorado comandos opcionales para tareas en paralelo, worktrees y delegación al agente en la nube.

Copilot CLI ofrece muchos otros comandos de barra. Para terminar, [repasarás lo aprendido][next-lesson] y explorarás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
