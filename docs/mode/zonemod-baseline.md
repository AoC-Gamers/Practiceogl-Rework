# Base de ZoneMod para PracticeOgl

| Referencia | Valor |
| --- | --- |
| Base | [SirPlease/L4D2-Competitive-Rework](https://github.com/SirPlease/L4D2-Competitive-Rework) |
| Perfil de origen | `cfg/cfgogl/zm1v1` · ZoneMod **1v1 v2.9.2b**. Usa los archivos compartidos de `cfg/cfgogl/zonemod` |
| Revisión comparada | [`0390482d48ee87ba16ecb39490dc6f235dbdc1e0`](https://github.com/SirPlease/L4D2-Competitive-Rework/tree/0390482d48ee87ba16ecb39490dc6f235dbdc1e0/cfg/cfgogl/zm1v1) |
| Perfil de destino | `cfg/cfgogl/pr1v1` · Confogl ID `pr1v1` · PracticeOgl **v2.4.1**. Usa los archivos compartidos de `cfg/cfgogl/practiceogl` |

La sincronización abarca el apagado y los mapas. Los perfiles `zm2v2`, `zm3v3` y `zonemod` 4v4 se consultaron como referencia para otros tamaños de equipo.

## Apagado y mapas sincronizados

| Área | Estado en PracticeOgl |
| --- | --- |
| Apagado | El [CFG compartido de apagado](../../cfg/cfgogl/practiceogl/confogl_off.cfg) desactiva ReadyUp, restaura las Cvars y ejecuta `pm_unload_plugins refresh` al final de la secuencia. Plugin Manager lo aporta la base. |
| Datos de mapas | [`mapinfo.txt`](../../cfg/cfgogl/pr1v1/mapinfo.txt) coincide con los datos de `zm1v1`. Incluye 218 mapas y las entradas de Dead Center 2025 con sus límites de píldoras por capítulo. |
| Ajustes por mapa | [`mapcvars.txt`](../../cfg/cfgogl/pr1v1/mapcvars.txt) ya coincidía con los seis límites de comunes de `zm1v1`. [`shared_settings.cfg`](../../cfg/cfgogl/practiceogl/shared_settings.cfg) adopta los mismos comandos por mapa, incluidas las reglas de Tank y excepciones de spit. |

PracticeOgl usa el Stripper de ZoneMod y el `cvar_tracking.cfg` de la base. El [mapa de empaquetado](../../scripts/package/package-map.json) enumera los recursos instalables y los CFG externos.

## Herramientas y reglas de práctica vigentes

| Componente | Comportamiento de PracticeOgl |
| --- | --- |
| [`practiceogl.smx`](../../addons/sourcemod/scripting/practiceogl.sp) | Ofrece `sm_give` y `sm_spawn`. Al comenzar la ronda, asigna a los supervivientes vivos la salud de `sm_pr_life`, con 10.000 por defecto. |
| [`l4d2_zcs.smx`](../../addons/sourcemod/scripting/l4d2_zcs.sp) | Permite elegir la clase de infectado en estado fantasma con MOUSE2. Usa [`l4d2_zcs.txt`](../../addons/sourcemod/gamedata/l4d2_zcs.txt). |
| [`l4d2_thirdperson.smx`](../../addons/sourcemod/scripting/l4d2_thirdperson.sp) | Ofrece cámara en tercera persona para supervivientes con `sm_tp` y sus alias. |
| [`l4d_InfiniteReserveAmmo.smx`](../../addons/sourcemod/scripting/l4d_InfiniteReserveAmmo.sp) | Repone munición de reserva al disparar. |
| [`witch_glow.smx`](../../addons/sourcemod/scripting/witch_glow.sp) | Marca con un contorno a la Witch al aparecer. El contorno se oculta cuando la alertan o se acerca un superviviente al rango configurado. |

[`confogl.cfg`](../../cfg/cfgogl/pr1v1/confogl.cfg) fija en `1` los límites de las seis clases de especiales. `zm1v1` habilita Hunter y Jockey mediante esos límites. Ambos perfiles fijan la reaparición en un segundo. Las probabilidades naturales de Tank y Witch son `0` en ambos. PracticeOgl habilita los indicadores de ambos jefes en [`pr1v1.cfg`](../../cfg/cfgogl/pr1v1/pr1v1.cfg) y permite invocarlos mediante `sm_spawn`.

## Pase de píldoras y adrenalina

PracticeOgl usa `fix_pill_pass` de la base con `fix_pill_pass_reload 1`,
como ZoneMod. El ajuste habilita el pase por recarga. [Carga compartida](../../cfg/cfgogl/practiceogl/shared_plugins.cfg) ·
[Ajustes compartidos](../../cfg/cfgogl/practiceogl/shared_settings.cfg).

## Diferencias pendientes de decisión

Estas configuraciones siguen vigentes en PracticeOgl. Quedan por revisar como parte de su diseño.

| Área | Diferencia frente a `zm1v1` |
| --- | --- |
| Bots infectados | Ambos CFG declaran `director_allow_infected_bots 0`. El [selector de clase](../../addons/sourcemod/scripting/l4d2_zcs.sp) también escribe esa Cvar y usa `zcs_enable_vib 1` y `zcs_respect_limits 0` por defecto. El valor que prevalece requiere comprobarse en SRCDS. |
| Armas y objetos | PracticeOgl omite `l4d_weapon_limits`, `starting_items` y los plugins de aparición de armas CS y variedad de armas. Mantiene conversiones y atributos distintos para varias armas y carga dos plugins de rifles. El límite general de píldoras, adrenalina y arrojables es `-1` y admite las excepciones de `mapinfo.txt`. Omite los rangos de flujo de píldoras y prescinde de la puntuación híbrida. [Carga](../../cfg/cfgogl/practiceogl/shared_plugins.cfg) · [Cvars](../../cfg/cfgogl/practiceogl/shared_cvars.cfg) · [Atributos de armas](../../cfg/cfgogl/practiceogl/shared_settings.cfg) |
| Jefes | PracticeOgl fija 1.500 HP y velocidad `205` para el Tank y permite combinar roca con golpe o salto. Omite el rango de seguridad de aparición del Tank en finales y el ajuste `l4d2_tank_bypass_extra_flow`. Ajusta el espacio personal y el daño de la Witch y carga `l4d_witch_damage_announce`. [Carga](../../cfg/cfgogl/practiceogl/shared_plugins.cfg) · [Cvars](../../cfg/cfgogl/practiceogl/shared_cvars.cfg) · [Ajustes compartidos](../../cfg/cfgogl/practiceogl/shared_settings.cfg) |
| Combate y movimiento | PracticeOgl conserva `steady_boost` para infectados, godframes del Hunter de `1.8`, flags de infectados poco comunes de `55` y el ajuste de perdigón central en `0`. Omite tres ajustes de interacción con objetos y los bloqueos de bhop, salto hacia atrás, Spitter durante Tank y cámara al hombro. [Ajustes compartidos](../../cfg/cfgogl/practiceogl/shared_settings.cfg) · [Carga](../../cfg/cfgogl/practiceogl/shared_plugins.cfg) |

Se verificaron los CFG de apagado y mapas contra la revisión indicada y se validó el ZIP local. La prueba de carga, inicio y cambio de modo en SRCDS queda pendiente. [Guía de compilación y distribución](../build/README.md).
