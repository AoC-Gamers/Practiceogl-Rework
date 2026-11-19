# Trabajo en Practiceogl-Rework

Consulta [el índice de documentación](docs/README.md) y
[la guía de compilación](docs/build/README.md) antes de editar.

- Conserva los IDs de Confogl, el orden de carga y descarga, los nombres de
  plugins y las rutas de Stripper, fuentes y recursos de SourceMod.
- Compila con el Makefile raíz. El mapa de producto en
  `scripts/package/package-map.json` declara los entrypoints. Genera y valida el ZIP con
  `make release` y `make validate-artifact`. Las salidas quedan en
  `.deps/`, `.build/` y `dist/`.
- Conserva autoría y licencias. Los cambios remotos y el despliegue requieren autorización.

## Actualizar la base de ZoneMod

1. Toma la versión y el SHA de la [referencia técnica](docs/mode/zonemod-baseline.md).
   Compara esa revisión con la candidata en `cfg/cfgogl/zm1v1`. Sigue los
   `exec` hasta los CFG compartidos de `zonemod` y revisa los cambios en
   Plugin Manager, la validación de clientes y Stripper.
2. Compara la nueva base con `pr1v1` y los CFG compartidos de
   `practiceogl`. Aplica los cambios acordados y conserva las herramientas
   y reglas de práctica. Revisa los límites por mapa, el orden de carga y la
   descarga de plugins. Ajusta el mapa de producto si cambian los recursos
   instalables o sus referencias externas.
3. Ejecuta `make validate-modes`, `make release` y `make validate-artifact`.
   Si falta una compilación válida o cambian fuentes o includes, sigue la guía
   de compilación y ejecuta `make build-smx` antes de empaquetar.
   Actualiza la revisión, el alcance sincronizado y las diferencias
   pendientes en la referencia técnica. Anota los cambios de comportamiento
   en el changelog.
