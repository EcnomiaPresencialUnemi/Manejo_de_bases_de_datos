# Scripts para Análisis del Conjunto de Datos de Ventas de Licor de Iowa

Este script está dedicado a cargar el conjunto de datos `Iowa_Liquor_Sales.csv` desde una ubicación específica en el sistema. Utiliza la librería `readr` para leer eficientemente el archivo CSV y cargarlo en un dataframe en R. Es el primer paso necesario antes de realizar cualquier análisis.

### 1. CARGAR EL ARCHIVO
```
# Cargar la librería necesaria para leer archivos CSV

library(readr)
```

### 2. ASIGNACION DEL DATA SET A UN NUEVO OBJETO PARA ANALISIS
Una vez cargado el conjunto de datos `Iowa_Liquor_Sales.csv` en R, el siguiente paso es asignarlo a un nuevo objeto llamado `data`. Esto se hace para asegurar que cualquier manipulación, análisis o transformación se realice en este nuevo objeto, manteniendo así el conjunto de datos original sin modificaciones. Este enfoque promueve las buenas prácticas de manejo de datos, permitiendo a los usuarios o colaboradores revertir o comparar sus análisis con el conjunto de datos original en cualquier momento.

```
# Asignar el conjunto de datos cargado a un nuevo objeto llamado 'data'
data <- Iowa_Liquor_Sales
```

### 3. INSTALACIÓN Y CARGA DE LOS PAQUETES NECESARIOS

Para el análisis y manipulación de los datos del conjunto `Iowa_Liquor_Sales.csv`, se requiere la instalación y carga de varios paquetes de R que proporcionan herramientas poderosas y flexibles. Estos paquetes incluyen `tidyverse` para una amplia gama de funciones de manipulación de datos y visualización, `dplyr` para la transformación y manipulación de datos, y `lubridate` para facilitar el trabajo con fechas y horas.

```
# Instalar los paquetes necesarios
install.packages("tidyverse")
install.packages("dplyr")
install.packages("lubridate")

# Cargar los paquetes
library(tidyverse)
library(dplyr)
library(lubridate)
```
***Nota***
Aunque dplyr ya está incluido dentro del tidyverse, se recomienda su instalación explícita para asegurar que se dispone de la última versión. lubridate se incluye para un manejo eficiente de datos de tipo fecha y hora, facilitando la transformación y análisis de series temporales.



