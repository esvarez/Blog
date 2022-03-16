---
title: "Generics Go"
date: 2022-03-15T20:32:27-06:00
draft: true
categories:
    - Tutorial
    - Golang
tags:
    - Tutorial
	- Novedades
    - Go
    - Golang    
    - Go 1.18
cover:
    image: "/img/generics-go/go-gopher.png"
    alt: "go logo"    
    relative: false 
---

El día 15 de marzo fue anunciada oficialmente la nueva version de go 1.18 y con ella las nuevas caracteriticas entre las que destacan, [Fuzzing](../fuzz-testing-go-fuzzing), el uso de workspaces y generics, de esto ultimo hablaremos en este post.

## Generics

Lo primero, es saber que son generics o a que nos referimos cuando hablamos de ellos, como quizas ya sepas, en los lenguajes fuertemente tipados es necesario declarar el tipo de variables que vamos a usar y que nuestros metodos, funciones van a recibir o devolver. Esto nos ayuda a ser mas eficientes con el uso de memoria y con la ejecucion de nuestros sistemas, porque sabemos cuanto es el espacio maximo que podriamos llegar a usar. 

Pero esta misma caracteristica nos complica un poco la vida cuando tenemos mas de un metodo que hace exactamente lo mismo, solo cambiando el tipo de variables que va a devolver. Imaginemos una calculadora en el cual tuvieramos lo siguiente:

```go
func sumarEnteros (num, num2 int) int {
	return num + num2
}

func sumarFlotantes(num, num2 float) float {
	return num + num2
}
```

Como podemos observar tenemos exactamente el mismo código en ambos metodo, la unica diferencia es el tipo de variable que reciben y que regresan. Este es un ejemplo simple y parece poco codigo, pero imagina que implementas una [pila](#Pila) en el cual debemos soportar varios tipos de datos, nuestro código se veria asi.

```go
func insertarEntero (item int) {
	...
}

func retirarEntero () int {
	...
}

func insertarCadena (item string) {
	...
}

func retirarCadena () string {
	...
}

func insertarCaracter (item char) {
	...
}

func retirarCaracter () char {
	...
}
```

Todas las funciones tienen la misma logica, solo cambia el valor que recibimos y retornamos y puede crecer aun mas, quiza necesitamos soportar booleando, flotantes, etc. Y si necesitamos actualizar la logica al momento de insertar es algo que debemos hacer en todos nuestros metodos, si lo vemos asi suena a mucho trabajo, código duplicado y más posibilidades de crear un bug, afortunadamente existen los genericos y llegaron a **go**.

Los genericos declaran el tipo de variable en tiempo de ejecución y no de compilación como lo hace cualquier otra variable en los lenguajes compilados. De esta manera podemos marcar nuestro metodo como generico y definir nuestro tipo de variable una vez que ya fue compilado nuestro código.

## Practica
Es importante mencionar que para poder seguir este post es necesario tener instalada la version 1.18 de go. Si aun no haz instalado go, puedes seguir el siguiente [post](../instalar-go).
### Calculadora basica
Volvamos al ejemplo de la calculadora. Abrimos nuestra terminal y en nuestro *HOME* .
```bash
$ cd ~
```
vamos a crear una carpeta.
```bash
$ mkdir generics
```
Dentro de ella vamos a crear nuestro archivo, `calculadora.go`.

```go {linenos=table,hl_lines=[],linenostart=1}
package main

import (
	"fmt"
)

func main() {
	var num, num2 int32 = 2, 2
	var flo, flo2 float32 = 3.5, 1.2
	fmt.Printf("Usando funciones no genericas %v y %v\n", sumaEnteros(num, num2), sumaFlotantes(flo, flo2))
}
  
func sumaEnteros(num, num2 int32) int32 {
	return num + num2
}
  
func sumaFlotantes(num, num2 float32) float32 {
	return num + num2
}
```

### Implementando generics

Hasta aquí declaramos dos funciones para sumar enteros y flotantes, ahora vamos a usar generics, para con una sola funcion aceptar enteros y flotantes.

```go {linenos=table,hl_lines=[5, "8-10"],linenostart=1}
...

func main() {
	var num, num2 int32 = 2, 2
	var flo, flo2 float32 = 3.5, 1.2
	fmt.Printf("Usando funciones no genericas %v y %v\n", sumaEnteros(num, num2), sumaFlotantes(flo, flo2))
	fmt.Printf("Usando funcion generica, %v y %v\n", SumaNumeros[int32](num, num2), SumaNumeros[float32](flo, flo2))
}
  
...

func SumaNumeros[V int32 | float32](num, num2 V) V {
	return num + num2
}
```

- En este metodo recibimos dos argumentos que recibe y regresan el tipo de valor `V`.
- Entre braquets especificamos el `V` tipo el cual especificamos que acepte dos valores `int32` y `float32` los cuales seran aceptados por el compilador.
- Cuando invocamos la funcion ` SumaNumeros[int32](2, 2)` entre los braquets especificamos el valor generico, que en este caso son lo valores que va a aceptar y regresar.

```bash
$ go run calculadora.go
Usando funciones no genericas 4 y 4.7
Usando funcion generica, 4 y 4.7
```

### Inferir argumento
```go {linenos=table,hl_lines=[4],linenostart=1}

func main() {
	var num, num2 int32 = 2, 2
	var flo, flo2 float32 = 3.5, 1.2
	fmt.Printf("Usando funciones no genericas %v y %v\n", sumaEnteros(num, num2), sumaFlotantes(flo, flo2))
	fmt.Printf("Usando funcion generica, %v y %v\n", SumaNumeros[int32](num, num2), SumaNumeros[float32](flo, flo2))
	fmt.Printf("Usando funcion generica, con inferencia de tipos, %v y %v\n", SumaNumeros(num, num2), SumaNumeros(flo, flo2))
}  

```

Si removemos de los braquets el tipo de valor, le dejamos al compilador que infiera el tipo de valor al momento de mandarle las variables.

```bash
$ go run calculadora.go
Usando funciones no genericas 4 y 4.7
Usando funcion generica, 4 y 4.7
Usando funcion generica, con inferencia de tipos, 4 y 4.7
```

### Definiendo restricciones de tipo
Podemos declarar tipo de restricciones como interfaz, de esta manera en la interfaz limitamos el tipo de valores que aceptara nuestra funcion con genericos

```go {linenos=table,hl_lines=["1-3", 10, "15-18"],linenostart=1}
type Numero interface {
	int32 | float32
} 

func main() {
	var num, num2 int32 = 2, 2
	var flo, flo2 float32 = 3.5, 1.2
	fmt.Printf("Usando funciones no genericas %v y %v\n", sumaEnteros(num, num2), sumaFlotantes(flo, flo2))
	fmt.Printf("Usando funcion generica, %v y %v\n", SumaGenericos[int32](num, num2), SumaGenericos[float32](flo, flo2))
	fmt.Printf("Usando funcion generica, con inferencia de tipos, %v y %v\n", SumaGenericos(num, num2), SumaGenericos(flo, flo2))
	fmt.Printf("Usando funcion generica, con restricciones, %v y %v\n", SumaNumeros(num, num2), SumaNumeros(flo, flo2))
}  

func SumaNumeros[V Numero](num, num2 V) V {
	return num + num2
}

```

Al correr el codigo obtenemos

```bash
$ go run calculadora.go
Usando funciones no genericas 4 y 4.7
Usando funcion generica, 4 y 4.7
Usando funcion generica, con inferencia de tipos, 4 y 4.7
Usando funcion generica, con restricciones, 4 y 4.7
```

## Conclución
Ahora vimos una introduccción de como implementar los genericos en go, son muy utilizados sobre todo en estructura de datos.

## Glosario

#### Pila 

Una pila es una estructura de datos responde a las siguientes relgas, los primeros elementos en entrar son los ultimos en salir de la pila, general mente tiene los metodos, `longitud`, `insertar` y `extraer`