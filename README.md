# Datos del delito en Mar del Plata — versión pública
https://www.instagram.com/datos.del.delito.mdp/

Esta es la versión anonimizada del proyecto. No es el dataset de trabajo; es una copia generalizada a partir de él.

## Qué contiene

- `relevamiento_prensa.csv` — hechos policiales relevados de prensa marplatense, con
  nombres propios de víctimas, detenidos y familiares reemplazados por rol, y direcciones de
  vivienda generalizadas.
- `relevamiento_ciudad.csv` — noticias de interés urbano, geográfico e histórico de la
  ciudad.
- `relevamiento_prensa_links.csv` — medio y fecha de publicación de cada nota que respalda
  un hecho. No incluye URL ni título.
- `relevamiento_prensa_relaciones.csv` — vínculos entre hechos (consecuencia, contradicción,
  misma serie, etc.), sin cambios respecto del dataset interno.
- `cemaed_*.csv` — estadística oficial trimestral del Centro Municipal de Análisis
  Estratégico del Delito de General Pueyrredón, agregada y sin datos personales. Sin
  cambios.
- `FUENTES.md` y `fuentes_monitoreo.md` — metodología del relevamiento. Sin cambios.

## Qué se generalizó, y por qué

**Nombres propios de víctimas, personas detenidas y familiares que declararon** se
reemplazaron por su rol y edad cuando estaba disponible ("la víctima", "un detenido de 23
años"). 

**Los menores de edad** perdieron además su edad exacta (pasa a "menor de edad") y
cualquier ubicación más fina que el barrio.

**Las direcciones de vivienda** conservan la calle pero no el número, cuando el hecho fue un
robo o intento de robo a una casa ocupada. 

**Los funcionarios públicos, fiscales, comisarios y voceros institucionales** que declararon
en carácter oficial se mantienen con nombre completo. 

## Lo que este dataset no garantiza

Las notas periodísticas originales, citadas por medio y fecha en `relevamiento_prensa_links.csv`,
pueden contener el nombre completo de las mismas personas que acá aparecen generalizadas —
así las publicó cada medio. 

Este es un criterio editorial de anonimización, aplicado caso por caso. No
constituye una garantía legal de cumplimiento normativo.

## Método

Ver `FUENTES.md` para la metodología completa del relevamiento: criterios de carga,
verificación cruzada de fechas y cifras contra el CeMAED, y las discrepancias detectadas
entre distintas coberturas de prensa.
