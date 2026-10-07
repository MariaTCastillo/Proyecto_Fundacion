Autor del encargo: Maria Teresa Castillo Calderon.


Objetivo: proponer tres pruebas para la última plaza de una sesión ficticia.


Material: la regla del CONTEXTO y el apartado de agenda del guion funcional.


Resultado: tabla con preparación, acción, resultado esperado y qué observar.


Límite: no implementar todavía; no modificar otras reglas del proyecto.


Comprobación: debe existir un caso de dos solicitudes simultáneas.

# Pruebas propuestas: última plaza

## Reglas aplicadas

- En `docs/ia/CONTEXTO.md`, una sesión con una plaza admite como máximo una reserva activa. Si falla un cambio de cita, se conserva la cita anterior.
- El apartado 8 del guion funcional concreta la concurrencia: dos conexiones distintas que compiten por la última plaza deben producir un éxito y un rechazo, nunca dos éxitos. La decisión y la comprobación final corresponden al servidor.
- El apartado 9 concreta el cambio de cita: si el destino está lleno, cerrado o ya ha comenzado, el cambio se rechaza y se conservan el ID de reserva y la cita de origen.
- El botón de la web puede desactivarse para evitar otro clic, pero eso por sí solo no garantiza la plaza ni evita duplicados. El servidor debe volver a comprobar la disponibilidad al procesar y guardar la solicitud.

## Contradicciones y datos pendientes

- La regla de una reserva activa por plaza de `docs/ia/CONTEXTO.md` coincide con el guion funcional. El apartado 8 concreta que dos conexiones distintas que compiten por la última plaza deben producir un éxito y un rechazo, nunca dos éxitos. No encuentro contradicción entre esas reglas.
- El apartado 9 concreta que se conserva el mismo ID y la cita de origen si el destino está lleno, cerrado o ya ha comenzado.
- El guion deja decisiones de cliente pendientes en su apartado 21, pero instruye usar sus reglas didácticas para el MVP. Para estas pruebas no hace falta resolverlas.
- El proyecto aún no contiene aplicación. Estos casos se pueden recorrer como simulación de escritorio, pero eso no demuestra bloqueo de filas, transacciones ni concurrencia real. El propio guion advierte que la maqueta local y un servidor que procesa una petición cada vez no acreditan R04.

## Casos propuestos (simulación)

| Caso | Preparación | Acción | Resultado esperado | Qué observar |
|---|---|---|---|---|
| 1. Una solicitud ocupa la última plaza | Preparar una sesión abierta de SV01 (atención individual, capacidad de una plaza), futura y sin reservas que ocupen plaza. Preparar una persona ficticia y una clave de idempotencia nueva. | Enviar una solicitud de reserva y confirmar el resumen. | <u>Se crea una reserva</u>; la sesión queda con una plaza ocupada. La respuesta de éxito incluye código de reserva, centro, servicio y hora, y entrega la clave privada para copiar una vez. | Contar reservas activas y comprobar el estado. No guardar ni incluir la clave privada en evidencias o registros. |
| 2. Dos solicitudes compiten simultáneamente por la última plaza (R04) | Preparar otra sesión abierta SV01, futura y vacía. Preparar dos solicitudes de personas ficticias, cada una con su propia clave de idempotencia. Para la prueba real harán falta dos conexiones distintas al servidor integrado y una barrera que las libere a la vez. | Enviar ambas solicitudes concurrentemente, de modo que las dos hayan visto inicialmente la plaza disponible. | Exactamente una solicitud tiene éxito y la otra se rechaza; nunca hay dos éxitos ni más de una reserva ocupando la plaza. Una maqueta de interfaz o ejecución secuencial no cuenta como verificación de concurrencia real. | Resultado individual de cada petición, estado final y número de reservas que ocupan plaza en la base de datos. Verificar que la comprobación se aplica en el servidor/transacción. |
| 3. Cambio rechazado al destino lleno (R06) | Preparar una reserva activa en una sesión origen futura y una sesión destino SV01 ya llena. Anotar el ID de la reserva y el estado de ambas sesiones. No asociar un documento firmado, porque el guion prohíbe reprogramar reservas con documento firmado. | Solicitar el cambio protegido de la reserva a la sesión destino llena. | El cambio se rechaza y revierte por completo: se conserva la cita original y el mismo ID de reserva. No se crea una reserva parcial en destino. | Comparar ID y sesión de la reserva antes y después, plazas ocupadas de ambas sesiones y resultado del intento. |

## Alcance de la simulación

En papel se pueden recorrer las preparaciones y comparar los estados esperados. El caso 2 solo quedará comprobado cuando exista un servidor integrado y se ejecuten dos solicitudes concurrentes reales; anotar el resultado esperado no equivale a haberlo ejecutado. El proyecto todavía no contiene aplicación, así que esta propuesta no se ha ejecutado y no demuestra el comportamiento real de la interfaz, del servidor ni de la concurrencia.
