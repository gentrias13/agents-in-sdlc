---
title: "Lección 5: Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de sus informes y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya hemos comprobado manualmente que el código funciona y utilizado archivos de instrucciones para asegurarnos de que cumple los estándares. Pero ¿qué ocurre con las pruebas, el lint y los demás elementos de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son una buena opción. Ayudan a Copilot a entender cómo ejecutar correctamente operaciones como estas.

En esta lección:

- Explorarás la habilidad `quality-checks` existente.
- Personalizarás el formato de los resultados.
- Volverás a cargar y ejecutar la habilidad.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En su forma más sencilla, una habilidad es una carpeta que contiene un archivo Markdown llamado `SKILL.md`. El archivo incluye metadatos YAML con un nombre y una descripción que definen la habilidad, una descripción general de lo que hace e indicaciones sobre cuándo debe utilizarse. La carpeta también puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden guardarse en `.github/skills` para que sean recursos del repositorio que comparta el equipo o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe ejecutar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes llamar a las habilidades directamente desde Copilot CLI o solicitarlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este mensaje:

   ```plaintext
   Ejecuta la habilidad quality-checks para realizar las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personalizar el informe

El informe debería indicar qué se ejecutó, si se completó correctamente y qué resultados devolvieron las herramientas. Actualiza la habilidad para que incluya esta información.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Al terminar, indica cada comando que se haya ejecutado y si ha finalizado correctamente, ha fallado o se ha bloqueado. Incluye recuentos de pruebas, duraciones, errores, advertencias y otras métricas solo si la herramienta las proporciona. Indica qué acción debe realizarse a continuación para resolver cada fallo o bloqueo, y nunca presentes como correctas las comprobaciones omitidas o incompletas.
   ```

3. El archivo se guardará automáticamente.

## Ejecutar la habilidad actualizada

Ahora que has modificado la habilidad, comprueba cómo funciona. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para realizar las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y próximos pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- Has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- Has personalizado el formato de sus resultados.
- Has vuelto a cargar y ejecutar la habilidad.

Ese cambio formará parte de la PR de filtrado. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven como referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
