# Chrono — paquete de contenido de prueba v1

Este paquete contiene datos iniciales provisionales para desarrollar y probar Chrono: Arcade Time Trial.
Los nombres, cifras de rendimiento, precios y tiempos objetivo son de prueba y podrán ajustarse.

## Estructura

- `index.json`: catálogo de IDs disponibles.
- `vehicles/<id>.json`: un archivo por vehículo.
- `tracks/<id>.json`: un archivo por circuito.
- `licenses/<id>.json`: un archivo por licencia.
- `championships/<id>.json`: un archivo por campeonato.

## Versionado

- `schema_version`: versión de la estructura JSON. Debe cambiar si cambia el contrato de datos.
- `content_version`: versión del contenido. Debe aumentar cuando se publiquen cambios compatibles en los datos.

## Convenciones de vehículos

- `performance.power_kw`: potencia de referencia en kW.
- `performance.weight_kg`: masa del vehículo.
- `performance.aerodynamics`: índice de eficiencia aerodinámica de 0 a 100; un valor mayor representa mejor eficiencia.
- `performance.surface_resistance`: multiplicadores relativos por superficie. `1.00` es la referencia; un valor menor ofrece menos resistencia y uno mayor penaliza más.
- `performance.offroad_penalty`: penalización adicional entre 0 y 1 al circular fuera de la superficie transitable del trazado.
- `handling.steering_response`: respuesta de dirección de 0 a 100.
- `handling.turn_rate_multiplier`: multiplicador de giro relativo.
- `visual.renderer`: identificador del renderizador que deberá existir en el registro de renderizadores.
- `starter_vehicle`: indica el vehículo que se entrega al crear una partida nueva.

La velocidad máxima no se almacena: se calculará en el juego a partir de las características del vehículo y la configuración global.

## Convenciones de circuitos

- `mode`: `laps` para circuitos cerrados o `point_to_point` para recorridos de salida a meta.
- `route.closed`: indica si el último waypoint conecta con el primero.
- `route.waypoints`: puntos centrales del recorrido, expresados en metros.
- `x` aumenta hacia la derecha e `y` hacia abajo.
- `surface_id` identifica la superficie que se aplica desde ese waypoint hasta el siguiente.
- En circuitos cerrados, el último waypoint conecta con el primero.
- `checkpoint_indices` contiene los índices de los waypoints que representan checkpoints, en orden de paso.
- `start_waypoint_index` y `finish_waypoint_index` identifican salida y meta.
- `default_laps` solo aparece en circuitos cerrados. En campeonatos o pruebas se puede indicar un número de vueltas específico.

Superficies utilizadas: `asphalt`, `rough_asphalt`, `gravel`, `dirt`, `sand`, `grass`, `mud`, `snow`, `ice`, `rock`.

## Convenciones de licencias

Cada licencia contiene una lista `tests`. Los tiempos de medalla están expresados en milisegundos. Para las pruebas punto a punto no se especifica `laps`; para pruebas de circuito sí.

La licencia D no exige otra licencia, para que el jugador pueda realizar la primera prueba desde una partida nueva. Las siguientes licencias requieren la anterior.

## Convenciones de campeonatos

- `rounds` conserva el orden de las rondas.
- `rules.scoring` usa `cumulative_elapsed_time`: se suman los tiempos reales de las rondas del intento actual.
- `same_vehicle_required` obliga a usar el mismo vehículo durante todo el campeonato.
- Los tiempos históricos de récords no deben utilizarse para calcular el resultado de un nuevo intento.

## Validaciones recomendadas al cargar

1. El `id` del JSON debe coincidir con el nombre del archivo sin `.json`.
2. Cada ID de `index.json` debe tener su archivo correspondiente.
3. No debe haber IDs duplicados.
4. Cada `surface_id`, `track_id`, `vehicle_id`, `license_id` y `championship_id` referenciado debe existir en el catálogo correspondiente.
5. Los índices de checkpoints y waypoints deben estar dentro de los límites de la ruta.
6. Los valores numéricos deben ser finitos y estar dentro de los rangos definidos por los modelos Dart.

## Importante

Este es un primer contrato de datos para el desarrollo. Antes de implementar los modelos Dart definitivos conviene revisar y congelar los nombres de campos y enumeraciones. El contenido puede cambiar después sin cambiar el esquema, siempre que se respeten esos contratos.
