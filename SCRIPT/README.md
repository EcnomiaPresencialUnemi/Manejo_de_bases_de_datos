# Scripts para Análisis del Conjunto de Datos de Ventas de Licor de Iowa

Este script está dedicado a cargar el conjunto de datos `Iowa_Liquor_Sales.csv` desde una ubicación específica en el sistema. Utiliza la librería `readr` para leer eficientemente el archivo CSV y cargarlo en un dataframe en R. Es el primer paso necesario antes de realizar cualquier análisis.

### 1. Cargar el archivo
```
# Cargar la librería necesaria para leer archivos CSV
library(readr)
```

### 2. Asignación del dataset a un nuevo objeto para análisis 
Una vez cargado el conjunto de datos `Iowa_Liquor_Sales.csv` en R, el siguiente paso es asignarlo a un nuevo objeto llamado `data`. Esto se hace para asegurar que cualquier manipulación, análisis o transformación se realice en este nuevo objeto, manteniendo así el conjunto de datos original sin modificaciones. Este enfoque promueve las buenas prácticas de manejo de datos, permitiendo a los usuarios o colaboradores revertir o comparar sus análisis con el conjunto de datos original en cualquier momento.

```
# Asignar el conjunto de datos cargado a un nuevo objeto llamado 'data'
data <- Iowa_Liquor_Sales
```

### 3. Instalación y carga de los paquetes necesarios

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
***Nota:***
*Aunque dplyr ya está incluido dentro del tidyverse, se recomienda su instalación explícita para asegurar que se dispone de la última versión. lubridate se incluye para un manejo eficiente de datos de tipo fecha y hora, facilitando la transformación y análisis de series temporales.*

### 4. Verificación de las Dimensiones del Conjunto de Datos

Para aquellos momentos en los que se necesita una confirmación rápida del tamaño del conjunto de datos `datos`, proporcionamos una instrucción en R extremadamente sencilla. Este comando muestra en la consola el número de filas y columnas del conjunto de datos, facilitando una visión instantánea de sus dimensiones.

```
# Mostrar directamente el número de filas y columnas del conjunto de datos
dim(datos)
```
### 5. Identificando la Clase de las Variables del Conjunto de Datos

Comprender la clase (tipo de dato) de cada variable en el conjunto de datos `data` es crucial para un análisis de datos efectivo. Para facilitar este entendimiento de manera rápida y eficiente, proporcionamos una instrucción en R que aplica la función `class` a cada columna del dataframe. Esto resulta en una visión clara de la naturaleza de los datos contenidos en cada variable, preparando el camino para manipulaciones y análisis posteriores adecuados.

```
# Mostrar la clase de cada variable del conjunto de datos
sapply(data, class)
```
### 6. Conversión de algunas variables
#### 6.1 Variable `Sale_Dollars`

En el análisis de datos, es común encontrar variables numéricas representadas como texto, especialmente cuando incluyen símbolos de moneda. La variable `Sale (Dollars)` en nuestro conjunto de datos `data` presenta este caso, al estar registrada con el símbolo del dólar y como tipo character. Para facilitar análisis numéricos y estadísticos, es necesario convertir esta variable a un formato numérico puro.

El siguiente fragmento de código en R demuestra cómo realizar esta conversión utilizando las librerías `dplyr` para manipulación de datos y funciones base de R para el manejo de strings. Este proceso implica dos pasos principales: primero, eliminar el símbolo del dólar de cada valor utilizando `gsub`; y segundo, convertir el resultado a tipo numérico con `as.numeric`.

```
data <- data %>%
  mutate(Sale_Dollars = as.numeric(gsub("\\$", "", `Sale (Dollars)`)))
```

***Notas:***

- **`gsub("\\$", "", \`Sale (Dollars)\`)`:** Esta instrucción elimina el símbolo del dólar (`$`) de los valores en la columna `Sale (Dollars)`, preparándolos para su conversión a numérico.
- **`as.numeric(...)`:** Convierte la cadena de texto resultante (ya sin el símbolo del dólar) en un valor numérico.
- **`mutate(Sale_Dollars = ...)`:** Añade o modifica la columna `Sale_Dollars` en el dataframe `data`, aplicando las transformaciones anteriores. Esto asegura que la variable pueda ser utilizada en análisis numéricos subsiguientes.

#### 6.2 Variable `City`

Antes de proceder con la normalización o cualquier otro tipo de manipulación de los nombres de las ciudades en nuestro conjunto de datos `data`, es esencial realizar una exploración preliminar. Esto nos permite identificar las variaciones en la representación de los nombres de las ciudades, incluyendo diferencias en la capitalización, posibles errores de ortografía y otras inconsistencias que podrían afectar los análisis posteriores.
Para obtener una visión general de cómo están representados los nombres de las ciudades en la variable `City`, utilizamos la siguiente instrucción, que nos proporciona una tabla de frecuencias de todos los nombres únicos de ciudades presentes en el conjunto de datos:

```
# Ver la distribución y variación de los nombres de ciudades
table(data$City)
```
Tras identificar variaciones en la representación de los nombres de ciudades en nuestro conjunto de datos `data`, procedemos a normalizar estos nombres para asegurar consistencia en todo el conjunto. La normalización de los nombres de ciudades a un formato uniforme es crucial para análisis agrupados, comparaciones precisas y para evitar la duplicación innecesaria de categorías debido a diferencias en la capitalización.
Utilizamos la función `toupper()` de R en combinación con `mutate()` de `dplyr` para convertir todos los nombres de ciudades a mayúsculas:

```
data <- data %>%
  mutate(City = toupper(City))
```

***Notas:***
- **mutate(City = toupper(City)):** Esta línea es el núcleo de nuestra operación de normalización. `mutate()` es una función de dplyr que nos permite crear o transformar columnas dentro de un dataframe. En este caso, estamos transformando la columna `City`.
- **toupper(City):** `toupper()` es una función base de R que convierte todas las letras de un string a mayúsculas. Al aplicar `toupper()` a la columna `City`, estamos asegurando que todos los nombres de ciudades sean homogéneos en cuanto a su capitalización, convirtiéndolos todos a mayúsculas.

### 7. Agrupación de Datos
La agrupación de datos por variables clave, nos permite realizar análisis detallados y operaciones específicas dentro de cada grupo, revelando patrones y tendencias únicas.

#### 7.1 Agrupación por la variable `City`
Para facilitar análisis que requieren una visión detallada por ciudad, utilizamos la función `group_by` de `dplyr` para agrupar nuestro conjunto de datos `data` por la variable `City`. Este paso es fundamental para realizar operaciones subsiguientes, como cálculos de agregación o estadísticas descriptivas, de manera específica para cada ciudad representada en los datos.
El siguiente fragmento de código demuestra cómo agrupar los datos:

```
g1 <- group_by(data, City)
```
- **group_by(data, City):** Agrupa el dataframe data por la variable `City`, permitiendo que cualquier operación subsiguiente se ejecute dentro de cada grupo de ciudad. Esto es especialmente útil para analizar patrones o calcular estadísticas que son específicas a cada ciudad.

#### 7.1.1 Cálculo de la Media de `Sale_Dollars` por Ciudad
Para profundizar en nuestro análisis de ventas por ciudad, calculamos la media de `Sale_Dollars` dentro de cada grupo de ciudades. Esto nos permite comparar el desempeño promedio de ventas entre ciudades.

```
resumen <- summarise(g1, media = mean(Sale_Dollars, na.rm = TRUE))
```

- **g1**: representa nuestro conjunto de datos previamente agrupado por la variable City.
- **summarise(g1, media = mean(Sale_Dollars, na.rm = TRUE)):** Esta línea de código utiliza la función summarise para calcular la media de la variable Sale_Dollars para cada grupo de ciudad en g1. La opción na.rm = TRUE asegura que se omitan los valores NA en el cálculo de la media, lo cual es una práctica común para manejar datos faltantes y obtener resultados precisos.

