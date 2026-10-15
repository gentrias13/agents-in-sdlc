---
title: "Lección 10: Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo con Tailspin Toys. En concreto:

- Has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- Has añadido valoraciones por estrellas, has revisado el resultado en un navegador reenviado y has combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- Has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, desarrollado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- Has orientado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- Has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real.
- Has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- Has revisado el cambio completo del filtrado y autorizado Agent Merge para la PR correspondiente.
- Has explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Cambios publicados

El taller incluye dos entregas principales en PR diferentes, cada una en una rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas:** muestra la propiedad `starRating` existente y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y proceso de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos reunido el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, revisando el sitio en el navegador y pidiendo a Copilot que lo explorase mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha mostrado los resultados con el nuevo formato. QA ha reunido esos resultados con la revisión de los requisitos y la cobertura de las pruebas antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales. Si una ejecución de QA determina que no hace falta añadir pruebas, puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas de verificación afectadas después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller, has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas cuando cambien las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Utiliza **Plan** para acordar un enfoque antes de empezar, **Interactive** para participar en cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para los cambios rutinarios y uno más potente para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones

Ya has completado el flujo de trabajo principal. Puedes explorar estas funciones adicionales de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Aplícala al código de producción, a tus proyectos personales o a esa pequeña aplicación que llevas años pensando en crear. Comparte tus aprendizajes con el equipo y aprende de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional de [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
