# Proyecto 1 – Obtención y Limpieza de Datos
CC3084 – Data Science, UVG, Semestre II–2026

**INTEGRANTES:**
- Dulce Ambrosio (231143)
- Daniel Chet (231177)
- Javier Linares (231135)
- Cristian Túnchez (231359)

Establecimientos educativos de nivel **DIVERSIFICADO** a nivel nacional, obtenidos del
buscador de establecimientos de MINEDUC: http://www.mineduc.gob.gt/BUSCAESTABLECIMIENTO_GE/

## Estructura del repositorio

```
proyecto1_establecimientos/
├── src/                  # scripts de limpieza 
├── notebooks/
│   └── 01_diagnostico_inicial.ipynb   # carga, unión y diagnóstico del estado crudo
├── data/
│   ├── raw/               # 22 CSV originales, uno por departamento / Ciudad Capital (NO SE MODIFICARON)
│   └── processed/         # salidas generadas 
├── requirements.txt
└── README.md
```

## Cómo reproducir

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace notebooks/01_diagnostico_inicial.ipynb
```

Esto toma los 22 archivos de `data/raw/`, los une (sin modificarlos) y genera el diagnóstico
inicial. El resultado intermedio se guarda en `data/processed/`.

## Qué incluye el diagnóstico inicial

El notebook `notebooks/01_diagnostico_inicial.ipynb` presenta el estado crudo del conjunto de datos
mediante tablas y estadísticas generadas con código. En particular, documenta:

- número de registros y variables;
- tipo de dato de cada variable;
- cantidad y porcentaje de valores faltantes por variable;
- cantidad de valores únicos;
- cantidad de registros duplicados exactos;
- variables con valores fuera de dominio o inconsistentes;
- variables con formatos inconsistentes;
- identificación de problemas potenciales de calidad de datos.

Además, guarda una copia unida de trabajo en `data/processed/establecimientos_diversificado_crudo_unido.csv`
sin modificar los archivos originales de `data/raw/`.

## Fuente de datos

- **Fuente:** Ministerio de Educación de Guatemala (MINEDUC), buscador de establecimientos educativos.
- **Filtro aplicado:** Nivel escolar = TODOS, todos los departamentos.
- **Archivos:** 22 CSV, descargados por departamento (Ciudad Capital se exporta por separado de Guatemala).



