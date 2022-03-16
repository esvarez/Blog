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
    alt: "go the logo"    
    relative: false 
---

El día 15 de marzo fue anunciada oficialmente la nueva version de go 1.18 y con ella las nuevas características entre las que destacan, [Fuzzing](../fuzz-testing-go-fuzzing), el uso de workspaces y generics, de esto último hablaremos en este post.

## Generics

Lo primero, es saber que son los *generics* o a que nos referimos cuando hablamos de ellos, como quizás ya sepas, en un lenguaje fuertemente tipado es necesario declarar el tipo de variables que vamos a usar y que nuestros métodos o funciones van a recibir o devolver. Esto nos ayuda a ser más eficientes con el uso de memoria y con la ejecución de nuestros sistemas, porque sabemos cuanto es el espacio máximo que podríamos llegar a usar. 

Pero esta misma característica nos complica un poco la vida cuando tenemos más de un método que va a hacer exactamente lo mismo, solo cambiando el tipo de variables que va a devolver. Imaginemos un ejemplo simple, una calculadora en el cual tuviéramos lo siguiente:

```go
func sumarEnteros (num, num2 int) int {
	return num + num2
}

func sumarFlotantes(num, num2 float) float {
	return num + num2
}
```

Como podemos observar tenemos exactamente el mismo código en ambas funciones, la única diferencia es el tipo de variable que reciben y que regresan. Este es un ejemplo simple y parece poco código, pero imagina que implementas una [pila](#pila) en el cual debemos soportar varios tipos de datos, nuestro código se vería así.

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

Todas las funciones tienen la misma lógica, solo cambia el valor que recibimos y retornamos y puede crecer aún más, quizá necesitamos soportar booleanos, flotantes, etc. Y si necesitamos actualizar la lógica al momento de insertar es algo que debemos hacer en todas nuestras funciones, si lo vemos así suena a mucho trabajo, código duplicado y más posibilidades de crear un bug, afortunadamente existen los *generics* y llegaron a **go**.

Los *generics* declaran el tipo de variable en tiempo de ejecución y no de compilación como lo hace cualquier otra variable en los lenguajes compilados. De esta manera podemos marcar nuestra función como genérica y definir nuestro tipo de variable una vez que ya fue compilado nuestro código.

## Practica
Es importante mencionar que para poder seguir este post es necesario tener instalada la version 1.18 de go. Si aún no has instalado go, puedes seguir el siguiente [post](../instalar-go).
### Calculadora básica
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

### Implementando *generics*

Hasta aquí declaramos dos funciones para sumar enteros y flotantes, como ya mencionamos, estamos duplicando código, pero las mismas características del lenguaje nos obligaba a hacerlo, hasta ahora, usando generics podemos tener una sola función que se va a encargar de sumar números, la cual no necesita saber si serán, *int* o *float*, si no hasta que se implemente. 

Agregamos una nueva función a nuestro código, para comparar con lo que ya teníamos previamente.

```go {linenos=table,hl_lines=[7, "12-14"],linenostart=1}
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

- En esta función recibimos dos argumentos que recibe y regresan el tipo de valor `V`.
- Entre corchetes especificamos el tipo `V` el cual declaramos que acepte dos valores `int32` y `float32`.
- Cuando invocamos la función ` SumaNumeros[int32](2, 2)` entre los corchetes especificamos el tipo de valor que va a tomar `V`.

```bash
$ go run calculadora.go
Usando funciones no genericas 4 y 4.7
Usando funcion generica, 4 y 4.7
```

### Inferir argumento
```go {linenos=table,hl_lines=[7],linenostart=1}

func main() {
	var num, num2 int32 = 2, 2
	var flo, flo2 float32 = 3.5, 1.2
	fmt.Printf("Usando funciones no genericas %v y %v\n", sumaEnteros(num, num2), sumaFlotantes(flo, flo2))
	fmt.Printf("Usando funcion generica, %v y %v\n", SumaNumeros[int32](num, num2), SumaNumeros[float32](flo, flo2))
	fmt.Printf("Usando funcion generica, con inferencia de tipos, %v y %v\n", SumaNumeros(num, num2), SumaNumeros(flo, flo2))
}  

```

Si removemos de los corchetes, le dejamos al compilador la tarea de inferir el tipo del dato, en este caso como nuestras variables son `int32` y `float32` sabe que tipo tomar en cada caso.

```bash
$ go run calculadora.go
Usando funciones no genericas 4 y 4.7
Usando funcion generica, 4 y 4.7
Usando funcion generica, con inferencia de tipos, 4 y 4.7
```

### Definiendo restricciones de tipo
Podemos declarar tipo de restricciones con una interfaz, de esta manera en ella limitamos el tipo de valores que aceptara nuestra función con *generics*, esto nos dara un código más legible y limpio.

```go {linenos=table,hl_lines=["1-3", 10, "14-16"],linenostart=5}
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

Al correr el código obtenemos

```bash
$ go run calculadora.go
Usando funciones no genericas 4 y 4.7
Usando funcion generica, 4 y 4.7
Usando funcion generica, con inferencia de tipos, 4 y 4.7
Usando funcion generica, con restricciones, 4 y 4.7
```

## Conclusión
Ahora vimos una introducción de como implementar los *generics* en go, son muy útiles sobre todo al momento de implementar estructura de datos.

## Glosario

#### Pila 

Una pila es una estructura de datos responde a las siguientes reglas, los primeros elementos en entrar son los últimos en salir de la pila, general mente tiene las funciones, `size()` (tamaño de la pila) , `push(item)` (insertar un valor en la pila) y `pop()` (devuelve y eliminar el valor de la pila que esta arriba).
