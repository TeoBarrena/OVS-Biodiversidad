# OVS-Biodiversidad (Partido de La Plata)
Proyecto final de la materia Web Semántica y Grafos de Conocimiento 

Este proyecto integra datos del Observatorio de Valores del Suelo (OVS) con datos de biodiversidad para:
- Enriquecer el grafo RDF con agregados de observaciones por inmueble (buffer 500 m).
- Incorporar observaciones individuales de especies (con coordenadas).
- Ejecutar consultas SPARQL y generar mapas de calor de inmuebles y de observaciones de especies.

El flujo principal está en el notebook: `entrega_final.ipynb`.

## Requisitos

- Python 3.10+
- Java 8+ (o 11+) para ejecutar Apache Jena Fuseki
- Git
- Visual Studio Code con extensiones:
  - Python
  - Jupyter

## Clonar el repositorio

En PowerShell o CMD:

```powershell
git clone https://github.com/TeoBarrena/OVS-Biodiversidad.git
cd <OVS-Biodiversidad>
```

## Crear entorno virtual e instalar dependencias

```powershell
py -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
```
```
%pip install -r requirements.txt
```
O directamente ejecutar la primer celda de código del archivo entrega_final.ipynb que se encarga de instalar las dependencias necesarias.

## Cargar los datos en Apache Jena Fuseki

Para consultas SPARQL se usa el endpoint: `http://localhost:3030/OVS-Biodiversidad/query`. Para que funcione:

1. Descarga Apache Jena Fuseki desde: https://jena.apache.org/download/index.cgi
2. Descomprime el archivo y abre una terminal en la carpeta donde está `fuseki-server.jar`.
3. Ejecuta el servidor:
   ```powershell
   java -jar fuseki-server.jar
   ```
4. Abre `http://localhost:3030` en el navegador.
5. Crea un dataset llamado exactamente: `OVS-Biodiversidad`.
6. Carga el archivo TTL incluido en el repo:
   - Ruta: `grafos/biodiv_inmuebles_la_plata_COMPLETO.ttl`
   - Cárgalo en el dataset `OVS-Biodiversidad`.

Si se desea usar otro motor de consultas sparql, la sintáxis de las consultas deberían funcionar igualmente.

## Abrir y ejecutar el notebook

Opción VS Code:
- Abrir la carpeta del proyecto en VS Code.
- Abrir `entrega_final.ipynb`.
- Selecciona el intérprete de Python del entorno `.venv`.
- Ejecuta celda por celda.

Opción Jupyter:
```powershell
jupyter notebook
```
Luego abrir `entrega_final.ipynb`.

## Estructura del notebook (entrega_final.ipynb)

1. Instalación de dependencias
   - Se ejecuta la celda con `%pip install -r requirements.txt`.

2. Generación de CSV de observaciones (opcional)
   - Combinación y depuración de CSVs de iNaturalist para producir `csv/datos_especies_final.csv`.

3. Generador del grafo RDF (.ttl) — opcional, porque ya está incluido en la carpeta "grafos"
   - Creación de `grafos/biodiv_inmuebles_la_plata_COMPLETO.ttl` con:
     - Agregados de observaciones por inmueble (buffer 500 m, conteos).
     - Observaciones individuales de especies con coordenadas.
   - Aviso: tarda ~28 minutos.

4. Consultas SPARQL para inmuebles
   - Generación de `mapa_calor_sparql/datos_basicos.csv` con:
     - URI del inmueble, punto WKT, conteo total de especies únicas.

5. Mapa de impacto por inmueble (colores por cantidad de especies)
   - Generación de `mapa_calor_sparql/mapa_impacto_inmuebles.html`.

6. Consulta SPARQL solo de inmuebles (para heatmap)
   - Generación de `mapa_calor_sparql/datos_inmuebles.csv`.

7. Mapa de calor de inmuebles
   - Generación de `mapa_calor_sparql/mapa_calor_inmuebles.html`.

8. Consulta SPARQL de observaciones individuales de especies
   - Generación de `mapa_calor_sparql/datos_especies_individuales.csv` con:
     - URI de observación, WKT, URI de especie, nombre común (si existe), reino (si existe).

9. Mapa de calor de observaciones de especies
   - Generación de `mapa_calor_sparql/mapa_calor_especies.html`.

## Salidas generadas

- CSV:
  - `mapa_calor_sparql/datos_basicos.csv`
  - `mapa_calor_sparql/datos_inmuebles.csv`
  - `mapa_calor_sparql/datos_especies_individuales.csv`
- Mapas:
  - `mapa_calor_sparql/mapa_impacto_inmuebles.html`
  - `mapa_calor_sparql/mapa_calor_inmuebles.html`
  - `mapa_calor_sparql/mapa_calor_especies.html`

Abrir los archivos HTML con un navegador, para la visualización del mapa creado.
