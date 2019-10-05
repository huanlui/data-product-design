# Data Product Design

Un proyecto de Data Science no siempre tiene que acabar en un nodelo de predicción. 

GDPR: No podemos poner DNI, Nombres, Apellidos, Tarjeta de crédito ni tampoco que haya conjunto de columnas que me lleven unívocamente a una persona (tiene 25 años, vive en tal calle y es de Ucrania). Esto último se mitiga agrupando (tiene entre 20-30 años, vive en el barrio de Tetuán, ...

Usar principios Agile, como por ejemplo:
- Mínimo Producto Viable. 
- Tener claro que vamos a tener que ir y volver. 
- Checkpoints e hitos intermedios: por ejemplo, primero un OverView de mis datos, después limpiarlo, después sacar nuevas variables que dependen de anteriores. 
- Poner fechas parano relajarse. 
- Estar preparado para ir para atrás si llegamos a algo que no tiene sentido. 
- Tener claro que en obtener/generar y limpiar los datos va a ser costoso. Tenerlo planificado. 

## Fases

1. Data Cleaning. Obtener, quitar NAs, fechas, etc. 

![](https://miro.medium.com/max/736/1*hTBA7atA6VtSLgbpJWmO9g.jpeg)

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

## Importancia del front-end, de presentar los datos

Una buena **visualización** se va a vender genial. Una página web bien montada. Pero ojo, no olvidar el mínimo producto viable. 

Hay científicos de datos muy buenos pero que después no son capaces de explicar o de sacarle un **valor real**. 

Se capaz de resaltar los **puntos fuertes** del proyecto y **qué importancia de negocio** tiene. 

Ya no sólo lo bonito que quede, sino, aún más importante, decir qué valor aportas. Por ejenplo, si eres capaz de extraer datos de muchos sutios distintos y al final tu resultado es un dataset, eso de por sí ya tiene mucho valor. Incluso más que el modelo posterior, que al final (simplificand mucho) es sólo aplicarlo. Lo importante es, a la hora de presentar tu dataset, es decir *por qué tiene ese valor*

# Proyecto

Compañía del mercado de viaje. Tiene muchos acceso a datos (tickets de viaje, inventarios de aerolíneas, etc.)

Matched Shopping: tenemos por un dato una serie de búsquedas y por otro lado una serie de reservas. Hay que hacer un match para saber qué búsquedas acaban en reserva. Los datos que nos da ya están matcheados. Para cada búsqueda tenemos los resultados que salieron (recomendaciones) y de esas cuáles fueron reservadas. 

Con esto, las aerolíneas podrían ver las preferencias de los clientes. Por ejemplo, si influye más el precio que la compañía o que el número de escalas. 

* Cada búsqueda tendrá una serie de features. 
* Cada recomendación (resultado de búsqueda) tendrá una serie de features.
* Cada reserva tiene una serie de features.
* Cada vuelo tiene una serie de features. 

## Problemas en el mundo real

* Parser en python y sin diccionario. Se decidió al final en Spark (que es con Scala). Después está pyspark , un FW para trabajar en Python (más Frankestein).
* Los datos eran complejos y encima anidados
* Los datos estaban sucios (filas duplicadas tal cual, ids duplicados, etc.)
* No estaban claro como se estaba haciendo el matching. 
* Proyecto nuevo sin una ruta clara, necesitamos flexibilidad. EL problema es que tener flexibilidad con Big Data es más complicado. Si con cada cambio tengo que compilar, entrenar, pasarlo al cluster, etc. Para solucionar eso tienen Zepelling, que es un notebook. Con el notebook puedes hacer pruebas más rápidas, en lugar de hacer algo ya cerrado. Es decir, **usar un notebook para primeras fases y después ya lo pondrás bonito para _producción_**
* Puede que haya tantos datos que no me quepan todos en memoria. Tengo que ser capaz de segmentar datos. Por ejemplo, puedo crear una _clave_, que puede ser compuesta (ejemplo: origen_destino) y enviar cada clave a un nodo diferente. Así, un nodo sólo gestionaría los MAD-BCN, otros los CUENCA-ALBACETE, etc. 

## Featura extraction

Sacar variables a partir de otras. Ejemplo muy simple: tengo metros del piso y precio, pues saco €/m2. 

## Data cleaning & preparation process. 

* Parseo.
* Eliminación de duplicados.
* Eliminación de claves duplicadas (ids).
* Eliminar datos erróneos, incoherencias (los puede haber, es más normal de lo esperado). Por ejemplo, había viajes disfrazados de one-way que eran en realidad round-way. 
* Hacer joins con otros datasets para obtener más datos. 
* Después de modelar, se puede hacer limpieza adicional. 
* Descartar datos. Por ejeplo, me vienen datos en € y en $. Pues me quedo sólo con € y así simplifico el problema. Pero esto (pensando en lo que me dijo Henry) supone perder datos, con lo caros que están. Podría ser otra iteración posterior. 

### Spark

Se ejecuta en un clúster de forma distribuida. 

![cluster](https://d2h0cx97tjks2p.cloudfront.net/blogs/wp-content/uploads/sites/2/2017/07/install-apache-spark-on-multi-node-cluster.jpg)

Problemas en sistemas distribuidos:
* Coordinación (estamos paralelizando, y hay que hacer que un)
* Atomización de operaciones. 
* Resilencia: tenemos que estar preparados para que un nodo se caiga.

Spark sse encarga de distribuit el trabajo entre cada nodo. 

### Función utilidad 

Función utilidad: es lo _útil_ que para un usuario es una determinada observación (file, por ejemplo, unaf fila de vuelo). 
Utilidad = baseline + beta_0 * variable_1 + beta_2 * variable_2

Si utilidad > 0, consideramos que el usuario considera útil esa observación (ese oferta,por ejemplo). 

baseline es un valor concreto de tuplas (variable_1,variable_2) que consideraremos nosotros que es la utilidad cero. 

Ver:
* Sobre todo,la presentación de este repo. 
* https://es.wikipedia.org/wiki/Utilidad_(econom%C3%ADa)
* https://towardsdatascience.com/12-things-i-learned-during-my-first-year-as-a-machine-learning-engineer-2991573a9195


### Ideas clave para el TFM

Subir tambien la presentacion inical para que la evaluen. 

#### Memoria

Memoria concisa, pero que:

* Cuente la historia de cómo lo hiciste , tu idea inicial por qué tuviste que cambiarlo 
* Dé la instrucciones claras para instalar los paquetes necesarios. Que sea facilísimo. Cómodo usar un virtual env incluyendo el fichero de configuración en el que se guardan las dependencias. Si usamos R podemos usar engtorno virtual de conde. Si es python, directamente los de python. 
* Documento pragmatico: no hay necesidad de meter literatura, incluso lo pueden ver mal.
* Metodología: es importante explicar por qué hemos elegido un modelo. Ese criterio es el núcleo de lo que van a evaluar. 
* Tablas y gráficos es importante y los detalles los peudes poner aparte, puedes poner gráficos . En la memoria puede ser útil poner alguno para que se hagan a la idea. 
* Conclusión: decir que cuál es la consecuencia, la interpretación. No confundir 
* Si haces un front-end, pequeño manual de uso del front-end. O que sea super intuitivo, aunque nunca sobra el manual. 

##### Tecnología

Cualquier tecnología. Si tiene licencia, ok, pero le tenemos que conseguir licencia a los dos evaluadores (Dani e Isra).

DEsde el 21 de octubre, van a estar monitorizando el repo. No quieren un solo commit al final. Incluir el documento y facilitarle la vida mucho. 

#### Presentación

También debe ser muy pragmática. Hay que venir a la hora a la que se nos convoque para hacer la presentación, no es necesario estar con todos. 

#### Repo

REferencias y recomendaciones en Linkedin se pueden dar si el proyecto es bueno. Por eso el repo debería estar público (en su opinión) y bioen hecho. 

Un repo normal es uno en el que sólo hay un commit. 

Incluir un buen Readme. 

Si lo hacemos en inglés no vamos a tener mejor nota que si es en español. Si hay errores en inglés, no pasa nada. PEro recomendado en inglés para que sea más público. 
Necesario que haya un red

#### Evaluation criteria (seis puntos, cada uno cuenta 1/6)

* Clarity in the document, easiness to replicate work
* Complexity ( In the data and the analysis methodologies. More complex is better)
* Clarity and correctness of source code
* Relevance and fit-to-purpose of the chosen analytical methods
* Relevance and fit-to-purpose of the chosen technologies
* UX and usability of the frontend

Si finalmente el modelo no da unos resultados buenísimos, podría también ser un proyeto de 10 si eres capaz de interpresetar por qué, etc.

Kaggle es un sitio legítimo para extraer datos o ideas. Puedes hacer eso y sacar un 10. Sí que a veces suele ser un indicador de que la persona no se está preocupando mucho por el tema y al final sale, dejando de cumplir alguno de los criterios de evalución. 
