# Changelog

## [2.4.2] - 2026-11-20

### Corregido

- Retirados `l4d2_steady_boost` y sus cvars tras su retirada de la base competitiva.

## [2.4.1] - 2026-11-19

### Cambiado

- PracticeOgl usa `fix_pill_pass` con pase por recarga, como ZoneMod.

- Los datos y comandos por mapa de PracticeOgl se sincronizaron con ZoneMod
  1v1 v2.9.2b, incluida la configuración de Dead Center 2025.

- El ZIP instalable conserva las rutas de Confogl y reúne los plugins
  recompilados, su código fuente y los recursos de PracticeOgl. Para el
  Stripper de ZoneMod se usa la instalación de L4D2-Competitive-Rework.

### Corregido

- Al salir de PracticeOgl, Confogl usa Plugin Manager para descargar los
  plugins del modo y recargar los de carga automática.

### Desarrollo

- Make y GitHub Actions compilan y validan el ZIP a partir de las fuentes.
