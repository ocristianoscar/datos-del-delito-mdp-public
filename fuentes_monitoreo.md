# Fuentes de monitoreo diario

Lista de lugares a revisar para cargar el relevamiento de prensa. No son noticias:
son índices. Por eso no entran como registros en `relevamiento_prensa_links.csv`.

## Secciones de policiales

| Medio | URL |
|---|---|
| La Capital | https://www.lacapitalmdp.com/temas/inseguridad/ |
| 0223 | https://www.0223.com.ar/tags/homicidios |
| Infobrisas | https://www.infobrisas.com/policiales |
| El Marplatense | https://www.elmarplatense.com/seccion/policiales |
| Ahora Mar del Plata | https://ahoramardelplata.com.ar/policiales |
| Qué Digital | https://quedigital.com.ar/ |

## Páginas por tema de La Capital

Son las más útiles porque agrupan por tipo de hecho, que es justo como está
organizado el dataset.

- Robos — https://www.lacapitalmdp.com/temas/robos/
- Homicidio — https://www.lacapitalmdp.com/temas/homicidio/
- Asalto — https://www.lacapitalmdp.com/temas/asalto/
- Robo a comercio — https://www.lacapitalmdp.com/temas/robo-a-comercio/
- Inseguridad — https://www.lacapitalmdp.com/temas/inseguridad/
- Femicidio — https://www.lacapitalmdp.com/temas/femicidio/
- Cacerolazo — https://www.lacapitalmdp.com/temas/cacerolazo-mar-del-plata/
- Patrulla Municipal — https://www.lacapitalmdp.com/temas/patrulla-municipal/

## Otros medios marplatenses a incorporar

Todavía sin cobertura en el relevamiento, pero relevados como fuente:
Noticias y Protagonistas, Cazador de Noticias, Canal 10, Canal 8, Canal 2,
La Tecla Mar del Plata, Región Mar del Plata, MDP Hoy, Mi 8.

## Fuente oficial

CeMAED — https://www.mardelplata.gob.ar/Contenido/informes-periodicos
Publica con aproximadamente un trimestre de retraso. Los PDF desde 2024 están
alojados en `storage.mardelplata.gov.ar` con URLs que no terminan en `.pdf`.

## Como agregar noticias y hechos


1. Recorrer las secciones de policiales de los seis medios principales
2. Por cada hecho nuevo: un renglón en `relevamiento_prensa.csv`
3. Por cada nota: un renglón en `relevamiento_prensa_links.csv` con el mismo `id_hecho`
4. Si el hecho ya está cargado y otro medio lo cubre, solo se agrega el link
