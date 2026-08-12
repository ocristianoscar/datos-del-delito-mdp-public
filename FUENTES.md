# Dataset CeMAED — General Pueyrredón

Índice de archivos y procedencia. **Sin análisis:** esto es solo el registro de qué hay,
de dónde salió y qué falta.

Armado el 2026-07-28.

---

## Archivos

| Archivo | Contenido | Filas |
|---|---|---|
| `cemaed_series.csv` | Serie trimestral, formato tidy (una fila por período + indicador). Es el archivo para agregar trimestres nuevos | 336 |
| `cemaed_series_wide.csv` | Los mismos datos en formato ancho (indicador × trimestre). Más cómodo para leer a ojo en OpenOffice | 48 |
| `cemaed_barrios.csv` | Robos y hurtos de motos y autos por barrio, por trimestre | 225 |
| `cemaed_vehiculos.csv` | Modelos de vehículo más sustraídos, por trimestre | 118 |
| `cemaed_911_categorias.csv` | Llamados al 911 desagregados por motivo (2025-Q4 y 2026-Q1) | 24 |
| `relevamiento_prensa.csv` | **Relevamiento de prensa propio.** Un renglón por hecho documentado | 127 |
| `relevamiento_ciudad.csv` | **Relevamiento de ciudad.** Urbanismo, infraestructura, ambiente, historia y geografía | 1 |
| `relevamiento_prensa_links.csv` | Un renglón por artículo. Sirve a los dos relevamientos | 190 |
| `relevamiento_prensa_relaciones.csv` | Un renglón por vínculo entre dos registros | 29 |
| `fuentes_monitoreo.md` | Índices de policiales a revisar para cargar el relevamiento | — |
| `../paginas_clave/*.png` | Los 7 resúmenes ejecutivos escaneados. Respaldo visual para verificar cualquier número | 7 |

> **Los dos archivos de relevamiento no son estadística y no se mezclan con las series
> del CeMAED.** Registran lo publicado por la prensa, no la totalidad de los hechos.
> Sirven para modalidades, casos, actores y cobertura; nunca para volumen ni tasas.

**Criterios del relevamiento.** El campo `partido` separa General Pueyrredón del resto
de la región: los medios marplatenses cubren partidos vecinos, y sin ese campo cualquier
comparación futura contra el CeMAED —que solo abarca General Pueyrredón— daría mal sin
que se sepa por qué. Todo resumen de Mar del Plata filtra por `partido = General Pueyrredon`.

El campo `estado` distingue `confirmado` de `en_investigacion` y `sin_confirmar`. Permite
cargar un hecho el día que ocurre sin adelantarse a la calificación de la fiscalía. El tipo
`muerte_dudosa` se usa mientras la carátula sea *averiguación de causales de muerte*; cuando
la justicia define, se reclasifica y cambia el `estado`.

**`tipo` y `modalidad` son dimensiones independientes.** `tipo` dice qué se atacó
—`robo_via_publica`, `robo_vivienda`, `robo_comercio`, `robo_automotor`, `homicidio`,
`violencia_genero`, `operativo`, `reclamo_vecinal`, `contexto`, `otro`—. `modalidad` dice
cómo, y admite más de un valor separado por comas: `motochorro`, `salidera`, `entradera`,
`forzamiento`, `escruche`, `mechera`, `arrebato`, `inhibidor`, `falsa_oferta_laboral`.

Un mismo hecho puede ser salidera **y** motochorro: una cosa es cómo eligieron a la víctima
y otra cómo llegaron. Meter las dos en un solo campo obliga a una elección falsa, y por eso
se separaron. Una entradera tampoco es un tipo: es un robo a vivienda con un modus
específico.

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
- Los PDFs de 2024 en adelante están alojados en `storage.mardelplata.gov.ar`
  (Nextcloud, dominio `.gov.ar`). Las URLs **no terminan en `.pdf`**: son shares
  con token del tipo `/index.php/s/TOKEN`. Se bajan agregando `/download` al final.

| Reporte | Token |
|---|---|
| 2026-Q1 | `rLNdDjGCCgYTem5` |
| 2025-Q4 | `qYYR7pD2bWGTmHT` |
| 2025-Q3 | `HiTn7ReiXT5qNEk` |
| 2025-Q2 | `2FSEMZXBtzRkNgd` |
| 2025-Q1 | `9msMcB5LAtGzZoS` |
| 2024-Q4 | `mFnwSKt2gN4mW7c` |
| 2024-Q3 | `pS9W3HWMHc4Q7gq` |

**Página de origen de los datos:** casi todo sale de la **página 3 de cada PDF**,
que es el "Resumen ejecutivo" y trae las cifras en prosa. Los barrios y los modelos
de vehículo salen de tablas que sí quedan como texto seleccionable.

---

## Método

Los PDFs son mayormente imágenes: 60-93 páginas que dan solo ~5.000 caracteres de
texto extraíble. El resumen ejecutivo de la página 3 está rasterizado y se transcribió
leyendo el PNG renderizado. Las tablas de barrios y vehículos sí salen como texto y se
parsearon automáticamente.

Herramientas: `pdftotext -layout` y `pdftoppm -r 100` (poppler 25.07.0), más dos
scripts de parseo en Python.

---

## Criterio de barrio (relevamiento de prensa)

**El campo `barrio` es donde ocurrió el hecho.** No donde empezó, no de dónde salió la
víctima, no dónde apareció después lo robado. Las otras ubicaciones se conservan en
`notas`, porque suelen importar: una salidera tiene banco y punto de asalto, un robo de
auto tiene lugar del hecho y lugar de recuperación.

Ejemplo cargado: en `M09` el banco está en Libertad y Dorrego (Nueva Pompeya) y el
asalto ocurrió en Libertad entre Jujuy y España (La Perla). El registro va como La
Perla, con el banco anotado.

**Las direcciones se chequean contra los límites de barrio de OpenStreetMap, nunca
contra el nombre de la calle.** Los medios marplatenses nombran el barrio por la
avenida y se equivocan seguido. Casos detectados: 0223 ubicó en "barrio Libertad" un
almacén de avenida Libertad y España que está en La Perla, y El Retrato de Hoy ubicó
ahí también el homicidio del ferretero, que ocurrió en Malvinas Argentinas.

Tramos verificados de avenida Libertad: **y Dorrego** = Nueva Pompeya · **y España** =
La Perla · **al 7700** = Malvinas Argentinas.

**Los hechos sin punto único no llevan barrio.** Una marcha que cruza la ciudad o un
reclamo gremial van con el campo vacío, salvo que el hecho registrable tenga una
ubicación propia.

---

## Relevamiento de ciudad

`relevamiento_ciudad.csv` registra noticias de interés urbano, geográfico o histórico de
Mar del Plata: obras, planeamiento, ambiente, patrimonio, transporte y servicios.

**Es un dataset separado y no se mezcla con el de delito.** De las catorce columnas del
relevamiento de prensa, una noticia de infraestructura llenaría cuatro: no tiene arma, ni
detenidos, ni víctima, ni modalidad. Y necesita campos que allá no existen. Si compartieran
archivo, cualquier conteo de hechos sumaría reservorios pluviales junto con robos.

| Columna | Contenido |
|---|---|
| `id` | Prefijo **`U`** de urbano |
| `fecha` · `periodo` · `partido` | Igual que en el relevamiento de prensa |
| `tema` | `infraestructura_hidraulica`, `urbanismo`, `transporte`, `ambiente`, `espacio_publico`, `patrimonio`, `historia`, `geografia`, `servicios` |
| `etapa` | `proyecto`, `aprobado`, `licitado`, `en_obra`, `terminado`, `archivado`, `sin_datos` |
| `zona` | La geografía, que acá casi nunca es un punto |
| `barrio` | Admite varios, separados por coma |
| `organismo` | Quién ejecuta |
| `impulsor` | Quién lo empuja, con nombre y cargo |
| `notas` | Lo demás |

**`etapa` es la columna que justifica el archivo.** Una noticia de planeamiento no vale por
el día en que se publicó: vale por si avanza o se muere. Sin ese campo, en seis meses queda
un montón de anuncios y ninguna forma de saber cuáles se concretaron.

**Los dos relevamientos comparten espacio de identificadores, archivo de notas y archivo de
relaciones.** Por eso se puede enlazar un barrio que se inunda con un barrio donde se roba,
o una obra con el reclamo vecinal que la pidió, sin duplicar nada. La validación de
relaciones busca los identificadores en **los dos** datasets.

**Separadores.** El delimitador es `;`, así que ese carácter no puede aparecer dentro de
ningún campo. Los valores múltiples van separados por coma, la misma convención que
`modalidad` en el relevamiento de prensa.

---

## Relaciones entre hechos

`relevamiento_prensa_relaciones.csv` guarda los vínculos que ninguna columna puede
expresar. Se lee como oración: **id_origen · tipo · id_destino**.

**Regla que evita que el archivo se infle: una relación solo existe si ningún campo ya
la expresa.** Si lo que une a dos registros es el tipo, la modalidad, el barrio, la
fecha, el arma o el partido, eso no es una relación: es un filtro. Los tres casos de
falsa oferta laboral no se enlazan, porque comparten `modalidad`. Los dos indicadores
económicos tampoco, porque comparten `tipo` y período.

**Tipos dirigidos.** El orden importa.

| Tipo | Se lee |
|---|---|
| `consecuencia_de` | el origen ocurrió a raíz del destino, **según una fuente** |
| `actualiza` | el origen es una versión posterior del mismo asunto |
| `contradice` | el origen afirma lo contrario que el destino |
| `responde_a` | el origen es la respuesta de una parte a lo que dijo la otra |

**Tipos simétricos.** El orden da igual. Se guardan una sola vez y **el destino es
siempre el `id` menor**, que funciona como ancla del grupo: para reunir un grupo de tres
o más se enlazan todos contra esa ancla.

| Tipo | Significado |
|---|---|
| `mismo_hecho` | dos registros del mismo suceso; uno sobra |
| `posible_duplicado` | se sospecha que son el mismo hecho, sin confirmar |
| `posible_vinculo` | hechos **distintos** con sospecha de vínculo que ninguna fuente oficial afirmó |
| `misma_serie` | episodios agrupados: misma noche, misma banda, mismo detenido, misma cobertura |
| `mismo_objetivo` | la misma víctima o el mismo comercio, en hechos distintos |

`consecuencia_de` **nunca afirma causalidad propia**: se usa solo cuando una fuente lo
plantea, y el motivo va en la columna `nota`. El despliegue de 170 efectivos se enlaza a
la marcha del 5 de agosto porque el titular dice "Tras los reclamos por seguridad", no
porque lo dedujéramos nosotros.

**Validación al escribir el archivo:** los dos `id` deben existir en
`relevamiento_prensa.csv`, el tipo debe pertenecer al vocabulario, no se admiten
autorreferencias, ni pares repetidos, ni una simétrica cargada dos veces en sentidos
opuestos.

---

## Huecos conocidos

- `delitos_contra_propiedad_total` no figura en el resumen de 2025-Q1. En 2024-Q3 y
  2024-Q4 el valor es la suma de los componentes declarados, no una cifra publicada.
- `comercios_total` de 2025-Q1 (163) es derivado: el informe 2025-Q2 dice "229, 66 más
  que en el primero". No está publicado de forma directa.
- Armas de fuego en autos, motos y viviendas: el dato se deja de informar en 2025-Q4
  y 2026-Q1.
- `via_publica_hechos` no figura en 2025-Q2 ni 2025-Q3.
- `lesiones_denuncias` y `amenazas_denuncias`: a partir de 2025-Q3 el informe pasa a
  darlos solo como porcentaje del total contra las personas, sin el número absoluto.
- `cemaed_vehiculos.csv` no tiene 2025-Q1 ni 2025-Q2: en esos dos informes la tabla de
  modelos aparece sin la marca adelante y el parser no la reconoce.
- `cemaed_barrios.csv` tiene robos y hurtos de **autos** solo en 2025-Q2. El resto de
  los trimestres solo publica el desglose barrial de **motos**.

---

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

**Lo que el resumen sí hace es recortar.** Las páginas de detalle contienen datos que
el resumen omite por completo: franjas horarias, día de la semana, perfil de víctimas
por edad y sexo, desglose por comisaría, mapas de zonas y puntos calientes por tipo de
delito, y el desglose barrial de viviendas, comercios y vía pública. Para cualquier
corte más fino que el agregado trimestral hay que ir al cuerpo del informe.

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

---

6. **Error aritmético del propio CeMAED.** El informe 2025-Q4 dice que los robos y
   hurtos de otros bienes fueron **1752**, y desagrega 609 en octubre, 537 en noviembre
   y 607 en diciembre. Esos tres números suman **1753**. Verificado sobre la imagen: el
   informe dice literalmente eso. El CSV conserva el total publicado (1752).

7. **El CeMAED revisa la serie de homicidios hacia atrás.** El informe 2025-Q4 cierra el
   año con **34 muertes**. La tabla mensual del informe 2026-Q1 muestra para 2025 un
   total de **36**, con 12 en el cuarto trimestre en lugar de los 8 hechos / 10 víctimas
   informados en su momento. El CSV conserva lo publicado en cada informe; para la serie
   larga hay que usar la tabla del informe más reciente. Serie anual según el informe
   2026-Q1: 2013=89, 2014=77, 2015=73, 2016=40, 2017=35, 2018=45, 2019=46, 2020=32,
   2021=41, 2022=32, 2023=43, 2024=40, 2025=36.

8. **"Récord de robo de autos desde 2015".** El primer trimestre de 2015 registró **691**
   automotores sustraídos, por encima de los 655 de 2026-Q1. La serie de primeros
   trimestres es: 2015=691, 2016=347, 2017=299, 2018=248, 2019=323, 2020=264, 2021=261,
   2022=202, 2023=265, 2024=282, 2025=567, 2026=655.

---

## Para agregar el próximo trimestre

1. Buscar el link nuevo en la página índice y bajar el PDF a la carpeta padre como
   `CeMAED_AAAA_QN.pdf`.
2. Renderizar la página 3 y transcribir el resumen ejecutivo.
3. Agregar una columna a `cemaed_series_wide.csv` y regenerar `cemaed_series.csv`.
4. Correr los parsers de barrios y vehículos sobre el texto nuevo.
