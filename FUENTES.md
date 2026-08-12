# Dataset CeMAED — General Pueyrredón

Índice de archivos y procedencia.

Actualizado el 2026-08-11.

---

## Archivos

| Archivo | Contenido | Filas |
|---|---|---|
| `cemaed_series.csv` | Serie trimestral, formato tidy | 336 |
| `cemaed_series_wide.csv` | Los mismos datos en formato ancho. Más cómodo para leer | 48 |
| `cemaed_barrios.csv` | Robos y hurtos de motos y autos por barrio, por trimestre | 225 |
| `cemaed_vehiculos.csv` | Modelos de vehículo más sustraídos, por trimestre | 118 |
| `cemaed_911_categorias.csv` | Llamados al 911, por motivo  | 24 |
| `relevamiento_prensa.csv` | **Relevamiento de prensa propio.** Un renglón por hecho documentado | 127 |
| `relevamiento_ciudad.csv` | **Relevamiento de ciudad.** Urbanismo, infraestructura, ambiente, historia, geografía, y datos en general de la ciudad | 1 |
| `relevamiento_prensa_links.csv` | Un renglón por artículo. | 190 |
| `relevamiento_prensa_relaciones.csv` | Un renglón por vínculo entre dos registros | 29 |
| `fuentes_monitoreo.md` | Índices de policiales a revisar para cargar el relevamiento | — |
| `../paginas_clave/*.png` | Los 7 resúmenes ejecutivos del CeMAED escaneados. | 7 |

**Los dos archivos de relevamiento no son estadística y no se mezclan con las series
 del CeMAED.** Registran lo publicado por la prensa y es agregado a mano a la base de datos.

**Criterios del relevamiento.** El campo `partido` separa General Pueyrredón del resto
de la región: los medios marplatenses cubren partidos vecinos, y sin ese campo cualquier
comparación futura contra el CeMAED —que solo abarca General Pueyrredón— daría mal sin
que se sepa por qué. Todo resumen de Mar del Plata filtra por `partido = General Pueyrredon`.

El campo `estado` distingue `confirmado` de `en_investigacion` y `sin_confirmar`. Permite
cargar un hecho el día que ocurre sin adelantarse a la calificación legal. El tipo
`muerte_dudosa` se usa mientras la carátula sea *averiguación de causales de muerte*; cuando
la justicia define, se reclasifica y cambia el `estado`.

**`tipo` y `modalidad` son dimensiones independientes.** 
`tipo` dice qué se atacó
—`robo_via_publica`, `robo_vivienda`, `robo_comercio`, `robo_automotor`, `homicidio`,
`violencia_genero`, `operativo`, `reclamo_vecinal`, `contexto`, `otro`—. 
`modalidad` dice
cómo, y admite más de un valor separado por comas: `motochorro`, `salidera`, `entradera`,
`forzamiento`, `escruche`, `mechera`, `arrebato`, `inhibidor`, `falsa_oferta_laboral`.

**Formato:** delimitador `;` y coma decimal.
Codificación UTF-8. Valores faltantes marcados como `n/d`.

---

## Cobertura

Siete trimestres consecutivos: **2024-Q3 a 2026-Q1**.

El CeMAED pasó a informes trimestrales a partir de julio de 2024; antes eran mensuales.
Por eso la serie trimestral empieza en 2024-Q3 y no antes.

**2026-Q2 (abril-junio) no está publicado** al 07/08/2026, 38 días después del cierre
del trimestre. El más reciente del índice sigue siendo el de 2026-Q1.

**No se conoce el retraso de publicación.** El índice del municipio
(`/Contenido/informes-periodicos`) lista los informes por período pero no informa
cuándo se publicó cada uno, y los PDFs tampoco traen fecha de edición. Lo único
verificable es que el informe de 2026-Q1 ya estaba disponible el 28/07/2026 y que el
de 2026-Q2 no lo está al 07/08/2026. Cualquier afirmación del tipo "publica con un
trimestre de retraso" es una estimación sin respaldo: no usarla como dato.

---

## Procedencia

Los 7 PDFs están en la carpeta padre. Se bajaron de la página oficial:

- Índice: https://www.mardelplata.gob.ar/Contenido/informes-periodicos


---

## Método

Los PDFs son mayormente imágenes: 60-93 páginas que dan solo ~5.000 caracteres de
texto extraíble. El resumen ejecutivo de la página 3 está rasterizado y se transcribió
leyendo el PNG renderizado. Las tablas de barrios y vehículos sí salen como texto y se
parsearon automáticamente.

Herramientas: `pdftotext -layout` y `pdftoppm -r 100` (poppler 25.07.0), más dos
scripts de parseo en Python.

## Relevamiento de ciudad

`relevamiento_ciudad.csv` registra noticias de interés urbano, geográfico o histórico de
Mar del Plata: obras, planeamiento, ambiente, patrimonio, transporte y servicios.


## Relaciones entre hechos

`relevamiento_prensa_relaciones.csv` guarda los vínculos que ninguna columna puede
expresar. Se lee como oración: **id_origen · tipo · id_destino**.


## Verificación (2026-07-28)

El grueso de los datos se transcribió del resumen ejecutivo de la página 3. Para
comprobar que ese resumen es fiel al cuerpo del informe se contrastaron seis cifras
del trimestre 2026-Q1 contra sus páginas de detalle.

| Cifra | Página | Resultado |
|---|---|---|
| Llamados al 911: 69.146 | 5 | Coincide |
| Homicidios: 16 hechos, 3/8/5 por mes | 58 | Coincide |
| Automotores: 655 (236/205/214) | 30 y 33 | Coincide |
| Viviendas: 397, 72% sin ocupantes | 22 | Coincide |
| Comercios: 182, 32 con arma de fuego | 18 | Coincide |
| Robos y hurtos otros bienes: 1.836 | 14 | Coincide |

Seis de seis. Además, los subtotales de cada página cierran contra su propio total
(por ejemplo viviendas: 336 robos + 2 tentativas + 59 hurtos = 397).

**Por qué el resumen resulta fiel:** no es una reescritura. Los párrafos del resumen
ejecutivo son los mismos textos que encabezan cada página de detalle, copiados. Eso
elimina el riesgo de error de transcripción interna.

Pruebas aritméticas sobre la serie completa: 25 de 26 sumas mensuales cierran contra
el total del trimestre, 7 de 7 en la composición de robos y hurtos, 7 de 7 en
homicidios, 5 de 5 en heridos. La única falla es un error del propio CeMAED (ver
punto 6 abajo).

---

## Discrepancias detectadas contra lo publicado en prensa

Registradas como control de calidad, sin interpretación.

1. **Llamados al 911, 2026-Q1.** El informe dice **69.146**. Varios medios y el informe
   del bloque UxP publicaron 69.416 (dígitos transpuestos).
2. **Homicidios por mes, 2026-Q1.** El informe dice **3 en enero, 8 en febrero, 5 en
   marzo**. Parte de la prensa publicó 7 para febrero.
3. **Homicidios 2025-Q1.** El informe dice **12 hechos**. El informe de UxP consigna una
   base de 10 para el cálculo interanual.
4. **Variaciones de homicidios en el informe UxP.** Sus dos columnas de variación están
   invertidas: el +33,3% corresponde a la comparación interanual (12→16) y el +60% a la
   comparación con el trimestre anterior medida en víctimas (10→16).
5. **Viviendas sin moradores, 2025-Q4.** El informe dice que el **88,5%** de los hechos
   fueron con modalidad *robo*, y que el **67%** ocurrió sin ocupantes. Parte de la
   prensa reportó el 88,5% como porcentaje de viviendas sin moradores.

6. **Error aritmético del propio CeMAED.** El informe 2025-Q4 dice que los robos y
   hurtos de otros bienes fueron **1752**, y desagrega 609 en octubre, 537 en noviembre
   y 607 en diciembre. Esos tres números suman **1753**. Verificado sobre la imagen: el
   informe dice literalmente eso. El CSV conserva el total publicado (1752).

7. **El CeMAED revisa la serie de homicidios hacia atrás.** El informe 2025-Q4 cierra el
   año con **34 muertes**. La tabla mensual del informe 2026-Q1 muestra para 2025 un
   total de **36**, con 12 en el cuarto trimestre en lugar de los 8 hechos / 10 víctimas
   informados en su momento. El CSV conserva lo publicado en cada informe; para la serie
   larga hay que usar la tabla del informe más reciente. 

8. **"Récord de robo de autos desde 2015".** El primer trimestre de 2015 registró **691**
   automotores sustraídos, por encima de los 655 de 2026-Q1. 


## Para agregar el próximo trimestre

1. Buscar el link nuevo en la página índice y bajar el PDF a la carpeta padre como
   `CeMAED_AAAA_QN.pdf`.
2. Renderizar la página 3 y transcribir el resumen ejecutivo.
3. Agregar una columna a `cemaed_series_wide.csv` y regenerar `cemaed_series.csv`.
4. Correr los parsers de barrios y vehículos sobre el texto nuevo.
