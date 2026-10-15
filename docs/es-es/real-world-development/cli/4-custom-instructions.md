---
title: "Lección 4: Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta, procura que Copilot tenga a mano esa orientación. Los [archivos de instrucciones][instruction-files] explican no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- Explorarás cómo llegan al agente las instrucciones del repositorio y las limitadas a rutas concretas.
- Actualizarás el archivo de instrucciones para garantizar que se siguen los estándares de programación.
- Observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene varias pautas y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no resulten obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su objetivo, parámetros y valores devueltos con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben seguir siendo válidos cuando cambie el código relacionado.
- Deben respetarse las pautas existentes de formato y lint.

Los archivos de instrucciones permiten comunicar estos requisitos a Copilot para que complete las tareas de acuerdo con las prácticas del equipo.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot pueda entender mejor el estilo y los requisitos de programación. Permiten orientar las sugerencias y los fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas e incluso el tipo de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o solo a determinados tipos de archivo.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener contexto general del proyecto que resulte pertinente en la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que contienen pautas para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones depende del entorno. Consulta la [referencia sobre la compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de utilizar un formato concreto.

## Explorar los archivos de instrucciones del proyecto

El proyecto de inicio ya incluye varios archivos de instrucciones. Explora los existentes antes de modificarlos para comprobar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y lee la breve descripción del proyecto y sus pautas de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un patrón glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir.

## Actualizar los archivos de instrucciones según las pautas del equipo

Los archivos existentes son un buen comienzo, pero falta una pauta. Modifica el archivo principal `copilot-instructions.md` para indicar que el código TypeScript nuevo debe incluir comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - El código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Usar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y reanúdala para que Copilot cargue la nueva orientación sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las pautas del código. ¿Puedes actualizar el código que generaste para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Fíjate en los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado una nueva pauta a la funcionalidad. En concreto:

- Has explorado cómo llegan al agente las instrucciones del repositorio y las limitadas a rutas concretas.
- Has actualizado el archivo de instrucciones para garantizar que se siguen los estándares de programación.
- Has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que el lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: instrucciones personalizadas y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
