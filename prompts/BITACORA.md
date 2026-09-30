# Bitacora de tecnicas avanzadas

Laboratorio 07: Tecnicas Avanzadas de Prompting.
Herramienta de IA usada: (escribe aqui cual usaste)

## Ejercicio 2: Zero-shot, one-shot y few-shot
| Tipo | Aciertos (de 5) | Formato de la respuesta | Todas con el mismo formato (Si/No) |
|------|-----------------|-------------------------|------------------------------------|
| Zero-shot|5|1.-Me encanto, llego rapido Positivo |No |
| One-shot |5|1.-Positivo ("Me encanto, llego rapido") |No |
| Few-shot |5|1.-"Me encanto, llego rapido" -> Positivo |No |
## Ejercicio 3: Chain of Thought
| Pedido | Respuesta de la IA | Muestra los pasos (Si/No) | Correcta (Si/No) |
|--------|--------------------|---------------------------|------------------|
| Directo | 325.68|No |No|
| Paso a paso |318.60 |Si |Si |
## Ejercicio 4: Role prompting
| Version | Vocabulario (sencillo/tecnico) | Usa ejemplos o codigo | A quien le sirve mas |
|---------|-------------------------------|-----------------------|----------------------|
| A. Sin rol |Sencillo |Ejemplo y codigo|Principiantes |
| B. Rol docente |Sencillo |Ejemplo y codigo|Estudiantes  |
| C. Rol senior |Tecnico |Ejemplo y codigo |Profesionales |
## Ejercicio 5: Descomposicion
```text
Paso 1: La IA entregó una lista con los 5 requisitos principales del sistema de inventario (CRUD, control de stock, registro de movimientos, reportes y autenticación).

Paso 2: La IA entregó el diseño del modelo de clases en Java (Producto, Usuario, MovimientoInventario, DetalleMovimiento y Enums) con sus atributos y tipos de datos.

Paso 3: La IA entregó el código Java completo de la clase Producto con atributos, constructores y métodos get/set.

Paso 4: La IA entregó 3 mejoras concretas para la clase Producto (encapsulamiento de lógica de stock, validaciones en setters y uso de Enum para la categoría).
```
## Ejercicio 6: Prompt estructurado y autocritica
```text
<rol>Actua como analista de pruebas de
software.</rol>
<contexto>Login web con correo y
contrasena. La cuenta se bloquea
despues de 3 intentos fallidos.</contexto>
<tarea>Piensa paso a paso que puede fallar
y escribe 6 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID,
escenario, datos de entrada,
resultado esperado.</formato>

Revisa tu tabla: faltan casos limite como campos
vacios, correo sin @
o contrasena con espacios? Agrega los que falten
e indica cuales agregaste.
```