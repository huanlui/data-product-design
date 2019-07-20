# Data Product Design

Un proyecto de Data Science no siempre tiene que acabar en un nodelo de predicción. 

GDPR: No podemos poner DNI, Nombres, Apellidos, Tarjeta de crédito ni tampoco que haya conjunto de columnas que me lleven unívocamente a una persona (tiene 25 años, vive en tal calle y es de Ucrania). Esto último se mitiga agrupando (tiene entre 20-30 años, vive en el barrio de Tetuán, ...

Usar principios Agile, como por ejemplo:
- Mínimo Producto Viable. 
- Tener claro que vamos a tener que ir y volver. 
- Checkpooints e hitos intermedios: por ejemplo, primero un OverView de mis datos, después limpiarlo, después sacar nuevas variables que dependen de anteriores. 
- Poner fechas parano relajarse. 
- Estar preparado para ir para atrás si llegamos a algo que no tiene sentido. 
- Tener claro que en obtener/generar y limpiar los datos va a ser costoso. Tenerlo planificado. 

## Fases

1. Data Cleaning. Obtener, quitar NAs, fechas, etc. 
2. Data Processing. Prepararlos para el modelo, extraer variables a partir de otras, etc.
3. Visualización. Tablas, gráficos, etc. para entender un poco los datos. Hay unos notebooks para Spark que se llaman Zepellin. Tiene integrado visualización de de datos que saquemos de alguna query. 
4. Modelado.
5. Evaludación. No siempre puedes tener un conjunto de prueba, a lo mejor puedes sacar la importancia de variables y ver por sentido común si tiene sentido. Por otro lado, estaría bien que integremos la evaluación en producción, es decir, ir testando cómo va ese modelo. 

## Tipos de proyectos:

* Exploratorio (white paper syle). Por ejemplo: quiero ver cómo evolucionan los hábitos deportivos en la sociedad. No tiene modelos, pero requiere saber sacar datos, visualizarlo, sacar conclusiones, etc. Data Science no siempre es ML. 
* Modelado o predictivo. El típico. 
* Clustering / merket segmentation. 
* Deep Learning (All or nothing)
* Recomendadores. 

## Importancia del front-end

Una buena **visualización** se va a vender genial. Una página web bien montada. Pero ojo, no olvidar el mínimo producto viable. 

Hay científicos de datos muy buenos pero que después no son capaces de explicar o de sacarle un **valor real**. 

Se capaz de resaltar los **puntos fuertes** del proyecto. 


