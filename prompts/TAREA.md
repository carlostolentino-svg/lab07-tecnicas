# Tarea: Mi prompt avanzado

Laboratorio 07: Técnicas Avanzadas de Prompting.
Alumno: Carlos Fernando Tolentino Lopez
Herramienta de IA usada: (escribe aquí cuál usaste: ChatGPT, Gemini, Claude o Copilot)

## Tarea elegida

Generar **casos de prueba para un formulario de registro de usuarios** con estas reglas:

- Correo electrónico obligatorio y con formato válido.
- Contraseña de mínimo 8 caracteres, con al menos una mayúscula y un número.
- Edad mínima de 18 años.

Elegí esta tarea porque probar formularios es algo que hago en mis proyectos de desarrollo de software y un buen conjunto de casos de prueba evita errores antes de entregar.

## Version 1: prompt basico

**Técnica agregada:** ninguna (zero-shot).

```text
Dame casos de prueba para un registro de usuarios.
```

**Qué pasó:** la respuesta fue genérica y de formato libre. No conocía mis reglas (8 caracteres, mayúscula, número, 18 años), mezcló casos de todo tipo y no tenía una estructura fija que pueda copiar a Excel.

**Qué faltaba:** un rol, el contexto con mis reglas y un formato definido.

## Version 2

**Técnicas agregadas:** role prompting + prompt estructurado (etiquetas).

```text
<rol>Actua como analista de pruebas de software (QA) que prepara
casos de prueba para un equipo de desarrollo junior.</rol>

<contexto>Formulario web de registro con tres campos: correo,
contrasena y edad. Reglas: el correo debe tener formato valido,
la contrasena debe tener minimo 8 caracteres con al menos una
mayuscula y un numero, y la edad minima es 18 anos.</contexto>

<tarea>Escribe 8 casos de prueba.</tarea>

<formato>Tabla con las columnas: ID, escenario, datos de entrada,
resultado esperado.</formato>

Responde en espanol.
```

**Por qué:** el rol hace que la respuesta use el enfoque de un analista QA, y las etiquetas separan rol, contexto, tarea y formato para que no se mezclen.

**Qué mejoró:** ahora la respuesta es una tabla con las 4 columnas y los casos respetan mis reglas. **Qué faltaba:** los casos de borde (edad 17 y 18, contraseña de exactamente 8 caracteres, campos vacíos) y que la IA explique qué puede fallar antes de escribir los casos.

## Version 3: prompt final

**Técnicas agregadas:** few-shot + chain of thought + autocrítica (sumadas al rol y al prompt estructurado de la versión 2).

```text
<rol>Actua como analista de pruebas de software (QA) que prepara
casos de prueba para un equipo de desarrollo junior.</rol>

<contexto>Formulario web de registro con tres campos: correo,
contrasena y edad. Reglas: el correo debe tener formato valido,
la contrasena debe tener minimo 8 caracteres con al menos una
mayuscula y un numero, y la edad minima es 18 anos.</contexto>

<ejemplo>
| ID | Escenario | Datos de entrada | Resultado esperado |
|----|-----------|------------------|--------------------|
| CP-01 | Registro correcto | ana@mail.com / Clave2024 / 25 | Cuenta creada |
| CP-02 | Correo sin arroba | anamail.com / Clave2024 / 25 | Error: correo invalido |
</ejemplo>

<tarea>
Paso 1: piensa paso a paso que puede fallar en cada campo,
incluyendo valores limite (por ejemplo edad 17 y 18, contrasena
de 7 y 8 caracteres) y campos vacios.
Paso 2: escribe 10 casos de prueba siguiendo el formato del ejemplo,
con IDs CP-01, CP-02, etc.
Paso 3: revisa tu tabla. Busca casos repetidos o que falten,
agrega los que falten e indica cuales agregaste.
</tarea>

<formato>Primero una lista corta con lo que puede fallar, luego la
tabla con las columnas: ID, escenario, datos de entrada, resultado
esperado, y al final la lista de casos agregados en la revision.
Responde en espanol.</formato>
```

**Por qué:** el ejemplo (few-shot) fija el formato exacto de cada fila, el paso 1 (chain of thought) obliga a pensar en los casos límite antes de escribirlos, y el paso 3 (autocrítica) hace que la IA complete lo que olvidó.

**Qué mejoró:** la respuesta incluye casos límite (17 y 18 años, 7 y 8 caracteres), campos vacíos, sigue el formato del ejemplo en todas las filas y declara qué casos agregó en la revisión.

## Tecnicas usadas en el prompt final

| Técnica | Parte del prompt final donde se aplica |
|---------|----------------------------------------|
| Role prompting | `<rol>`: analista QA que prepara casos para un equipo junior (rol específico, no "experto") |
| Prompt estructurado | Etiquetas `<rol>`, `<contexto>`, `<ejemplo>`, `<tarea>` y `<formato>` |
| Few-shot | `<ejemplo>`: dos filas de muestra con el formato de la tabla |
| Chain of thought | `<tarea>`, Paso 1: "piensa paso a paso que puede fallar" |
| Autocrítica | `<tarea>`, Paso 3: "revisa tu tabla... agrega los que falten e indica cuales agregaste" |

## Evaluacion del resultado

<!-- Revisa la respuesta REAL de tu IA y cambia Si/No según lo que veas -->

| Criterio | Cumple (Si / No) |
|----------|------------------|
| ¿Tiene las 4 columnas pedidas (ID, escenario, datos de entrada, resultado esperado)? | Si |
| ¿Incluye casos límite de edad (17 y 18) y de contraseña (7 y 8 caracteres)? | Si |
| ¿Incluye casos con campos vacíos? | Si |
| ¿Indica qué casos agregó en la autocrítica? | Si |
| ¿Todas las filas siguen el mismo formato del ejemplo? | Si |
| ¿Hay algún caso repetido o que no tenga sentido? | No |

## Por que elegi estas tecnicas

Elegí role prompting y prompt estructurado porque los casos de prueba necesitan el punto de vista de un analista QA y porque el prompt tiene varias partes (reglas, ejemplo, pasos y formato) que conviene mantener separadas. Usé few-shot porque quiero que todas las filas tengan exactamente las mismas columnas para poder copiarlas a una hoja de cálculo sin corregir nada. Usé chain of thought porque encontrar casos límite requiere pensar qué puede fallar antes de escribir, y la autocrítica porque en la parte guiada vi que la primera tabla suele dejar casos sin cubrir. No usé descomposición porque la tarea es pequeña y cabe en un solo pedido; dividirla en varios mensajes habría tomado más tiempo sin mejorar el resultado. Aun así, yo soy el último revisor: verifico que los casos coincidan con mis reglas antes de usarlos.
