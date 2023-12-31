# DataScienceCoderhouse

## Comisión: 42365

### Docente: Rubén Darío González

### Tutor: Pedro Miguel Pérez

<p align="center">
  <img src="Otros/CODERLOGO.png" width="400" alt="logo"/>
</p>

Este repositorio fue creado con el fin de almacenar todas las entregas de Federico Massini, para el curso de Data Science 2023 dictado por CoderHouse.

Link a presentación ejecutiva **[aquí](https://www.canva.com/design/DAFunxBYZVA/T_8mOwGeap-Ib5fWWcb-tQ/edit?utm_content=DAFunxBYZVA&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)**.

---

## 📋 Tabla de contenido

1. 🐺 [Abstract](#abstract)
2. 🃏 [Data acquisition](#data-acquisition)
3. 🔨 [Data wrangling](#data-wrangling)
4. 🚀 [Exploratory data analysis](#exploratory-data-analysis)
5. 🐳 [Predicción de packaging de cajas (usando modelos sup. de ML)](#modelos1)
6. 💯 [Clasificación de bases (usando modelos no sup. de ML)](#modelos2)
7. 🌿 [Conexión a APIs de interés](#conexión-a-apis)
8. ☑️ [Predicción de tipo de pallet (usando modelos sup. de ML)](#modelos3)
9. 📈 [Final conclusion](#final-conclusion)

## <a name="abstract">🐺 Abstract</a>

La empresa Bosch es conocida por fabricar productos diversos para todo el mundo. Recientemente comenzó a producir un nuevo producto, el cual cuenta con 5 versiones distintas.

Este producto tiene la característica de producirse de una manera diferente a lo que la empresa está acostumbrada, por ello se han desarrollado nuevos procedimientos para llevar adelante la producción.

Estos procedimientos al ser nuevos, lamentablemente distan bastante del ideal; hace falta mucho estudio y trabajo para volverlos más eficientes.

De ahí nace la necesidad de este estudio, con la información que nos proporciona el desarrollador del producto, el sector logístico deberá buscar la manera de optimizar sus procesos, para poder cumplir con los plazos de abastecimiento a planta.

De ninguna manera puede suceder que la línea productiva pare por falta de piezas a la hora de montar, logística debe hacer lo indispensable para evitar este panorama. Y a su vez, hacerlo de la manera más eficiente posible (en cuanto a tiempos y costos).

<p align="center">
  <img src="Otros/LOGO.jpeg" width="400" alt="logo"/>
</p>

El siguiente dataset contiene información sobre las piezas con su respectivo código (referencia) necesarias para construir distintas versiones de un producto en específico. Estas versiones difieren ya que algunas son más completas que otras, o tienen diferentes funciones, y por ende llevan distintas piezas en su ensamblado.

La mercadería viene en cajones llamados "bases" dentro de contenedores. A su vez, cada caja dentro de estas bases tiene dentro un número determinado de piezas (indicado en el dataset). En resumen, las piezas vienen en cajas, las cuales vienen en bases, las cuales a su vez vienen en contenedores. Toda esta información está detallada en el dataset, donde se indica incluso dimensiones, pesos y tipo de packaging de las bases o cajas.

Cada fila además aclara a que versión del producto pertenece la pieza, hay piezas comunes a todos y otras que no lo son. Una vez traidos los contenedores, las piezas deberán separarse en clases. La clase de una pieza determina de qué forma se ensambla, por lo que es necesario clasificar todo antes de comenzar a ensamblar el producto.

Algunos puntos interesantes a ver en este estudio:

1. Como se distribuyen las referencias entre las clases? Hay alguna que contenga más variedad?
2. Para contar con el total de piezas de una referencia, cuantas cajas se deben abrir aproximadamente?
3. Las bases, cuantas cajas traen? Y referencias? Cómo viene la mercadería distribuída?
4. Cada referencia, en cuantas versiones se usa? Hay muchas comunes a todas?
5. Qué clase conlleva más volumen?
6. Cuáles versiones son más parecidas/diferentes entre si?
7. Existe algún patrón entre las bases? Se pueden clasificar?
8. Se puede preever cual será el tipo de pallet en el cual irá una referencia, solo conociendo información de ella misma?

Por otro lado, en determinadas ocasiones sucede que logística tiene rotura de piezas, las cuales es necesario reponer. Para esto, se hacen solicitudes al proveedor de envíos puntuales, con la mercadería específica que se necesita.

Esta mercadería puede venir en diferentes tipos de packaging, el cual determina las condiciones de almacenamiento a posteriori. De ahí, se abren otros puntos interesantes a analizar:

9. En el caso de que logística necesite pedir un envío especial de piezas a reponer, es posible predecir en qué tipo de packaging lo enviará el proveedor?
10. Qué variables son importantes a la hora de predecir dicho packaging?
11. Es posible deducir qué lógica usa el proveedor para seleccionarlo?
12. Cuántos días demora un envío? Qué tanto varía según cuál sea el país de destino?
13. Qué variable es más importante a la hora de calcular el costo del envío? El peso? El volumen? Otra?
14 .Cual sea la variable de la respuesta anterior, también afecta los días que demora un envío en llegar?

Todos estos puntos indudablemente ayudan a optimizar el proceso de ingreso de la mercadería, tanto para lo que es inspección como para almacenamiento y traslado. Es crítico que todas las piezas se encuentren en el lugar correcto a la hora de producir, y lo que buscamos es volver más eficiente el camino a dicho objetivo.

Dataset original **[aquí](https://docs.google.com/spreadsheets/d/1PeO17z4eE4ftrbbM4lVSYoR5cN5qbC2c/edit?usp=sharing&ouid=111044325679429769254&rtpof=true&sd=true)**.

## <a name="data-acquisition">🃏 Data acquisition</a>

### Diccionario de columnas del DS

| COLUMNA |	DESCRIPCIÓN |	EJEMPLO 1 |	EJEMPLO 2 |
|:-------------------:|:-------------------:|:-------------------:|:-------------------:|
| CLASE |	Sector en el cual se ensambla esta pieza | F1 |	A |
| VERSION |	Versión del producto al cual pertenece esta pieza |	M42 |	A42 |
| CONTENEDOR |	Contenedor en el cual viene esta pieza |	CMAU4908460 |	MRSU3285393 |
| BASE |	Base en la cual viene esta pieza |	AF0001 |	MP001 |
| TIPO BASE |	Tipo de base |	IRON FRAME |	IRON FRAME |
| LARGO BASE |	Largo de la base en mm |	2110 |	2250 |
| ANCHO BASE |	Ancho de la base en mm |	600 |	1700 |
| ALTURA BASE |	Altura de la base en mm |	1200 |	1050 |
| PESO BASE |	Peso de la base en kg |	205 |	861.0 |
| CAJA |	Caja en la cual viene esta pieza |	AF0001 |	MC0072 |
| TIPO CAJA |	Tipo de caja |	IRON FRAME |	CARTON |
| LARGO CAJA |	Largo de la caja en mm |	1700 |	800 |
| ANCHO CAJA |	Ancho de la caja en mm |	100 |	500 |
| ALTURA CAJA |	Altura de la caja en mm |	130 |	530 |
| PESO CAJA |	Peso de la caja en kg |	153.92 |	13.85 |
| REFERENCIA |	Código de esta pieza |	2804110-BU01 |	6900006P201D |
| UNID/CAJA |	Cantidad de esta pieza que viene en la caja |	16 |	16 |

### Diagrama de la operativa

<p align="center">
  <img src="Otros/Flujo.jpg" width="600" alt="logo"/>
</p>

## <a name="data-wrangling">🔨 Data wrangling</a>

Como se explicaba en la introducción, para comenzar a producir es necesario clasificar las piezas en clases. Para esto se debe inspeccionar la mercadería que ingresa, aunque no es necesario hacerlo para todas las bases.

Si las referencias que vienen en la base, pertenecen a distintas clases, se deberá inspeccionar y clasificar, de lo contrario no hará falta, y la base se abatecerá directamente a la clase correspondiente.

```py
DSNX['CLASES/BASE'] = DSNX.groupby(['BASE', 'VERSION'])['CLASE'].transform('nunique') #Genero la columna CLASES/BASE
DSNX['INSPECCIÓN'] = np.where(DSNX['CLASES/BASE']>1, 1, 0) #Si la base tiene referencias pertenecientes a más de una clase, la inspecciono
```

Además de esta, se realizaron diversas transformaciones más, quedando el dataset para trabajar de la siguiente manera (primeras 5 filas):

| CLASE |	VERSION |	CONTENEDOR |	BASE |	TIPO BASE |	LARGO BASE |	ANCHO BASE |	ALTURA BASE |	PESO BASE |	CAJA |	... |	UNID/CAJA |	CAJAS/BASE |	REFS/BASE |	CLASES/BASE |	INSPECCIÓN |	REFS/CAJA |	VOLUMEN CAJA |	VOLUMEN CAJA/REF	| VOL TOTAL |	TIPO PALLET |
|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|:-------------------:|
| F1 |	M42 |	CMAU4908460 |	AF0001 |	IRON FRAME |	2110 |	600 |	1200 |	205.0 |	AF0001 |	... |	16 |	1 |	1 |	1 |	0 |	1 |	0.022100 |	0.022100 |	0.132600 |	DIRECTO |
| F1 |	M42 |	CMAU4908460 |	AF0002 |	IRON FRAME |	2110 |	600 |	1200 |	205.0 |	AF0002 |	... |	16 |	1 |	1 |	1 |	0 |	1 |	0.022100 |	0.022100 |	0.132600 |	DIRECTO |
| F1 |	M42 |	CMAU4908460 |	AF0003 |	IRON FRAME |	2110 |	600 |	1200 |	205.0 |	AF0003 |	... |	16 |	1 |	1 |	1 |	0 |	1 |	0.022100 |	0.022100 | 	0.132600 |	DIRECTO |
| F1 |	M42 |	CMAU4908460 |	AF0004 |	IRON FRAME |	2110 |	600 |	1200 |	205.0 |	AF0004 |	... |	16 |	1 |	1 |	1 |	0 |	1 |	0.022100 |	0.022100 |	0.132600 |	DIRECTO |
| F1 |	M42 |	CMAU4908460 |	AF0005 |	IRON FRAME |	2110 |	600 |	1200 |	205.0 |	AF0005 |	... |	16 |	1 |	1 |	1 |	0 |	1 |	0.022100 |	0.022100 |	0.132600 |	DIRECTO |

## <a name="exploratory-data-analysis">🚀 Exploratory data analysis</a>

Se realizaron varios gráficos utilizando Seaborn y Matplotlib, algunos ejemplos son:

### Matplotlib

```py
labels = venn.get_labels([A41, A42, M21, M41, M42], fill=['number'])
fig, ax = venn.venn5(labels, names=['A41', 'A42', 'M21', 'M41', 'M42'])
ax.set_title('DIAGRAMA DE VENN PARA COINCIDENCIA DE REFERENCIAS ENTRE VERSIONES')
```

<p align="center">
  <img src="Otros/1.png" width="1000" alt="logo"/>
</p>

Se generan 5 conjuntos, cada uno correspondiente a una versión. Lo que se ilustra en este gráfico es la cantidad de referencias que coinciden entre estas versiones.

Como se puede ver, tenemos 680 referencias comunes a las 5 versiones (71%), y luego distintas cantidades para cada combinación posible.

Esto sucede porque el producto tiene 3 funcionalidades (X-Y-Z):

X. Puede ser A o M.

Y. Puede ser 4 o 2.

Z. Puede ser 2 o 1.

Por ejemplo, la zona A42/M42 contiene 103. Esto significa que hay 103 referencias que solo utilizan estas dos versiones (por ser las únicas dos con las funcionalidades Y=4 y Z=2 a la vez).

Por otro lado, la zona A41/A42 contiene 29. En este caso hay 29 referencias que solo utilizan estas dos versiones (por ser las únicas dos con las funcionalidades X=A y Y=4) a la vez.

Los modelos A42/M21 no tienen nada en común, no comparten referencias en ningún cruce, salvo en el central que son las referencias comunes a todas las versiones. Profundizaremos en estos temas más adelante.

### Seaborn

```py
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(16, 12))
sns.barplot(data=DSNX_5, x=DSNX_5.index, y='DEDICADO', color='blue', label='DEDICADO', alpha=0.7, ax=ax1)
sns.barplot(data=DSNX_5, x=DSNX_5.index, y='DIRECTO', color='green', label='DIRECTO', alpha=0.7, bottom=DSNX_5['DEDICADO'], ax=ax1)
sns.barplot(data=DSNX_5, x=DSNX_5.index, y='MIXTO', color='orange', label='MIXTO', alpha=0.7, bottom=DSNX_5['DEDICADO'] + DSNX_5['DIRECTO'], ax=ax1)

sns.barplot(data=DSNX_5, x=DSNX_5.index, y='DEDICADO', color='blue', label='DEDICADO', alpha=0.7, ax=ax2)
sns.barplot(data=DSNX_5, x=DSNX_5.index, y='MIXTO', color='orange', label='MIXTO', alpha=0.7, bottom=DSNX_5['DEDICADO'], ax=ax2)

ax1.set_title('Suma total de pallets por clase y tipo de pallet (promedio por versión)')
ax1.set_xlabel('Clase')
ax1.set_ylabel('Suma pallets')
ax1.legend()

ax2.set_title('Mismo gráfico pero quitando los directos')
ax2.set_xlabel('Clase')
ax2.set_ylabel('Suma pallets')
ax2.legend()
```
<p align="center">
  <img src="Otros/2.png" width="1000" alt="logo"/>
</p>

Las clases "S" y "CH1" conllevan gran cantidad de pallets directos, esto sucede porque suelen estar compuestos por referencias que conllevan gran volumen, son piezas estructurales del producto.

Luego repetimos el mismo gráfico pero quitando los directos, para que se aprecie mejor la distribución entre dedicados y mixtos. Ya en este gráfico se empieza a dejar ver cuales son las clases que requerirán más espacio de almacenamiento.

## <a name="modelos1">🐳 Predicción de packaging de cajas (usando modelos sup. de ML</a>

En esta etapa del proyecto analizaremos el packaging de las cajas.

Muchas veces sucede que, debido a roturas, logística tenga que pedir puntualmente al proveedor cajas con piezas para realizar reemplazos.

En virtud de esto, es de interés saber si es posible predecir que tipo de packaging enviará el proveedor, en función del contenido que tendrá la caja (o sea en función del pedido de piezas a reponer).

Tener esta información ayudaría a preparar de manera anticipada, las condiciones de almacenamiento necesarias para dicho arribo de producto.

A su vez, poder deducir el criterio que usa el proveedor para seleccionar el packaging, podría ayudar a dar recomendaciones al mismo de cómo mejorarlo.

Se realizó el split de los datos, para entrenar, validar y testear:

```py
x_trainval, x_test, y_trainval, y_test = train_test_split(PCA_DSNX_7,DSNX_7.iloc[:,6], test_size=0.30, random_state=42, stratify=DSNX_7.iloc[:,6]) #Divido DS en trainval y test
x_train, x_val, y_train, y_val = train_test_split(x_trainval,y_trainval, test_size=0.30, random_state=42, stratify=y_trainval) #Divido el trainval en val y train
```
Y se entrenaron modelos, Decision Tree:

```py
modelo_arbol = DecisionTreeClassifier() #Entreno
modelo_arbol.fit(x_train,y_train)
y_predict = modelo_arbol.predict(x_val) #Valido
```
Random Forest Classifier:

```py
modelo_arbol2 = RandomForestClassifier() #Entreno
modelo_arbol2.fit(x_train,y_train)
y_predict2 = modelo_arbol2.predict(x_val) #Valido
```

Obteniendo muy buenas métricas:

| Métricas | Decision Tree VAL |	Random Forest VAL |	Random Forest TEST |
|:-------------------:|:-------------------:|:-------------------:|:-------------------:|
| Precision |	0.996079 |	0.996012 |	0.945155 |
| Recall |	0.972899 |	0.971946 |	0.997515 |
| Accuracy |	0.998496 |	0.998281 |	0.998346 |
| F1 | 0.983651 | 0.983145 | 0.967755 | 

## <a name="modelos2">💯 Clasificación de bases (usando modelos no sup. de ML</a>

A continuación, intentaremos abordar otra de las preguntas que nos hicimos al inicio, existe algún patrón entre las bases?

Para esto utilizaremos modelos de machine learning no supervisados.

```py
kmeans_model = KMeans(n_clusters=k, max_iter=1000, random_state=42, init='k-means++') #Defino el modelo
kmeans_model.fit_predict(X) #Entreno y predigo
```

Con KMeans se obtuvo 3 clusters bien definidos, también utilizando el Score de Silhouette.

<p align="center">
  <img src="Otros/5.png" width="1000" alt="logo"/>
</p>

Como se puede ver, el número de clusters recomendado es 3.

<p align="center">
  <img src="Otros/3.png" width="1000" alt="logo"/>
</p>

## <a name="conexión-a-apis">🌿 Conexión a APIs de interés</a>

Ya contamos con un modelo predictivo que nos permite estimar el tipo de caja en función del pedido que tengamos que hacer al proveedor, por lo que ahora indagaremos un poco en los costos.

Es de interés poder conocer aproximadamente qué costo tendrá el envío de la caja de reposición que se necesita, y para esto utilizaremos una API pública del proveedor Freightos.

Dicha API necesita que le digamos qué es lo que vamos a enviar, cuanto pesa, sus dimensiones, la cantidad y desde/hacia donde se dirige el envío. Una vez contemos con toda esta información, la misma se debe representar dentro de un URL con el formato correcto, y así se podrá hacer la consulta.

API **[aquí](https://ship.freightos.com/api/shippingCalculator)**.

### Ejemplo:

```py
loadtype = 'boxes' #Enviaremos una caja
weight = 200 #Peso en kg
width = 50 #Ancho en cm
length = 50 #Largo en cm
height = 50 #Altura en cm
origin = 'PVG' #El origen del envío será Shangai, el proveedor nos enviará el pedido desde allí
quantity = 1 #Cantidad a pedir
destination = 'JFK' #El destino puede ser, por ejemplo, Nueva York
url = 'https://ship.freightos.com/api/shippingCalculator?loadtype=' + loadtype + '&weight=' + str(weight) + '&width=' + str(width) + '&length=' + str(length) + '&height=' + str(height) + '&origin=' + origin + '&quantity=' + str(quantity) + '&destination=' + destination
url #Una vez se tienen todos los datos, se combinan como muestra la fila anterior, y así generamos el URL correcto para la consulta
```
Se realizaron consultas como esta de forma masiva, para sacar conclusiones sobre los costos y demoras de envíos para todo tipo de cajas.

<p align="center">
  <img src="Otros/4.png" width="1000" alt="logo"/>
</p>

Estos gráficos nos dan una idea de cómo se comportan los costos y los días de viaje, en función del volumen y el peso de cada caja. A su vez, podemos identificar el tipo de caja por colores.

En cuanto al costo, claramente aumenta a más peso o volumen. Aunque, cómo habíamos previsto en las primeras consultas de la API y en el gráfico anterior, el volumen es mucho más contundente a la hora de definir el costo.

Para los días de viaje, a priori no parece que el peso tenga un gran impacto. El volumen sí marca alguna tendencia, pero también tenemos excepciones.

## <a name="modelos3">☑️ Predicción de tipo de pallet (usando modelos sup. de ML)</a>

Para finalizar el análisis de las preguntas iniciales, se intentará responder si es posible predecir en qué tipo de pallet irá una referencia, solamente conociendo información de ella, y no de las bases/cajas involucradas en el proceso.

Recordemos que se tienen bases inspeccionables y no inspeccionables (dependiendo de la cantidad de clases involucradas en la base). Las referencias que salen de las no inspeccionables, van en pallets directos, ya que todas pertenecen a la misma clase, por ende no hace falta separar. Por otro lado, las demás irán en pallets mixtos o dedicados, dependiendo del volumen total que conlleve cada referencia.

Por ejemplo, si todas las piezas de una referencia llegan a ocupar el volumen de medio pallet, ese pallet tendrá que ser compartido con otras referencias hasta completarse (esto es un pallet mixto). En cambio, si la cantidad de piezas de una referencia es suficiente como para completar un pallet, este será un pallet dedicado.

En resumen, para determinar el tipo de pallet se tienen en cuanta variables como:

Si la base es inspeccionable o no, lo cual depende de la cantidad de clases/base.
El volumen total que ocupan todas las piezas de una referencia, lo cual depende del volumen de cada caja y de la cantidad de piezas dentro de la caja.
Todas estas variables involucran bases y cajas, queremos saber si, teniendo información solamente de la referencia, podemos determinar de antemano en que tipo de pallet irá.

Se realizó el split de trainval y test, y se dividió el dataset en KFolds para hacer CrossVal.

```py
x_trainval, x_test, y_trainval, y_test = train_test_split(DSNX_11.iloc[:,:-1],DSNX_11.iloc[:,-1], test_size=0.30, random_state=42, stratify=DSNX_11.iloc[:,-1]) #Separamos en trainval y test

skf = StratifiedKFold(n_splits=5, random_state=42, shuffle=True) #Haremos CrossVal con 5 splits
splits = skf.get_n_splits(x_trainval, y_trainval)
```

También se generaron pipelines para poder transformar las variables numéricas y categóricas.

```py
categorical_transformer = Pipeline(steps=[('onehot', OneHotEncoder(sparse_output=False, drop='first'))]) #Parametrizo el OHE, dropeo las columnas que no aportan
numeric_transformer = Pipeline(steps=[('scaler', StandardScaler())]) #Parametrizo el SS
```

Finalmente, se entrenaron 5 modelos (con tuneo de hiperparámetros).

| Modelo | Accuracy promedio (VAL) |	Acuraccy final (TEST) |
|:-------------------:|:-------------------:|:-------------------:|
| KNeighborsClassifier | 0.992 | 0.993 |
| LogisticRegression | 0.988 | 0.986 |
| SVM | 0.897 | 0.889 |
| GradientBoostingClassifier | 0.996 | 0.997 |
| MLPClassifier | 0.995 | 0.995 |

Estas son las correspondientes matrices de confusión:

https://github.com/fmassini/DataScienceCoderhouse/assets/145942477/3d4a3d0a-c14c-4f40-a302-a18c2957cd7f

## <a name="final-conclusion">📈 Final conclusion</a>

Quiero aprovechar este momento para expresar mi más sincero agradecimiento a profesores y tutores de Coderhouse.

Su dedicación y apoyo incondicional han sido fundamentales en mi aprendizaje durante todo el año.

Además, las herramientas adquiridas me inspiran a continuar creciendo profesionalmente en el campo de la Ciencia de Datos.

Miro hacia atrás, y me parece increíble la evolución que tuve desde principio de año, que sabía codear de manera muy básica, a ahora que he entrenado modelos y hecho gráficos de todo tipo.

Estoy emocionado por aplicar estos aprendizajes en mi trabajo y seguir mejorando día a día, gracias por todo!!
