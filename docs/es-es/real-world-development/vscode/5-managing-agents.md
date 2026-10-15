---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat de VS Code muestra cada llamada a herramientas y cada edición mientras ocurre. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con mensajes de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas para los archivos.
- dirigirás al agente durante la sesión para que añada un control de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha añadido una petición: también quiere un modo claro para las personas a quienes les parece demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, dirigirás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio aparece en línea en el chat junto con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cómo se realizan las llamadas a herramientas. Aparecerán lecturas y ediciones de archivos, además de comandos del terminal que necesiten ejecutarse.
3. Cuando el agente proponga una edición, aparecerá una diferencia en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para consultar todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Dirigir al agente para añadir el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un control de modo claro. Como continúas en el mismo chat, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el mecanismo de almacenamiento local.

1. En el campo de Copilot Chat, envía este mensaje de seguimiento:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control para el modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar de forma independiente el modo de alto contraste y el modo claro.
    ```

2. El agente continuará desde el trabajo anterior, explorará los cambios existentes y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nuevo cambio según aparezca, como hiciste con el modo de alto contraste. Selecciona **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, si intenta reorganizar la interfaz de los controles—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviarle una aclaración.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Envía mensajes de seguimiento para pedir ajustes («simplifica el CSS; utiliza variables CSS en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos modos»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba que se han seguido las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo del agente en tiempo real.
- aceptar o rechazar ediciones individuales.
- dirigir una sesión en curso mediante mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás tu trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los resultados del agente personalizado local generados durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
