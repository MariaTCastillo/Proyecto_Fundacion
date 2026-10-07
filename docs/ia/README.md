# Simulación de las pruebas de la última plaza
Ruta usada: IDE.
Este proyecto todavía no contiene una aplicación. Los casos de [PRUEBAS_ULTIMA_PLAZA.md](docs/PRUEBAS_ULTIMA_PLAZA.md) son una **simulación propuesta**; no se han ejecutado. La simulación no demuestra concurrencia real.

## Pasos para repetir la simulación

1. Abre `docs/PRUEBAS_ULTIMA_PLAZA.md` y lee las reglas de los apartados 8 y 9 del guion funcional.
2. Prepara una hoja de seguimiento con sesión (SV01), capacidad (una plaza), estado de la reserva (ocupada/disponible), ID (U001/U003), acción y resultado; usa solo personas y datos ficticios.
3. Para el caso 1, deja vacía una sesión SV01 de una plaza, simula una solicitud y anota que queda exactamente una plaza ocupada.
4. Para el caso 2, prepara dos solicitudes con claves de idempotencia distintas y representa el intento simultáneo; el resultado esperado es un éxito y un rechazo. Márcalo como simulación: solo dos conexiones reales al servidor podrán verificar R04.
5. Para el caso 3, anota el ID y la sesión de origen; intenta cambiar a otra sesión SV01 llena y comprueba en la tabla que el ID y la cita original se conservan.
6. Registra resultados simulados por separado de resultados obtenidos en una aplicación; no afirmes que se ejecutó una prueba real sin evidencia de esa ejecución.
