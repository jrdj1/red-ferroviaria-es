# red-ferroviaria-es

Datasets abiertos de la **red ferroviaria de España**: trazado, estaciones, atributos de la vía y
evolución de la red desde diciembre de 2020, construidos a partir de OpenStreetMap y fuentes
oficiales. Pensados para simulación, análisis y docencia.

Los genera el pipeline del proyecto [Ancho Ibérico](https://github.com/jrdj1/Ancho-Iberico)
(carpeta `pipeline/`), pero son independientes de él: cualquiera puede usarlos sin el juego.

> Licencia: **ODbL 1.0** (base de datos derivada de OpenStreetMap). Ver [LICENSE.md](LICENSE.md).

## Contenido

| Carpeta | Qué es |
|---|---|
| `regions/levante/` | Alicante, Murcia y Valencia, con mapa base, población y historia desde 2020 |
| `national/` | Red peninsular simplificada (red y estaciones), sin historia |
| `schemas/` | JSON Schema (draft 2020-12) de cada fichero |

Cada versión se publica como *release* (`vAAAA.MM.N`) con un paquete descargable. La fecha de
la instantánea de OpenStreetMap está en `meta.json` (`osm_date`).

## Ficheros de una región

| Fichero | Contenido |
|---|---|
| `meta.json` | Región, proyección (`crs`), origen local (`origin`), fechas y versión del dataset |
| `network.json` | Grafo: `nodes` (estaciones, bifurcaciones, extremos, frontera) y `edges` (tramos) |
| `stations.json` | Estaciones con nombre, nombres alternativos, municipio y tipos de red |
| `municipalities.geojson` | Municipios con código INE, nombre y población (padrón a 1/1/2021) |
| `basemap.geojson` | Tierra (contorno = costa) y límites municipales simplificados |
| `history.json` | Cambios de la red con fecha desde el 14/12/2020 (aperturas, cierres, cortes…) |
| `id_equivalences.json` | Identificadores que cambiaron entre versiones (antiguo → nuevo) |
| `report.json` | Estadísticas, atributos desconocidos, validaciones y huecos de la cronología |
| `sources.json` | Fuentes usadas, con versión, licencia, atribución y huella SHA-256 |

## Diccionario de datos

**Coordenadas:** metros en la proyección de `meta.crs` (EPSG:25830 en la península y Baleares,
EPSG:4083 en Canarias), **restando** `meta.origin`. Coordenada real = valor + origen.

**Tramo (`network.json` → `edges[]`)**

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | texto | Identificador estable (`e_` + 12 caracteres) |
| `from`, `to` | id de nodo | Extremos; `polyline` va de `from` a `to` |
| `polyline` | [[x, y], …] | Trazado |
| `pk` | [m, …] | Punto kilométrico acumulado en cada vértice (empieza en 0) |
| `length_m` | número | Longitud en metros |
| `network_kind` | `high_speed` \| `conventional` \| `narrow_gauge` | Tipo de red |
| `gauge` | `iberian` \| `standard` \| `metric` \| `mixed` | Ancho (1668, 1435, 1000 mm, mixto) |
| `electrified` | booleano | Electrificado |
| `max_speed_kmh` | entero | Velocidad máxima |
| `tracks` | entero | Número de vías |
| `usage` | `passenger` \| `freight` \| `mixed` | Uso |
| `commuter` | booleano | Servido por Cercanías |
| `border` | booleano | Sale de la región hasta el primer nodo exterior |
| `unknown` | lista | Atributos que no constan en las fuentes y llevan valor por defecto |

Valores por defecto cuando falta el dato: ancho estándar en alta velocidad, métrico en ancho
métrico e ibérico en el resto; electrificado solo en alta velocidad; velocidad 250 / 100 / 70 km/h
(alta velocidad / convencional / métrico); 1 vía.

**Nodo (`nodes[]`):** `id` (`n_` + 12 caracteres), `kind` (`station`, `junction`, `end`,
`border`), `x`, `y`, `station_id`.

**Estación (`stations.json`):** `id` (el de su nodo), `name` (rotulación del operador o del
administrador de infraestructura), `alt_names`, `municipality_code` (INE, 5 dígitos),
`network_kinds`, `importance` (1 = principal … 4 = apeadero), `x`, `y`.

**Cambio (`history.json` → `changes[]`):** `id`, `type` (`open`, `close`, `temp_close`, `reopen`,
`electrify`, `gauge_change`, `double_track`, `station_open`, `station_close`, `other`), `date`,
`approximate` (si la fuente solo da mes o año: día 15 o 1 de julio), `end_date` (cortes
temporales), `status` (`confirmed` o `planned`), `edges`, `stations`, `set` (atributos nuevos) y
`source` (URL de la fuente). La red de `network.json` describe el estado al inicio
(`meta.start_date`) más todo lo que existe en algún momento; los cambios se aplican en orden.

## Metodología (resumen)

- **Red:** OpenStreetMap vía API Overpass, con la misma consulta en dos fechas (inicio y actual).
  Vías de servicio excluidas; agujas solo cuando son necesarias para la conectividad; vías
  paralelas de un corredor agrupadas en un tramo con su número de vías; nodos duplicados
  fusionados; pequeños cortes de mapeo unidos con conectores (contados en `report.json`).
- **Historia:** la diferencia entre ambas instantáneas se fecha con una cronología curada a mano
  con fuentes (solo eventos importantes). Las diferencias sin evento se listan como huecos.
- **Estaciones:** elementos de OSM fusionados por nombre y distancia y anclados a la vía; nombres
  comprobados con los horarios de Cercanías de Renfe cuando es posible.
- **Población y límites:** padrón del INE (1/1/2021) y recintos municipales del IGN.
- **Reproducibilidad:** con las mismas fuentes, la construcción es idéntica byte a byte.

Limitaciones conocidas: la calidad depende del etiquetado de OSM (atributos desconocidos y
huecos en `report.json`); los metros y tranvías no están incluidos todavía.

## Cómo citar

Ver [CITATION.cff](CITATION.cff). Al usar los datos hay que mantener las atribuciones de
[LICENSE.md](LICENSE.md).
