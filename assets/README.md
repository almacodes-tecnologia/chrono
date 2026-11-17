# Chrono — paquete de contenido de prueba v1 (JSON en español)

Este paquete contiene datos iniciales provisionales para desarrollar y probar Chrono: Arcade Time Trial.
Los nombres comerciales, cifras de rendimiento, precios y tiempos objetivo son de prueba y podrán ajustarse.

## Estructura

- `index.json`: catálogo de identificadores disponibles.
- `vehiculos/<id>.json`: un archivo por vehículo.
- `circuitos/<id>.json`: un archivo por circuito.
- `licencias/<id>.json`: un archivo por licencia.
- `campeonatos/<id>.json`: un archivo por campeonato.

Los nombres de las carpetas, las propiedades JSON y los valores de categorías, modalidades y superficies están en español. Los identificadores de archivo y las referencias entre entidades se mantienen estables en inglés.

## Identificadores y claves

- Los `id` y las referencias (`*_id`) se mantienen en inglés y en `snake_case`, para que sean estables y no haya que renombrar archivos ni referencias.
- Las claves JSON usan español sin tildes: `descripcion`, `rendimiento`, `requisitos_desbloqueo`, etc.
- Las categorías, modalidades, superficies y otros valores de catálogo se expresan en español.
- `version_esquema` cambia cuando cambia la estructura del contrato JSON.
- `version_contenido` aumenta cuando se publica contenido nuevo compatible con el mismo esquema.

## Convenciones de vehículos

- `rendimiento.potencia_kw`: potencia de referencia en kW.
- `rendimiento.peso_kg`: masa del vehículo.
- `rendimiento.aerodinamica`: índice de eficiencia aerodinámica de 0 a 100; un valor mayor representa mejor eficiencia.
- `rendimiento.resistencia_superficies`: multiplicadores relativos por superficie. `1.00` es la referencia; un valor menor ofrece menos resistencia y uno mayor penaliza más.
- `rendimiento.penalizacion_fuera_pista`: penalización adicional entre 0 y 1 al circular fuera de la superficie transitable.
- `manejo.respuesta_direccion`: respuesta de dirección de 0 a 100.
- `manejo.multiplicador_giro`: multiplicador de giro relativo.
- `visual.renderizador`: identificador del renderizador que deberá existir en el registro de renderizadores.
- `vehiculo_inicial`: indica si el vehículo se entrega al crear una partida nueva.

La velocidad máxima no se almacena: se calculará en el juego a partir de las características del vehículo y la configuración global.

## Convenciones de circuitos

- `modalidad`: `vueltas` para circuitos cerrados o `punto_a_punto` para recorridos de salida a meta.
- `ruta.cerrado`: indica si el último punto conecta con el primero.
- `ruta.puntos_ruta`: puntos centrales del recorrido, expresados en metros.
- `x` aumenta hacia la derecha e `y` hacia abajo.
- `superficie_id` identifica la superficie que se aplica desde ese punto hasta el siguiente.
- En circuitos cerrados, el último punto conecta con el primero.
- `indices_puntos_control` contiene los índices de los puntos de control, en orden de paso.
- `indice_salida` e `indice_meta` identifican salida y meta.
- `vueltas_por_defecto` solo aparece en circuitos cerrados. En campeonatos o pruebas se puede indicar un número específico de vueltas.

Superficies utilizadas: `asfalto`, `asfalto_rugoso`, `grava`, `tierra`, `arena`, `hierba`, `barro`, `nieve`, `hielo`, `roca`.

## Convenciones de licencias

Cada licencia contiene una lista `pruebas`. Los tiempos de medalla están expresados en milisegundos. Para pruebas punto a punto no se especifica `vueltas`; para pruebas de circuito sí.

La licencia D no exige otra licencia, para que el jugador pueda realizar la primera prueba desde una partida nueva. Las siguientes licencias requieren la anterior.

## Convenciones de campeonatos

- `rondas` conserva el orden de las rondas.
- `reglas.puntuacion` usa `suma_tiempos_reales`: se suman los tiempos reales de las rondas del intento actual.
- `mismo_vehiculo_obligatorio` obliga a usar el mismo vehículo durante todo el campeonato.
- Los tiempos históricos de récords no deben utilizarse para calcular el resultado de un nuevo intento.

## Validaciones recomendadas al cargar

1. El `id` del JSON debe coincidir con el nombre del archivo sin `.json`.
2. Cada ID de `index.json` debe tener su archivo correspondiente.
3. No debe haber IDs duplicados.
4. Cada referencia a una superficie, circuito, vehículo, licencia o campeonato debe existir.
5. Los índices de puntos de control y puntos de ruta deben estar dentro de los límites de la ruta.
6. Los valores numéricos deben ser finitos y estar dentro de los rangos definidos por los modelos Dart.

## Importante

Este es el primer contrato de datos para el desarrollo. Antes de implementar los modelos Dart definitivos conviene revisar y congelar los nombres de campos y enumeraciones. El contenido podrá cambiar después sin cambiar el esquema, siempre que respete esos contratos.
