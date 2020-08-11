# Primer cuadro de mando


## Iniciaciòn
Crear un documento.
Selecionamos la excel (Sales_data).
Desde el asistente guardamos todo el documento.
Elejimos el grafico , nos va a dar a elejor entre los mas comunes en nuestro caso escojemos barras.
En la seccion de datos escogemos la dimensiones y las expresiones:

1.1 SalesChanneles (en dimensiones)

1.2 UnitSould (en expresiones)


## Cargamos los datos
Abrimos el script.
Ponemos alias a las columenas.
```
LOAD custId  as [Id], 
     custName as [Nombre] , 
     custCountry as [Pais], 
     productSold as [Producto], 
     salesChannel as [Canal], 
     unitsSold as [Unidades Vendidas], 
     dateSold as [Fecha],
     month(dateSold) as [Mes],
     Year(dateSold) as [Año]
FROM
[xxxxxxx\Sales_Data.xls]
(biff, embedded labels);
```
Recargamos los datos y guardamos .

## Personalizamos el primer grafico
Vamos al grafico que hemos generado con la guiado
Boton derecho -> propiedades
General -> Titulo de Ventana y ponemos **Unidades Vendidas**
Mostrar titulo de grafico **Unidades Vendidas**
Titulo y deseleccionar "Mostrar Titulo"
Vamos a Numeros y cambiamos el formato

## Añadimos Grafico/tabla
Clonamos el grafico actual.
Boton derecho y cambiamos las dimensones quitando *canal* y ponemos pais.
Cambiamos el formato a tabla simple
Añadimos en expresiones la etiqueta **Unidades Vendidas**

## Añadimos Grafico/tabla II
Clonamos el mismo grafico de antes y ponemos el campo **Fechas**.
Vamos a Editar y añadimos 
* Month(Fecha)
* Year(Fecha)
Propiedades -> Lineas
Ordenar -> Mes -> Valor numerico ASC y desactivamos el Valor y.
Añadimos las etiqueteas en las dimensiones de 
* Mes 
* Año.

## Modificacion Grafico/tabla II
Vamos a cambiar las expresiones por campos de nuestro **LOAD**
* Month(Fecha) as Mes,
* Year(Fecha) as Año
Luego vamos a el grafico y cambiamos las dimensiones por las nuevas.

## Set Analys

Creamos un grafico nuevo de tipo tabla siemple
Vamos a las dimensiones y creamos un grupo de tipo cliclico con:
* Año
* Canal
* Pais
* Producto
Cambiamos la expresion `SUM({Unidades Vendidas})` a  `SUM({<[Año]={2011}>}{Unidades Vendidas}) `  repetimos lo mismo para el 2012


### Resultado Final 
![Resultado final](https://github.com/ravamo/qlikview_Iniciacion/blob/doc/PrimerCuadroMando/assert/CuadroMando1.png?raw=true)
