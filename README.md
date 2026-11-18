# Fonasa o Isapre: gasto de bolsillo en salud de los hogares (IX EPF)

**Proyecto final · Fundamentos de Programación para Análisis Económico · UdeC-EAN · 2026-2**
Equipo: Cristóbal Sepúlveda, Eduardo Guerrero

> Estado: **Hito 1 (propuesta)**. El repositorio irá creciendo con cada hito.
> La propuesta está en [`propuesta/propuesta.pdf`](propuesta/propuesta.pdf).

## Pregunta

¿Cuánto más gastan de su bolsillo en salud al mes los hogares cuyo jefe de hogar
está afiliado a una Isapre, comparados con hogares en Fonasa con igual ingreso,
número de integrantes, edad del jefe de hogar y macrozona?

## Datos

- **Fuente:** Instituto Nacional de Estadísticas (INE), *IX Encuesta de Presupuestos
  Familiares (EPF)*, octubre de 2021 a septiembre de 2022.
- **Página:** <https://www.ine.gob.cl/estadisticas/sociales/ingresos-y-gastos/encuesta-de-presupuestos-familiares>
  (Bases de datos › IX EPF).
- **Fecha de descarga:** 17-11-2026.
- **Unidad:** hogares de las capitales regionales (zonas urbanas de las 16 regiones),
  con factor de expansión `fe`.

| Archivo (en `data/raw/`) | Tamaño | Contenido |
|---|---:|---|
| [`base-personas-ix-epf-stata.dta`](https://www.ine.gob.cl/docs/default-source/encuesta-de-presupuestos-familiares/bbdd/ix-epf-(octubre-2021---septiembre-2022)/base-personas-ix-epf-stata.dta?sfvrsn=ba34a5ee_12&download=true) | 48 MB | 44.688 personas en 15.134 hogares · 314 variables |
| [`base-gastos-ix-epf-stata.dta`](https://www.ine.gob.cl/docs/default-source/encuesta-de-presupuestos-familiares/bbdd/ix-epf-(octubre-2021---septiembre-2022)/base-gastos-ix-epf-stata.dta?sfvrsn=27bebde3_8&download=true) | 205 MB | 970.773 registros de gasto · 14 variables |
| [`ccif-ix-epf-stata.dta`](https://www.ine.gob.cl/docs/default-source/encuesta-de-presupuestos-familiares/bbdd/ix-epf-(octubre-2021---septiembre-2022)/ccif-ix-epf-stata.dta?sfvrsn=130d900c_6&download=true) | 0,3 MB | Clasificador CCIF: 1.666 códigos |

> **Las bases no están en el repositorio** porque pesan más de lo que GitHub acepta
> (máximo 100 MB por archivo). Para reproducir, descárgarlas con los enlaces de la
> tabla y déjarlas en `data/raw/` con esos nombres, sin editarlas.

**Variables clave** (base de personas; `folio` identifica al hogar):

| Variable | Qué es | Rol |
|---|---|---|
| `sp02` | Sistema de salud (Fonasa A–D, Isapre, FF.AA., otro) | Variable de interés, del sustentador/a (`parentesco == 1`) |
| `gastot_d06_hd` | Gasto mensual del hogar en salud (división 06 CCIF), pesos | Variable dependiente |
| `ing_total_hogar_hd` | Ingreso total del hogar, pesos | Control |
| `npersonas`, `edad`, `sexo`, `macrozona` | Tamaño del hogar y características del jefe/a | Controles |
| `fe` | Factor de expansión | Ponderador |

## Estructura

```
Proyecto_Final/
├── data/
│   ├── raw/          bases originales de la EPF (sin editar)
│   └── processed/    base de hogares limpia que generan los scripts
├── scripts/          scripts numerados: 00_explorar.do → 01_limpieza.do → 02_modelo.do → 03_figuras.R
├── output/
│   ├── figuras/
│   └── tablas/
└── propuesta/        propuesta del Hito 1
```

## Reparto R / Stata

| Etapa | Herramienta | Por qué |
|---|---|---|
| `00_explorar.do` | Stata | Primera exploración: abre las bases y ubica las variables clave. |
| `01_limpieza.do` | Stata | Une bases de distinto nivel (gastos → hogar con `collapse`; personas → jefe de hogar) con `merge`; trata faltantes y outliers. |
| `02_modelo.do` | Stata | Regresión con categóricas (`i.`) y factor de expansión; tabla con `esttab`. |
| `03_figuras.R` | R | Figuras con `ggplot2`, con más control del diseño (lo trabajado en la T4). |

## Para reproducir

1. Descargar las tres bases con los enlaces de la tabla de Datos y dejarlas en `data/raw/`.
2. Abrir Stata parado en la carpeta del proyecto y correr los scripts en orden: `00` → `01` → `02` → `03`.

*(Se completa a medida que existan los scripts.)*

## Declaración de autoría y uso de IA
- Herramienta utilizada: Claude
- Para qué la usé: ayuda con redacción y organización del readme, además de ayuda con la busqueda de datos y estructura del PDF inicial
- Qué hice yo: Selección de la pregunta y analisis del tema y pregunta
- Verificación: confirmo que entiendo y puedo explicar todo lo que entrego.
