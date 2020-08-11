# Segundo cuadro de mando


## Iniciaciòn
Creamos un fichero nuevo y cancelamos eso significa tendremos el fichero vacio
Vamos a script
Datos -> Ficheros planos
Cargamos transacciones
```
LOAD id as [Id], 
     date  as [Fecha], 
     subscription_id as [Suscripciòn], 
     processor_id as [Procesador], 
     processor_account_id as [Cuenta de Procesador], 
     amount as [Cantidad], 
     currency as [Moneda], 
     country as [Pais]
FROM
[xxxxxxx\transactions.csv]
(txt, codepage is 28591, embedded labels, delimiter is ',', msq);


LOAD Subs as [Suscripciòn], 
    cancellation_date as [Fecha de Cancelaciòn]
FROM
[xxxxxxx\subscriptions.csv]
(txt, codepage is 28591, embedded labels, delimiter is ',', msq);
```
Vamos a Visor de tablas y vemos la relacion y la informaciòn que no da.
Observamos que la fecha no es correcta y lo uqe hacemos es usar la funcion:
* Date#(**date**,'MM/DD/YY') para aplicar una mascara
* Data para convertir ese objeto en formato date.

Con lo que el load quedaria algo asi :
```
LOAD id as [Id], 
     Date(Date#(date,'MM/DD/YY')) as [Fecha], 
     subscription_id as [Suscripciòn], 
     processor_id as [Procesador], 
     processor_account_id as [Cuenta de Procesador], 
     replace(amount,'.',',') as [Cantidad], 
     currency as [Moneda], 
     country as [Pais]
FROM
[xxxxxxx\transactions.csv]
(txt, codepage is 28591, embedded labels, delimiter is ',', msq);


LOAD Subs as [Suscripciòn], 
     Date(Date#(cancellation_date,'MM/DD/YY')) as [Fecha de Cancelaciòn]
FROM
[xxxxxxx\subscriptions.csv]
(txt, codepage is 28591, embedded labels, delimiter is ',', msq);
```

## Numero de subscripciones activas
Creamos un Grafico de tipo Tabla simple
Ponemos como dimensiones **subscripciones y Fecha de cancelación**
Añadimos esta expresion **=Sum(if(IsNull([Fecha de Cancelaciòn]),'1','0'))** y ponemos la etiquete subscripciones activas.

## Numero de transacciones por dia y cantidad
Creamos una grafica de lineas 
Dimensiones Fecha y como campo el *Id* y como queremos sacar la cantidad lo que vamos hacer es un **count** o **count(distinct)** añadimos la etiquete de **transacciones**
Boton derecho -> pestaña ejes -> Primera dimension checkbox continuo y mostrar rejilla.
Quitar titulo
Añadimos otra expresion **=sum(Cantidad)** com etiquete cantidad, aqui vemos un problema con el formato de cantidad, para ello tenemos que cambiar el LOAD (**replace(amount,'.',',') as [Cantidad**)
Como vemos que se ve mal vamos hacer un cambio
A un grafico combinado y cambiamos la cantidad a barras.

### Set Analysis
Clonamos dos veces  (teniendo tres en total).
Cambiamos las todas las matricas fijandolas a las distintas monedas :
* € Count({<[Moneda] = {'EUR'}>}DISTINCT Id) y sum({<[Moneda] = {'EUR'}>}Cantidad)
* $ Count({<[Moneda] = {'USD'}>}DISTINCT Id) y sum({<[Moneda] = {'USD'}>}Cantidad)
* £ Count({<[Moneda] = {'GBP'}>}DISTINCT Id) y sum({<[Moneda] = {'GBP'}>}Cantidad)
Una vez que tengamos esto lo que hacemos es crear un grupo y metemos esos tres graficos dentro.


### Velocimetro
Creamos un nuevo grafico  -> indicador -> subscripciones activas
propiedades -> estilos
quitamos el titulo
Presentacion y ponemos 30.000 en el apartado configuraciones de indicador.
Mostrar Etiqueteas de cada -> 1 unidad
Ahora vamos a cambiar el formato SHFT+CONTROL
Añadimos texto dentro del grafico subscripciones activas
En propiedades vamos a Colores -> fondos-> transparente.
Diseño -> ancho de borde = 0

## Numero de cancelaciones por dia
Clonamos el grafico y borramos las transacciones
Etiqueta cancelaciones **Count([Fecha de Cancelaciòn])**
Cambiamos el titulo y el eje
Añadimos las cancelaciones y ponemos titulo de Cancelaciones por dia.
Creamos un grupo con Moneda,Pais , procesador y Fecha
Quitamos Fecha y ponemos el grupo
Clonamos Ventas  USD y lo pasamos a tabla simple y añadimos transacciones a numero con dos decimales.
Cambiamos el titulo de las exprsiones con el simbolo de cada moneda.
Metemos todo esto en un contenedor


### Resultado Final 
![Resultado final](https://github.com/ravamo/qlikview_Iniciacion/tree/doc/SegundoCuadroMando/blob/doc/assert/CuadroMando2.png?raw=true)