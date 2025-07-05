# S9---SHOWZ
Ayudar a optimizar los gastos de marketing para Showz, una empresa de venta de entradas de eventos. Análisis por cohortes, cálculos de KPIs (Metricas del negocio) y visualización de heatmaps (mapas de calor).

## Descripción

Ayudar a optimizar los gastos de marketing para Showz, una empresa de venta de entradas de eventos.

Para ello se requieren obtener la siguiente información:
* Sobre las Visitas:
* * ¿Cuántas personas lo usan cada día, semana y mes?
  * ¿Cuántas sesiones hay por día? (Un usuario puede tener más de una sesión).
  * ¿Cuál es la duración de cada sesión?
  * ¿Con qué frecuencia los usuarios regresan?
* Ventas:
* * ¿Cuándo empieza la gente a comprar? ( el tiempo (días) que transcurre entre el registro y la conversión, es decir, cuando el usuario se convierte en cliente.)
  * ¿Cuántos pedidos hacen durante un período de tiempo dado?
  * ¿Cuál es el tamaño promedio de compra?
  * ¿Cuánto dinero traen? (LTV)
* Marketing:
* * ¿Cuánto dinero se gastó?  (Total/por fuente de adquisición/a lo largo del tiempo)
  *  ¿Cuál fue el costo de adquisición de clientes de cada una de las fuentes?
  *  ¿Cuán rentables eran las inversiones? (ROMI)


Se cuanta con los registros del servidor con datos sobre las visitas a Showz desde enero de 2017 hasta diciembre de 2018;
un archivo con los pedidos en este periodo;
estadísticas de gastos de marketing.

## Herramientas utilizadas
![Python](https://img.shields.io/badge/:Python-024A86?style=for-the-badge&logo=python&logoColor=white&labelColor=101010)</br>
![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/seaborn-%233F4F75.svg?style=for-the-badge&logo=seaborn&logoColor=white)
![Datetime](https://img.shields.io/badge/datetime-%233F4F75.svg?style=for-the-badge&logo=datetime&logoColor=white)

![Colab](https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252)
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)

## Conclusiones 

* Sobre las Visitas:
* * 23,228 usuarios únicos mensuales, 5,716 semanales y 907 diarios.
  * 1.08 sesiones por usuario al día.
  * La mayoría de las sesiones duran menos de 1 hora.
  * Mediante el análisis de cohortes se encontró que, en general, la mayor tasa de retención en todos los cohortes es el tercer mes de actividad.
* Ventas:
* * Se descubre que la cantidad de tiempo que trancurre para la conversión de los usuarios esta ligado a estrategia de marketing que los atrajo, el 4,3,2 y 5 fueron los canales que trajeron mayor conversión desde el día cero. 
  * Se crea una función para obtener las compras en un periodo dado, en todo el periodo registrado fueron 28,001 compras generadas.
  * Al hacer el análisis por cohortes, observamos que el primer mes de actividad es cuando mayor compras se realizan por usuario (de 2 a 6 compras por usuario), los meses posteriores sus la cantidad de compras por usuaruaio son de 0,1 y hasta 2 compras. 
  * Con el calculo de LTV (valor de vida del cliente), vemos que hay furtes ingresos para el primer mes de actividad de los usuarios.
* Marketing
* * Se genera una tabla con los gastos mensuales por tipo de canal de marketing, siendo el de mayor costo el 3.
  *  Al calcular el  CAC (costo de adquisición de comprador),  observamos que los canales 9 y 10 son los de menor costo de adquisión por comprador.
  *  Con Retorno de la Inversión en Marketing ROMI , observamos que los canales 10 y 9 son los más rentables.
 
Consejos generales para el equipo de marketing:
* Analisando los datos recabados, se recomienda las fuentes de "4", "5", "2".
* Si bien buscamos que el ROMI ( la relacion entre las ganacias y los gastos) sea alto, este parametro nos llevaría a escoger los medios "9" y "10", sin embargo, revisando la tasa de conversión observamos que son los medios que menos compradores generan.
* Por el contrario, si nos basamos en la tasa de conversion, nos llevaría a escoger el la fuente "3", sin embargo comparando los gastos que se generan por este medio son muchomás elevados que los demás.
* En los medios "4", "5", "2", se encuentra un balance, por otra parte el "7" no esta generando conversiones.

## Profundización técnica
* Importación de datasets, exploración de datos, correción de nombres de columnas, adecuación de tipos de datos
* Manejo de datos tipo datetime - df['col']=pd.to_datetime(df['col'],format='%Y-%m-%d %H:%M') / df['col']=df['col'].astype('datetime64[M]') , df['col'].dt.year/month/day
* Cálculo de DAU,WAU y MAU - Media de usuarios únicos activos diarios/semanales/mensuales.
* Cálculo de la moda para una serie- .mode()
* Cálculo de cohortes -
* * Primera fecha de actividad de cada usuario -  df['first_activity_date'] = visits.groupby(['uid'])['start_date'].min()
  * Cálculo ciclo de vida - df['cohort_lifetime'] = (visits['activity_month'] - visits['first_activity_month']) / np.timedelta64(1, 'M')
  * Conteo usuarios únicos por mes - cohorts ['uid'] = (visits.groupby(['first_activity_month','cohort_lifetime']).agg({'uid':'nunique'}).reset_index())
  * Cálculo usurios iniciales de cada cohorte - df['cohort_users'] = cohorts[cohorts['cohort_lifetime'] == 0][['first_activity_month', 'uid']]
  * Cálculo de retención de usurios - cohorts['retention'] = cohorts ['uid'] / cohorts ['cohort_users']
  * Tabla pivote - retention_pivot = cohorts.pivot_table(index ='first_activity_month',  columns = 'cohort_lifetime', values = 'retention', aggfunc= 'sum')
  * Creación heatmap - plt.figure(figsize=(,)),plt.title(''), sns.heatmap( retention_pivot, annot=True, fmt='.4f',linewidths=1)
* Unión de tablas/series - df.join(s,on='col', rsuffix=''), df.merge(s,on='')
* Cálculo del valor de vida del cliente -  LTV = (Ingresos * Margen de beneficio)/ cantidad de usuarios
* Cálculo de Costo de Adquisición de compradores - CAC = Costo canal marketing / # compradores
* Cálculo de Retorno de la Inversión en Marketing - ROMI= LTV (ganacias totales) / CAC (costo de adquisición)
