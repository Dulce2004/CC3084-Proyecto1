# Codebook: Establecimientos Educativos Nivel Diversificado (MINEDUC)

Conjunto de datos usado en `notebooks/01_diagnostico_inicial.ipynb` (y en las
etapas posteriores de limpieza). Contiene el listado de establecimientos
educativos de nivel **DIVERSIFICADO** de los 22 departamentos de Guatemala.

## Descripción general

- **Fuente:** Ministerio de Educación de Guatemala (MINEDUC), buscador de
  establecimientos educativos: http://www.mineduc.gob.gt/BUSCAESTABLECIMIENTO_GE/
- **Filtro aplicado:** NIVEL ESCOLAR = DIVERSIFICADO, los 22 departamentos del país.

- **Unidad de observación:** un establecimiento educativo de nivel diversificado.
- **Registros:** 11,867 (dato crudo, sin filas vacías de exportación; el total
  puede variar tras eliminar duplicados/registros cerrados en la limpieza final).
- **Variables originales:** 17. Se agregan variables derivadas durante la
  limpieza (ver sección "Variables derivadas").
- **Versión de este codebook:** v0 — basado en el diagnóstico inicial y el plan
  de limpieza (`AVANCE.md`). Se actualizará con la versión final del conjunto
  limpio (conteos, dominios corregidos y variables derivadas confirmadas).

## Tabla resumen

| Variable | Descripción | Tipo | Nivel de medición | Unidades | Valores permitidos / rango / ejemplo |
|---|---|---|---|---|---|
| `CODIGO` | Identificador único del establecimiento asignado por MINEDUC | string | Nominal | N/A | Formato `NN-NN-NNNN-NN`, ej. `16-01-0137-46` |
| `DISTRITO` | Código del distrito educativo al que pertenece el establecimiento | string | Nominal | N/A | Formato mixto en crudo (`NN-NNN` o `NN-NN-NNNN`); se estandariza en limpieza; puede ser NA |
| `DEPARTAMENTO` | Departamento donde se ubica el establecimiento | string | Nominal | N/A | Uno de los 22 departamentos oficiales de Guatemala, ej. `GUATEMALA`, `QUETZALTENANGO` (en crudo aparece además `CIUDAD CAPITAL`, que se reasigna a `GUATEMALA` en la limpieza) |
| `MUNICIPIO` | Municipio donde se ubica el establecimiento | string | Nominal | N/A | Municipio válido según catálogo oficial de Guatemala; en crudo, para Ciudad Capital contiene zonas (`ZONA 1`…`ZONA 25`) que se corrigen a `GUATEMALA` (ver variable derivada `zona_capital`) |
| `ESTABLECIMIENTO` | Nombre oficial del establecimiento educativo | string | Nominal | N/A | Texto libre en mayúsculas, ej. `INSTITUTO NORMAL MIXTO ALEJANDRO CORDOVA`; se limpian espacios y comillas, no la ortografía |
| `DIRECCION` | Dirección física del establecimiento | string | Nominal | N/A | Texto libre, ej. `6A. AVENIDA 1-15 ZONA 4`; valores tipo `-`, `.` se tratan como faltante (NA) |
| `TELEFONO` | Número(s) de teléfono de contacto | string | Nominal | N/A | 8 dígitos para Guatemala, ej. `79400455`; celdas con varios números se separan (ver variables derivadas) |
| `SUPERVISOR` | Nombre del supervisor educativo asignado | string | Nominal | N/A | Texto libre (nombre propio), puede ser NA |
| `DIRECTOR` | Nombre del director del establecimiento | string | Nominal | N/A | Texto libre (nombre propio); valores tipo `-`, `----` se tratan como faltante (NA) |
| `NIVEL` | Nivel escolar del establecimiento | string (category) | Nominal | N/A | Un solo valor en este conjunto: `DIVERSIFICADO` |
| `SECTOR` | Tipo de administración del establecimiento | string (category) | Nominal | N/A | `OFICIAL`, `PRIVADO`, `MUNICIPAL`, `COOPERATIVA` |
| `AREA` | Área geográfica del establecimiento | string (category) | Nominal | N/A | `URBANA`, `RURAL`; en crudo aparece también `SIN ESPECIFICAR` (queda como NA) |
| `STATUS` | Estado operativo del establecimiento | string (category) | Nominal | N/A | `ABIERTA`, `CERRADA TEMPORALMENTE`, `CERRADA DEFINITIVAMENTE`, `TEMPORAL TITULOS`, `TEMPORAL NOMBRAMIENTO` |
| `MODALIDAD` | Modalidad lingüística de enseñanza | string (category) | Nominal | N/A | `MONOLINGUE`, `BILINGUE` |
| `JORNADA` | Jornada en la que opera el establecimiento | string (category) | Nominal | N/A | `MATUTINA`, `VESPERTINA`, `NOCTURNA`, `DOBLE`, `INTERMEDIA`, `SIN JORNADA` |
| `PLAN` | Plan de estudios / frecuencia de clases | string (category) | Nominal | N/A | Ej. `DIARIO(REGULAR)`, `FIN DE SEMANA`, `SEMIPRESENCIAL (FIN DE SEMANA)`, `A DISTANCIA`, `VIRTUAL A DISTANCIA`, entre otros (13 categorías en crudo) |
| `DEPARTAMENTAL` | Dirección departamental de educación a cargo | string (category) | Nominal | N/A | 26 categorías en crudo (Guatemala se subdivide en `GUATEMALA NORTE/SUR/ORIENTE/OCCIDENTE`, Quiché en `QUICHÉ`/`QUICHÉ NORTE`); se documenta junto con `departamento_real` |

## Variables derivadas (planificadas en la limpieza, ver `AVANCE.md`)

| Variable derivada | Descripción | Tipo | Nivel de medición | Cómo se calcula | Utilidad |
|---|---|---|---|---|---|
| `telefono_principal` | Primer número telefónico válido (8 dígitos) extraído de `TELEFONO` | string | Nominal | Se extraen los grupos de dígitos de la celda original y se toma el primero con longitud plausible (8 dígitos) | Deja un teléfono de contacto limpio y consistente para reportes |
| `telefono_secundario` | Número(s) telefónico(s) adicionales encontrados en la misma celda | string | Nominal | Resto de los números extraídos de `TELEFONO`, si existen | Conserva información que de otro modo se perdería al limpiar `TELEFONO` |
| `zona_capital` | Zona de la Ciudad de Guatemala (cuando aplica) | string | Nominal | Se copia el valor original de `MUNICIPIO` para los registros de "Ciudad Capital" (ej. `ZONA 1`) antes de corregir `MUNICIPIO` a `GUATEMALA` | Conserva el detalle de ubicación dentro de la capital que se pierde al corregir `MUNICIPIO` |
| `departamento_real` | Departamento oficial estandarizado (uno de los 22) | string (category) | Nominal | Igual a `DEPARTAMENTO` ya corregido (`CIUDAD CAPITAL` → `GUATEMALA`) | Permite análisis por los 22 departamentos oficiales sin perder la variable administrativa original `DEPARTAMENTAL` |

## Notas de tratamiento 

- Los valores `-`, `--`, `---`, `.` y similares en `DIRECTOR`, `DIRECCION` y
  `SUPERVISOR` se convierten a `NA` real; no representan un nombre o dirección válidos.
- `ESTABLECIMIENTO` solo recibe limpieza de formato (espacios múltiples, comillas
  mal escapadas); se conserva la ortografía original (tildes, mayúsculas, "Ñ") tal
  como la reporta MINEDUC, ya que estos datos se usan para redactar informes.
- `DEPARTAMENTO` y `MUNICIPIO` se validan contra el catálogo oficial de
  departamentos y municipios de Guatemala.
- Las variables categóricas (`SECTOR`, `AREA`, `STATUS`, `MODALIDAD`, `JORNADA`,
  `PLAN`, `NIVEL`) se convierten a tipo `category` una vez confirmado su dominio.

## Fuente y versión

- **Fuente:** MINEDUC — buscador de establecimientos educativos (ver enlace arriba).
- **Versión del conjunto:** v0 (crudo diagnosticado). Se actualizará a v1 cuando
  se genere el conjunto limpio final (`data/processed/establecimientos_diversificado_limpio.csv`).
