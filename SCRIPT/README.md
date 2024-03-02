## Script: cargar_datos.R

### Descripción
Este script está dedicado a cargar el conjunto de datos `Iowa_Liquor_Sales.csv` desde una ubicación específica en el sistema. Utiliza la librería `readr` para leer eficientemente el archivo CSV y cargarlo en un dataframe en R. Es el primer paso necesario antes de realizar cualquier análisis.

### Código
```r
# Cargar la librería necesaria para leer archivos CSV
library(readr)

# Especificar la ruta al archivo CSV
archivo_csv <- "/ruta/al/archivo/Iowa_Liquor_Sales.csv"

# Leer los datos del archivo CSV y almacenarlos en un dataframe
datos_liquor <- read_csv(archivo_csv)

# Imprimir las primeras filas para verificar la carga correcta
head(datos_liquor)
