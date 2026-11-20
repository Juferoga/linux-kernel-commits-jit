# Commits del kernel de Linux, 2019–2026, etiquetados por `Fixes:`

Tabla por commit de la rama principal de `torvalds/linux` (sin fusiones), para predecir **justo a tiempo** si un commit tendrá que corregirse. Se construyó para el proyecto final de Inteligencia Computacional (Maestría en Ciencias de la Información y las Comunicaciones, Universidad Distrital Francisco José de Caldas).

*Per-commit table of Linux kernel mainline commits (2019–2026), labelled by later `Fixes:` trailers, for just-in-time defect prediction.*

## Descarga

El archivo está publicado como [release](../../releases); no está en el historial de git.

```python
import hashlib, urllib.request
import pandas as pd

URL = "https://github.com/Juferoga/linux-kernel-commits-jit/releases/download/v1.0/kernel-commits-2019-2026.parquet"
SHA256 = "ac6858f4b16cb9ee4c7fd1f8baf3c28d480a729dbce9829665d9cdc65045fe9e"

urllib.request.urlretrieve(URL, "kernel-commits.parquet")
assert hashlib.sha256(open("kernel-commits.parquet", "rb").read()).hexdigest() == SHA256
df = pd.read_parquet("kernel-commits.parquet")
```

## Ficha (Datasheets for Datasets)

| Campo | Descripción |
|---|---|
| Nombre y versión | Commits de `torvalds/linux`, rama `master`, del clon del 2026-11-07. Versión v1.0 |
| Fuente | https://github.com/torvalds/linux |
| Licencia | La tabla derivada se publica bajo CC BY 4.0. El kernel es GPL-2.0 y sus metadatos de git son públicos |
| Tamaño | 641.926 filas × 33 columnas. De ellas, 26.133 están censuradas (`censurado = True`) |
| Periodo | 2019-01-01 a 2026-11-06 |
| Objetivo | `riesgoso` (0/1): un commit posterior de la rama principal lo cita con `Fixes:` en 90 días o menos |
| Censura | Los commits con menos de 90 días de observación antes de la fecha del clon no tienen etiqueta definitiva y se marcan con `censurado` |
| Faltantes | Ninguno. `dias_a_fix` vale `inf` cuando nadie lo corrigió |
| Sesgos conocidos | Solo cuentan los defectos que alguien corrigió con `Fixes:`. `drivers/` concentra cerca del 49 % de los commits |
| Uso previsto | Priorizar la revisión de commits. **No** debe usarse para evaluar a personas |

## Privacidad

El correo del autor se reemplazó por los 12 primeros caracteres de un SHA-256 con sal (`autor`). No se incluyen nombres, correos ni mensajes. La columna `hash` es el identificador público del commit, así que cualquiera puede consultarlo en el repositorio del kernel. La anonimización solo evita que la tabla exponga los correos de forma directa.

## Columnas

| Columna | Tipo | Qué es |
|---|---|---|
| `hash` | texto | SHA-1 del commit |
| `fecha` | fecha UTC | Fecha del committer, es decir, cuándo entró al árbol |
| `t_autor`, `t_commit` | entero | Marcas Unix de autor y de committer |
| `hora_utc`, `dia_semana` | entero | Hora (0–23) y día de la semana (0 = lunes), según la fecha de autor |
| `len_asunto` | entero | Longitud del asunto en caracteres |
| `asunto_fix` | 0/1 | El asunto contiene una palabra que empieza por *fix* (fix, fixes, fixed…) |
| `lineas_add`, `lineas_del` | entero | Líneas añadidas y borradas |
| `n_archivos`, `n_binarios` | entero | Archivos tocados, y cuántos de ellos son binarios |
| `n_dirs_raiz`, `n_subsistemas` | entero | Directorios raíz y subsistemas distintos tocados |
| `subsistema` | texto | Primer subsistema en orden alfabético entre los tocados; cada subsistema son los dos primeros niveles de la ruta, p. ej. `fs/ext4` |
| `n_reviewed`, `n_acked`, `n_tested`, `n_reported`, `n_signed`, `n_codev`, `n_link` | entero | Trailers `Reviewed-by`, `Acked-by`, `Tested-by`, `Reported-by`, `Signed-off-by`, `Co-developed-by` y `Link:` |
| `cc_stable` | 0/1 | Lleva `Cc: stable` |
| `es_fix` | 0/1 | El propio commit trae un `Fixes:` |
| `assisted_by`, `assisted_valor` | 0/1, texto | Trailer `Assisted-by` y su valor |
| `autor` | texto | Identificador anonimizado del autor |
| `exp_autor`, `exp_autor_subsistema` | entero | Commits previos del autor desde 2014, en total y en el mismo subsistema |
| `dias_desde_primer_commit` | real | Días desde el primer commit del autor desde 2014 |
| `dias_a_fix` | real | Días hasta el primer `Fixes:` que lo cita (`inf` si no lo cita nadie) |
| `riesgoso` | 0/1 | **Objetivo**: `dias_a_fix` ≤ 90 |
| `censurado` | lógico | Tiene menos de 90 días de observación |

Todas las variables se conocen en el momento de integrar el commit, salvo `dias_a_fix`, `riesgoso` y `censurado`, que se derivan de commits posteriores. No las uses como entradas de un modelo.

## Cómo citar

Rodríguez Galindo, J. F. (2026). *Commits del kernel de Linux, 2019–2026, etiquetados por `Fixes:`* (v1.0) [Conjunto de datos]. https://github.com/Juferoga/linux-kernel-commits-jit
