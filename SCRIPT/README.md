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

Para el análisis y manipulación de los datos del conjunto `Iowa_Liquor_Sales.csv`, se requiere la instalación y carga de varios paquetes de R que proporcionan herramientas poderosas y flexibles. 
```
# Instalar los paquetes necesarios
install.packages("tidyverse") # colección de paquetes que facilita la importación, manipulación y visualización de datos.
install.packages("dplyr") # paquete para la transformación y manipulación de datos que proporciona funciones intuitivas.
install.packages("lubridate") # paquete que facilita el trabajo con fechas y horas, simplificando la manipulación y cálculo de intervalos de tiempo.
install.packages("SmartEDA") # paquete que automatiza la exploración de datos y la generación de informes.
install.packages("formattable") # paquete que permite crear tablas con opciones avanzadas de formato, mejorando la presentación y la capacidad de resaltar elementos clave mediante el uso de estilos condicionales.
```
```
# Cargar los paquetes
library(tidyverse)
library(dplyr)
library(lubridate)
library(SmartEDA)
library(formattable)
```
***Nota:***
*Aunque dplyr ya está incluido dentro del tidyverse, se recomienda su instalación explícita para asegurar que se dispone de la última versión. lubridate se incluye para un manejo eficiente de datos de tipo fecha y hora, facilitando la transformación y análisis de series temporales.*

### 4. Verificación de algunas características del dataset
Se realiza un resumen general del conjunto de datos, incluyendo la cantidad de observaciones, número de variables, cantidad de valores faltantes, entre otros, por medio de la función `ExpData` del paquete `SmartEDA`. El argumento type = 1 proporciona esta salida y la función `formattable` del paquete del mismo nombre ayuda a una mejor estilización de la salida.
```
formattable(ExpData(data=data,type=1)) 
```
### 5. Identificando la estructura de los datos
Se realiza un resumen a nivel de variable, donde para cada columna del conjunto de datos se proporciona información como el número de observaciones, el recuento de valores faltantes, el porcentaje de valores faltantes y el número de valores distintos, por medio de la función `ExpData` del paquete `SmartEDA`. El argumento type = 2 proporciona esta salida y la función `formattable` del paquete del mismo nombre ayuda a una mejor estilización de la salida.

```
formattable(ExpData(data=data,type=2))
```
### 6. Conversión de algunas variables
#### 6.1 Variable `Sale_Dollars`

En el análisis de datos, es común encontrar variables numéricas representadas como texto, especialmente cuando incluyen símbolos de moneda. La variable `Sale (Dollars)` en nuestro conjunto de datos `data` presenta este caso, al estar registrada con el símbolo del dólar y como tipo character. Para facilitar análisis numéricos y estadísticos, es necesario convertir esta variable a un formato numérico puro.

El siguiente fragmento de código en R demuestra cómo realizar esta conversión utilizando las librerías `dplyr` para manipulación de datos y funciones base de R para el manejo de strings. Este proceso implica dos pasos principales: primero, eliminar el símbolo del dólar de cada valor utilizando `gsub`; y segundo, convertir el resultado a tipo numérico con `as.numeric`.

```
# Convertir la variable Sale_Dollars a numérica
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
# Normaliza los nombres de ciudades a un formato uniforme
data <- data %>%
  mutate(City = toupper(City))
```

***Notas:***
- **mutate(City = toupper(City)):** Esta línea es el núcleo de nuestra operación de normalización. `mutate()` es una función de dplyr que nos permite crear o transformar columnas dentro de un dataframe. En este caso, estamos transformando la columna `City`.
- **toupper(City):** `toupper()` es una función base de R que convierte todas las letras de un string a mayúsculas. Al aplicar `toupper()` a la columna `City`, estamos asegurando que todos los nombres de ciudades sean homogéneos en cuanto a su capitalización, convirtiéndolos todos a mayúsculas.

### 7. Agrupación de Datos
La agrupación de datos por variables clave, nos permite realizar análisis detallados y operaciones específicas dentro de cada grupo, revelando patrones y tendencias únicas.

#### 7.1 Agrupación por una variable: `City`
Para facilitar análisis que requieren una visión detallada por ciudad, utilizamos la función `group_by` de `dplyr` para agrupar nuestro conjunto de datos `data` por la variable `City`. Este paso es fundamental para realizar operaciones subsiguientes, como cálculos de agregación o estadísticas descriptivas, de manera específica para cada ciudad representada en los datos.
El siguiente fragmento de código demuestra cómo agrupar los datos:

```
# Agrupar los datos por ciudad
g1 <- group_by(data, City)
```
- **group_by(data, City):** Agrupa el dataframe data por la variable `City`, permitiendo que cualquier operación subsiguiente se ejecute dentro de cada grupo de ciudad. Esto es especialmente útil para analizar patrones o calcular estadísticas que son específicas a cada ciudad.

#### 7.1.1 Cálculo de la Media de `Sale_Dollars` por Ciudad
Primero para asegurar la integridad de nuestros análisis, es fundamental conocer si existen **valores perdidos** en la variable `Sale_Dollars`. Esto nos permite evaluar la necesidad de limpieza de datos o imputación de valores antes de proceder.

```
# Mostrar en la consola el número de valores perdidos en Sale_Dollars
print(sum(is.na(data$Sale_Dollars)))
```
Con los datos **agrupados por ciudad** y habiendo confirmado la ausencia de **valores perdidos** en `Sale_Dollars`, calculamos la `media` de ventas para cada ciudad. Este análisis nos permite evaluar el rendimiento promedio de las ventas en diferentes ubicaciones, ofreciendo una base para comparaciones y toma de decisiones informadas.

```
# Calcular la media de las ventas por ciudad
summarise(g1, media = mean(Sale_Dollars))
```
Una vez hecho esto, queremos ahora demonstrar la eficiencia del operador pipe (`%>%`) en `dplyr` para realizar tareas de manipulación de datos en secuencia. En este caso, agrupamos nuestro conjunto de datos `data` por la variable `City` y calculamos la media de `Sale_Dollars` para cada ciudad en pasos consecutivos, todo en una única expresión concisa.

```
# Agrupar y calcular la media en una sola instruccion
data %>%
  group_by(City) %>%
  summarise(media = mean(Sale_Dollars))
```

#### 7.1.2 Análisis Detallado de Ventas por Ciudad con Múltiples Estadísticas
Para obtener una comprensión más profunda de las ventas en cada ciudad, extendemos nuestro análisis para incluir no solo la `media` de ventas (`Sale_Dollars`) sino también los valores `mínimos` y `máximos`. Esto nos da una visión completa del rango de ventas, complementando nuestra evaluación del desempeño de ventas por ciudad.

```
# Calcular multiples estadisticos por ciudad
data %>%
  group_by(City) %>%
  summarise(Media_Ventas = mean(Sale_Dollars),
            Mínimo_Ventas = min(Sale_Dollars),
            Máximo_Ventas = max(Sale_Dollars)) %>%
  print()
```
#### 7.2 Agrupación por dos variables: `City` y `Category Name`
Extendemos ahora la agrupación a dos variables, a `City` le añadimos `Category Name`, con lo cual el análisis se enfocará en entender cómo las ventas varían por ciudad y por categoría de producto. Este enfoque permitirá identificar tendencias de consumo locales y la popularidad de diferentes categorías de licor en distintas áreas. Antes de correr el código de agrupación verificamos algunas cosas de la nueva variable.

#### 7.2.1 Ver la distribución de la variable `Category Name`
```
# Ver la distribución y variación de las categorias de la variable Category Name
table(data$`Category Name`)
```
Vemos que Category Name tiene nombres repetidos en mayúsculas y en minúsculas, por tanto hay que estandarizar estos nombres.

#### 7.2.2 Normaliza los nombres de Category Name a un formato uniforme
```
# 7.2.2 Normaliza los nombres de Category Name a un formato uniforme
data <- data %>%
  mutate(`Category Name` = toupper(`Category Name`))
```
A continuación se explica brevemente el significado de las tres categorías de licores de la variable `Category Name`
- **CREAM LIQUEURS**: Los licores cremosos o "Cream Liqueurs" son bebidas alcohólicas que combinan crema con un licor base, como el whisky, y a menudo se les añaden sabores como chocolate, café, vainilla, entre otros.
- **FLAVORED RUM**: El ron con sabor o "Flavored Rum" se refiere a las variantes del ron tradicional a las que se les han añadido sabores naturales o artificiales.
- **VODKA FLAVORED**: El vodka con sabor o "Vodka Flavored" es vodka al que se le han añadido sabores durante el proceso de destilación o después de él. 

#### 7.2.3 Agrupando los datos por `City` y `Category Name`
```
# Agrupando los datos por `City` y `Category Name`
library(dplyr)

data %>%
  group_by(City, `Category Name`) %>%
  summarise(Media_Ventas = mean(Sale_Dollars),
            Mínimo_Ventas = min(Sale_Dollars),
            Máximo_Ventas = max(Sale_Dollars),
            Total_Ventas = sum(Sale_Dollars)) %>%
  arrange(City, `Category Name`) %>%
  print(n = Inf)
```
***Notas:***
Este código amplía nuestro análisis de ventas incorporando una agrupación adicional por `Category Name` junto con `City`. Realiza los siguientes pasos:
1. **`group_by(City, \`Category Name\`)`**: Agrupa los datos por ambas, ciudad y categoría de producto, permitiendo un análisis detallado de las ventas por tipo de producto dentro de cada localidad.
2. **`summarise(...)`**: Calcula la media (`Media_Ventas`), el mínimo (`Mínimo_Ventas`), el máximo (`Máximo_Ventas`), y el total (`Total_Ventas`) de `Sale_Dollars` para cada combinación de ciudad y categoría de producto.
3. **`arrange(City, \`Category Name\`)`**: Ordena los resultados para facilitar su revisión.
4. **`print(n = Inf)`**: Asegura que todos los grupos sean mostrados en la consola.

***Comentarios sobre los resultados:***

La desagregación de ventas por ciudad y categoría de producto revela patrones interesantes y variaciones en las preferencias de consumo entre las localidades:

- **Predominancia de Vodka Flavored:** En todas las ciudades, `VODKA FLAVORED` destaca como la categoría con el mayor total de ventas, subrayando su popularidad universal. Sin embargo, las variaciones en las medias, los mínimos y los máximos de ventas dentro de esta categoría sugieren diferencias en cómo se consume y se valora este producto en cada ciudad.

- **Variabilidad en el Consumo:** 
  - En **Cedar Rapids**, la categoría `VODKA FLAVORED` muestra una considerable variabilidad, con un mínimo de ventas de $3.36 y un máximo de $5040, reflejando una amplia gama de preferencias y posiblemente promociones o eventos que impulsan las ventas pico.
  - **Davenport** comparte esta preferencia por `VODKA FLAVORED`, pero con un precio medio de venta más alto comparado con otras categorías, lo que podría indicar una tendencia hacia productos premium o ediciones especiales dentro de esta categoría.
  - **Waterloo**, manteniendo la tendencia, también ve a `VODKA FLAVORED` como su categoría más vendida, con una media de ventas que supera las otras categorías, lo que sugiere una demanda constante y posiblemente una lealtad del consumidor hacia marcas o productos específicos en esta categoría.

- **Insights para Estrategias de Negocio:** La constante demanda de `VODKA FLAVORED` a través de las ciudades señala una oportunidad clave para las estrategias de marketing y de inventario. Sin embargo, las diferencias en las medias y los rangos de ventas sugieren que las tácticas pueden necesitar ser personalizadas para cada ciudad, aprovechando la popularidad general de `VODKA FLAVORED` mientras se adaptan a las peculiaridades locales en el comportamiento de compra.

Este análisis recalca la importancia de entender no solo qué categorías son populares, sino también cómo las características de las ventas varían entre las localidades. Estos insights pueden guiar a los negocios para afinar sus estrategias de oferta, promoción y precio, maximizando así su resonancia y eficacia en cada mercado local.


