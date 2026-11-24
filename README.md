# 📊 Bases de Datos - TP 2C 2025 (FRBA – Gestión de Cursos)

Trabajo Práctico de la materia **Bases de Datos** — Universidad Tecnológica Nacional, Facultad Regional Buenos Aires (**UTN.BA**), correspondiente al **2° Cuatrimestre de 2025**.

* **Curso:** `K3571`
* **Grupo:** `39 - INSERT_PROMOCIONADOS`
* **Esquema en Base de Datos:** `INSERT_PROMOCIONADOS`
* **Motor de Base de Datos:** Microsoft SQL Server (T-SQL)

---

## 👥 Integrantes

| Apellido y Nombre | Legajo |
| :--- | :---: |
| **Deza, Alexia** | `2035625` |
| **Fernández, Javier** *(Representante)* | `2133222` |
| **Loza Flores, Melina** | `1687645` |
| **Bidart, Franco** | `232712` |

---

## 📋 Descripción del Proyecto

El proyecto simula el diseño, migración e implementación de un sistema integral de gestión académica y administrativa para una institución educativa con múltiples sedes. El trabajo se divide en dos grandes etapas desarrolladas íntegramente en **T-SQL**:

1. **Modelo Transaccional (OLTP) y Migración:** Normalización de una única tabla maestra desorganizada (`gd_esquema.Maestra`) hacia un modelo relacional que soporta la gestión de sedes, cursos, inscripciones, evaluaciones parciales, trabajos prácticos, exámenes finales, facturación, pagos y encuestas de satisfacción.
2. **Modelo de Inteligencia de Negocios (BI):** Implementación de un Data Warehouse con esquema estrella/constelación (dimensiones y tablas de hechos) y desarrollo de vistas analíticas para alimentar tableros de control gerencial.

---

## 📁 Estructura del Repositorio

```text
├── data/
│   ├── script_creación_inicial.sql   # Creación del esquema transaccional, tablas, constraints, índices y migración
│   └── script_creacion_BI.sql        # Creación del modelo dimensional (BI), carga de hechos/dimensiones y vistas
├── Estrategia.pdf                    # Documentación detallada de decisiones de diseño, calidad de datos y métricas
├── MODELO DER.png                    # Diagrama Entidad-Relación del modelo transaccional (OLTP)
├── MODELO BI.png                     # Diagrama del modelo dimensional (Business Intelligence)
└── README.md                         # Documentación del repositorio
```

---

## 🗄️ Arquitectura de Datos

### 1. Modelo Transaccional (`script_creación_inicial.sql`)
Se diseñó un modelo normalizado bajo el esquema `INSERT_PROMOCIONADOS`, contemplando limpieza y saneamiento de anomalías detectadas en la tabla origen:
* **Tablas de referencia y ubicación:** `provincia`, `localidad`, `categoria`, `turno`, `dias`, `medio_pago`, `estado_inscripcion`.
* **Entidades principales:** `sede` (unificando datos institucionales), `profesor`, `alumno`.
* **Gestión académica:** `curso`, `dias_por_curso`, `modulos`, `inscripcion_curso`, `evaluacion`, `evaluacion_alumno`, `trabajo_practico`, `final`, `inscripcion_final`, `evaluacion_final`.
* **Finanzas y encuestas:** `periodo`, `factura`, `detalle_factura`, `pago`, `encuesta_respondida`, `pregunta`.
* **Calidad de datos aplicada:** Deduplicación de encuestas mediante `ROW_NUMBER()`, corrección de columnas invertidas entre provincia y localidad de sede, y resolución de registros gemelos en exámenes finales.

### 2. Modelo de Business Intelligence (`script_creacion_BI.sql`)
Modelo multidimensional diseñado para consultas analíticas rápidas:
* **Dimensiones (`[D]`):** `bi_tiempo` (año, semestre, mes), `bi_sede`, `bi_categorias_curso`, `bi_turnos_curso`, `bi_medio_de_pago`, `bi_rango_etario_alumnos`, `bi_rango_etario_profesores`, `bi_bloque_de_satisfaccion`.
* **Tablas de Hechos (`[H]`):** `bi_inscripcion`, `bi_cursada`, `bi_final`, `bi_pagos`, `bi_encuesta`.

#### 📈 Vistas Analíticas Implementadas
1. `categorias_turnos_top_3`: Top 3 de categorías y turnos más solicitados por año y sede.
2. `tasa_rechazo_inscripciones`: Porcentaje mensual de inscripciones rechazadas por sede.
3. `aprobacion_cursada_sede`: Porcentaje anual de aprobación de cursada por sede.
4. `promedio_finalizacion_curso`: Tiempo promedio (en meses) entre el inicio del curso y la aprobación del primer final por categoría y año.
5. `promedio_finales_rango`: Nota promedio de finales según rango etario del alumno y categoría por semestre.
6. `tasa_ausentismo_finales`: Porcentaje semestral de ausentismo a exámenes finales por sede.
7. `desvio_pagos_semestre`: Porcentaje semestral de pagos realizados fuera de término.
8. `tasa_morosidad_mensual`: Porcentaje mensual de deuda sobre la facturación esperada por sede.
9. `top_ingresos_categoria_anio_sede`: Top 3 de categorías con mayores ingresos por año y sede.
10. `indice_satisfaccion_rango_sede`: Índice de satisfacción anual según rango etario de los profesores y sede.

---

## 🚀 Cómo Ejecutar el Proyecto

### Prerrequisitos
* **Microsoft SQL Server 2019** (o superior).
* **SQL Server Management Studio (SSMS)** o extensión de SQL Server para VS Code.
* Base de datos `GD2C2025` creada y poblada previamente con la tabla maestra (`gd_esquema.Maestra`) provista por la cátedra.

### Pasos de Ejecución
1. Conectarse a la instancia de SQL Server y seleccionar la base de datos `GD2C2025`:
   ```sql
   USE GD2C2025;
   GO
   ```
2. Ejecutar de forma completa el script transaccional y de migración:
   ```text
   data/script_creación_inicial.sql
   ```
3. Una vez finalizada la migración sin errores, ejecutar el script del modelo dimensional y las vistas:
   ```text
   data/script_creacion_BI.sql
   ```
4. Consultar cualquiera de las vistas generadas dentro del esquema `INSERT_PROMOCIONADOS`, por ejemplo:
   ```sql
   SELECT * FROM INSERT_PROMOCIONADOS.categorias_turnos_top_3;
   ```
