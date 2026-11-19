# Compilación y distribución

Se requiere GNU Make y Python 3.10 o posterior.

```sh
make deps-smx
make build-smx
make validate-modes
make package-modes
make release
make validate-artifact
```

`deps-smx` descarga el compilador y los includes de SourceMod en `.deps/smx/`.
`SOURCEMOD_VERSION` selecciona la rama del SDK y su valor predeterminado es
`1.12`. Se puede usar un SDK instalado con `make build-smx SPCOMP=/ruta/a/spcomp`.
Las rutas de Make se resuelven desde el repositorio.

`build-smx` compila los entrypoints del
[mapa de producto](../../scripts/package/package-map.json) con los includes
del SDK y los versionados por el modo. Genera los plugins bajo
`.build/smx/addons/sourcemod/plugins/optional/` y el diagnóstico en
`.build/build-smx-compile.log`.

## Paquete local

- `validate-modes` revisa archivos seleccionados, destinos de `exec`, el árbol
  de Stripper y las llamadas de carga de los plugins propios.
- `package-modes` valida la compilación y prepara `.build/package-modes/`.
  Compara las huellas de las fuentes, los includes locales y los SMX con el
  registro creado por `build-smx`.
- `release` prepara el staging y genera `dist/release/practiceogl-rework-local.zip`.
- `validate-artifact` verifica los nombres y bytes del ZIP contra los archivos
  actuales y la compilación. Pasa el mismo `RELEASE_BASENAME` a `release` y
  `validate-artifact` para usar otro nombre.

Las variantes seleccionables son pr1v1. El ZIP lleva la configuración
del modo, plugins recompilados, fuentes, recursos seleccionados y los
[avisos de autoría y licencia](../../NOTICES.md). El registro interno de
compilación queda en `.build/smx/`.

## Dependencias y verificación

En la copia revisada de L4D2-Competitive-Rework (`0390482d`) se encontraron
los 107 plugins externos referenciados por las configuraciones de carga.
`cvar_tracking.cfg` está en esa base. `confogl_rates.cfg` y
`confogl_personalize.cfg` se deben verificar en el perfil del servidor.

La variante pr1v1 usa los archivos compartidos en cfg/cfgogl/practiceogl y el árbol Stripper de ZoneMod de la base. ReadyUp y Left4DHooks se instalan con sus proveedores.

La aceptación en SRCDS requiere comprobar la activación, el inicio de ronda,
el cambio de modo y la restauración de cvars. La validación offline cubre
el contenido del paquete. Los pendientes para una publicación se registran
en `NOTICES.md`.

El validador comprueba el texto de la licencia GNU y el enlace CC BY-SA 3.0
en `NOTICES.md` antes de empaquetar.

## GitHub Actions

El [workflow de compilación](../../.github/workflows/sourcemod-build.yml)
ejecuta los siguientes jobs en orden:

| Job | Operación |
|---|---|
| `deps-smx` | Valida `SOURCEMOD_VERSION` y prepara el SDK con `make deps-smx` cuando falta en la caché. |
| `build-smx` | Restaura el SDK preparado y ejecuta `make build-smx` con `spcomp64`. Entrega los SMX y el registro de compilación al job siguiente. |
| `package` | Descarga la compilación de la misma revisión Git y ejecuta `make validate-modes`, `make release` y `make validate-artifact`. Sube el ZIP con el SHA de la ejecución en el nombre. |

La caché distingue la rama del SDK, el sistema, la arquitectura y el
código de preparación de dependencias. La publicación de releases se
gestiona por separado.
