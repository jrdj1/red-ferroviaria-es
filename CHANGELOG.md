# Cambios

## v2026.11.3 — 2026-11-24

Estaciones sin nombre en OSM en el dataset nacional. Misma instantánea de OSM (1/11/2026).

- `national/`: las estaciones que no tienen ningún nombre en OSM lo toman de su parada de
  Cercanías de Renfe (el resto de nombres no cambia). Aparecen Crevillente (con el mismo id que
  en `regions/levante/`), Aguda, Barros y Hernani-Erdia: 1.610 estaciones. La red no cambia.
  `sources.json` registra el GTFS de Cercanías como fuente.
- `regions/levante/`: sin cambios.

## v2026.11.2 — 2026-11-23

Conectividad corregida en el Levante. Misma instantánea de OSM (1/11/2026).

- `regions/levante/`: Xàtiva vuelve a enlazar con la línea hacia València y con el ramal de
  Alcoi (tenía una sola estación vecina). En la red del 14/12/2020, Villena AV llega a Alicante y
  Requena-Utiel a València Joaquín Sorolla: solo quedan sin estaciones vecinas Elche AV y Callosa
  de Segura-Cox, abiertas en 2021. Aparecen algunos tramos de 2020 que antes se perdían junto a
  una línea paralela de otro tipo; los que no tienen evento figuran como huecos en `report.json`.
  Se conserva el 99,8 % de los ids de tramos (los cambiados están en `id_equivalences.json`).
- `national/`: mismas estaciones; ids de tramos casi sin cambios (98,5 %).

## v2026.11.1 — 2026-11-23

Corrección de la historia del Levante. Misma instantánea de OSM (1/11/2026).

- `regions/levante/`: la red a 14/12/2020 vuelve a ser la real. El corredor Valencia–Alicante
  de ancho estándar (2027) ya no oculta la LAV Madrid–Alicante ni el acceso a Joaquín Sorolla; la
  variante Xàtiva–La Encina abre solo su trazado nuevo; la antigua línea 300 por Moixent está en
  servicio en 2020 y cierra el 9/9/2023 (nuevo cambio con fuente). Solo quedan sin tramos en
  servicio al inicio Elche AV y Callosa de Segura-Cox, abiertas en 2021. 116 estaciones (vuelve
  Balsicas-Mar Menor); 396 km de alta velocidad en servicio hoy (la línea 300 está en obras).
- Estaciones mejor ancladas: prefieren la vía en servicio frente a una obra cercana y la
  convencional frente a una LAV paralela, y nunca se anclan a una aguja.
- `national/`: 1.606 estaciones. Vuelven cinco que se perdían ancladas a agujas (Os Peares,
  Barra de Miño, Irauregi-Alonsotegi, Monistrol Enllaç y Can Feu-Gràcia) y dejan de publicarse
  nueve sin nombre en OSM, que salían con su id como nombre.

## v2026.11.0 — 2026-11-23

Primera versión.

- `regions/levante/`: Alicante, Murcia y Valencia. Instantánea de OSM del 1/11/2026 e inicio el
  14/12/2020. 916 km convencional, 421 km de alta velocidad y 20 km de ancho métrico (incluidos
  los tramos de frontera); 115 estaciones en una única red conectada; 8 cambios con fecha.
- `national/`: red peninsular simplificada, unos 16.000 km y 1.610 estaciones.
- `schemas/`: esquemas (versión de esquema 1).
