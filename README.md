# Introducción

El dataset de Superstore es una colección de datos de ventas de una cadena de supermercados ficticia llamada Superstore. Este conjunto de datos contiene información sobre ventas de diversos productos, como muebles, suministros de oficina, tecnología, etc. Además, el conjunto de datos contiene información sobre las regiones en las que se realizaron las ventas, los clientes que realizaron las compras, los vendedores que realizaron las ventas, etc.

Este conjunto de datos es muy útil para realizar análisis y estudios de mercado en el sector minorista. Los analistas pueden utilizar este conjunto de datos para identificar patrones de ventas, tendencias de mercado, productos más vendidos, etc. Además, el conjunto de datos también puede ser utilizado para realizar estudios de satisfacción del cliente y para identificar áreas de mejora en el servicio al cliente.

# Objetivos

En esta primera parte, usaremos este dataset para hacer el análisis y previsión de ventas de dos de los productos contenidos (los de mayores ventas) con técnicas aplicables a series temporales. En una segunda parte, se elaborará un panel de mandos con el que hacer un análisis más global útil para varias áreas de la empresa.

# Datos

Los datos pueden descargarse [aquí](https://community.tableau.com/s/question/0D54T00000CWeX8SAL/sample-superstore-sales-excelxls).

## Tablas

Se trata de un archivo xls con 3 tablas. Cada una tiene las siguientes variables:

**Tabla “Orders”**

- Row ID
- Order ID
- Order Date
- Ship Date
- Ship Mode
- Customer ID
- Customer Name
- Segment
- Country
- City
- State
- Postal Code
- Region
- Product ID
- Category
- Sub Category
- Product Name
- Sales
- Quantity
- Discount
- Profit

Esta tabla presenta los pedidos realizados entre enero de 2014 y diciembre de 2017, es decir, 4 años de ventas en varias tiendas de la empresa en USA.

**Tabla “Returns”**

- Returned
- Order ID

**Tabla “People”**

- Person
- Region

Sobre esta división, indicar lo siguiente:

- No parece que el modelado sea el adecuado. En la tabla Orders vienen varias variables que deberían ser trasladadas a una dimension table. La fact table debería contener solo los datos específicos de cada venta y los datos de Clientes, productos o ubicaciones geográficas deberían tener cada una su dimension table. Es cierto que, en caso de ir con prisas, podríamos ahorrarnos este paso, que será engorroso, pero nos será útil a efectos formativos y más útil aún si hacemos un estudio posterior para un panel de mandos en Power BI, donde un modelado adecuado supone una importante mejora en el rendimiento, ahorro de almacenamiento, amén de una mayor limpieza y claridad de los datos para otros análisis.
- Por contra, los datos de la tabla Returns son parte del pedido. Queremos saber si cada pedido ha sido devuelto o no, lo que debería integrarse en la fact table de Orders.
- Por último, la tabla People, de personal de ventas, es irrelevante para nuestro análisis, pues parece que un solo comercial lleva cada región de USA.

## Fases del estudio

1. Fase ETL
    1. Extracción. Sin dificultades, ya que se nos proporciona un archivo Excel que cargaremos con Pandas y trasladaremos a DataFrame.
    2. Transformación. Esta fase será más compleja, dado que modelaremos la base de datos como se ha indicado antes, veremos si existen campos vacios, outliers, limpiezas, etc.
    3. Carga. Modificaremos los DataFrame con los datos ya limpios
2. EDA
    1. Realizaremos un análisis descriptivo del dataset para sacar los valores estadísticos fundamentales.
    2. Trataremos de buscar correlaciones de interés.
    3. Extraeremos los dos productos con mayores ventas globales, que serán los que utilizaremos en el análisis principal.
    4. Agruparemos los datos de pedidos por dias para los productos elegidos, sumando los datos que coincidan en el mismo día (si existe alguna coincidencia).
3. Análisis
    1. Expondremos los datos de ventas del producto más vendido y haremos un análisis de la serie temporal resultante, con datos de ventas por días.
    2. Analizaremos si la serie es o no estacionaria y si presenta algún tipo de patrón de estacionalidad. 
    3. Con ello determinaremos el mejor modelo para realizar un forecast de ventas durante el próximo mes.

NOTA: Se dividirá el dataset en dos conjuntos:

1. Entrenamiento, con toda la fact table, menos los dos últimos meses.
2. Test, con los dos últimos meses de ventas, para comprobar el rendimiento del modelo con datos que éste no había tenido en cuenta en el entrenamiento.
3. El forecast final será, claro está, sin datos conocidos (ventas en enero de 2018).
