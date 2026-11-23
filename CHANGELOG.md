# Cambios

## Pendiente

Mejoras propuestas, aparcadas mientras la prioridad es el MVP del juego. Están en orden de
prioridad; la 1 y la 2 conviene hacerlas juntas.

1. **Diccionario de datos completo.** Añadir `description` a cada campo de los esquemas (ahora
   solo hay 2), cambiar los `$id` `ancho-iberico/...` por URLs reales de este repositorio y
   generar `docs/diccionario.md` a partir de los esquemas. Documentar `municipalities`,
   `basemap`, `report`, `sources` e `id_equivalences`, y aclarar el `start_date` de `national/`.
   Los esquemas se generan en el pipeline del juego.
2. **Validación automática en la CI.** Un GitHub Action que valide cada fichero con su esquema y
   compruebe la coherencia interna: que los `from`/`to` existen, que `length_m` coincide con `pk`,
   que los ids de `history.json` existen y que las huellas de `sources.json` son correctas.
3. **Metadatos para Zenodo.** Añadir `.zenodo.json`, completar `CITATION.cff` (autor con
   nombre, ORCID, resumen, `preferred-citation`) y activar la integración GitHub–Zenodo para
   tener un DOI por versión.
4. **Releases automáticas.** Al subir una etiqueta `v*`, la CI valida, crea el zip, publica
   `SHA256SUMS` y saca las notas de este fichero. Así sería más fácil actualizar la huella en
   `game/data/datasets.json` del juego.
5. **Formatos estándar.** Exportaciones en GeoJSON WGS84 y/o GeoPackage, y CSV de estaciones y de
   cambios, publicadas solo en la release para no inflar el repositorio.
6. **Material académico.** Un `datapackage.json` (Frictionless), un cuaderno de ejemplo (cargar el
   grafo con networkx o geopandas y aplicar la historia), un documento de metodología y calidad
   que explique `report.json`, y una guía para contribuir y reportar errores.

## v2026.11.0 — 2026-11-23

Primera versión.

- `regions/levante/`: Alicante, Murcia y Valencia. Instantánea de OSM del 1/11/2026 e inicio el
  14/12/2020. 916 km convencional, 421 km de alta velocidad y 20 km de ancho métrico (incluidos
  los tramos de frontera); 115 estaciones en una única red conectada; 8 cambios con fecha.
- `national/`: red peninsular simplificada, unos 16.000 km y 1.610 estaciones.
- `schemas/`: esquemas (versión de esquema 1).
