---
title: "Curso Go 01: Fundamentos"
date: 2022-03-02T10:34:29-06:00
draft: false
categories:
    - Curso
    - Golang
tags:
    - Curso
    - Go
    - Golang    
    - Básico
    - Variables
cover:
    image: "/img/curso-go-fundamentos/gophers_aprendiendo.webp"
    alt: "gopher tester"    
    relative: false 
---

Go, también conocido como **golang**, es un lenguaje de programación creado por Google en 2012, es uno de los lenguajes recomendados para aprender a programar por su *sintaxis* simple, a pesar de ser [tipado](#tipado) y [compilado](#compilado).

## Instalar Go

Si aun no tienes instalado go, puedes hacerlo siguiente este post [pagina oficial](./instalar-go.md)

Para instalar Go, es necesario ir a la [pagina oficial](https://go.dev/doc/install), descargar el paquete de acuerdo a tu sistema operativo y seguir las siguientes instrucciones.

### Windows

1. Abrimos el archivo descargado y seguimos las instrucciones.

2. Verificamos que la instalación sea correcta
    1. Abrimos la línea de comandos
        1. En Inicio buscamos `cmd` y damos enter.
    2. Escribimos el siguiente comando en nuestra línea de comandos:
```bash
$ go version
```

### Linux

1. Extraer el archivo en `/usr/local` con el siguiente comando:

```bash
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.8.linux-amd64.tar.gz
```

2. Agregar `/usr/local/go/bin` a nuestras variables de ambiente
```bash
export PATH=$PATH:/usr/local/go/bin
```
  Estos cambios se verán reflejados hasta la próxima vez que inicies tu consola, para aplicar los cambios usa el comando

```bash
source $HOME/.profile.
```

3. Verificar nuestra instalación de Go, el siguiente comando nos debe imprimir nuestra version actual:
```bash
$ go version
```

### MacOS

1. Abrir el paquete y seguir las instrucciones de instalación
  Necesitamos reiniciar las terminales que tenemos abiertas para aplicar el cambio

2. Verificamos que nuestra version de go se haya instalado correctamente:
```bash
$ go version
```

## Hola mundo

Estamos a punto de escribir nuestras primeras líneas en Go. Para escribir nuestro código, creamos nuestra carpeta, recomiendo nombrarla `curso_go`, podemos usar el editor o IDE que más nos guste.

Crearemos nuestro primer modulo, dentro de nuestra carpeta el comando:
```bash
$ go mod init ejemplo/hola
go: creating new go.mod: module ejemplo/hola
```

Seguimos dentro de nuestra carpeta, crearemos nuestro archivo `main.go`, puede tener cualquier nombre, pero por convención y buenas practicas se recomienda nombrarlo así.

Empezamos creando el nombre de nuestro paquete, una buena practica es usar un nombre descriptivo. Siempre que inicies un nuevo programa en Go, tu paquete principal debe ser `main`.

```go {linenos=table,hl_lines=[1],linenostart=1}
package main
```

De igual manera el nombre de nuestra función, debe ser `main()`, la primer función en ser ejecutada cuando corra nuestro programa. Aquí podemos ver que estamos usando la [función](#función) **Println** del paquete **fmt**, que está recibiendo como [parámetro](#parámetro) nuestro mensaje *"Hola mundo!!"*, el cual para poder usar es necesario importar el paquete, como se ve en la línea 3.

```go {linenos=table,hl_lines=[1, "3-5"],linenostart=3}
import "fmt"

func main()  {
	fmt.Println("Hola mundo!!")
}
```

Nuestro programa completo debe verse así:

```go
package main

import "fmt"

func main()  {
	fmt.Println("Hola mundo!!")
}
```

Si ejecutemos, con el comando `go run main.go`, nos debe mostrar el siguiente mensaje.

```bash
$ go run main.go
Hola mundo!!
```

## Variables y constantes

### Variables

Las variables son una parte importante de cualquier lenguaje de programación. Gracias a ella podemos manejar el estado de nuestra aplicación.

Hay diferentes formas de declarar una o más variables, todas son validas y depende de tus necesidades.
```go
func main() {
    // Declarar
    var edad int
    fmt.Println(edad) // 0
    
    // asignar valor
    edad = 10
    fmt.Println(edad) // 10
    
    // Declarar explicitamente e inicilizar
    var nombre string = "Gopher"
    fmt.Println(nombre) // Gopher
    
    // Declaracion multiple
    var ciudad, estado string = "Guadalajara", "Jalisco"
    fmt.Println(ciudad, estado) // Guadalajara Jalisco
    
    // inferir tipo de valor
    var activo = true
    fmt.Println(activo) // true
    
    // La sintaxis ':=' es un atajo para declarar e inizilizar 
    alias := "Go"
    fmt.Println(alias) // Go
}
```
Formas invalidas de declarar una variable

```go
func main() {
    var edad int
    fmt.Println(edad) // 0  
    
    // Linea NO valida, ya que version no ah sido declarada
    version = 16
    
    // Linea NO valida, edad ya fue declarada
    var edad = 13
}
```

### Constantes

Las constantes en go, como su nombre lo indica, su valor es constante ya que es declarado en tiempo de compilación, o sea que no va a cambiar a lo largo de la ejecución de nuestro programa.

![no me digas, meme](/img/curso-go-fundamentos/no_me_digas.jpeg)

Existen diferentes formas en como podemos declarar constantes.

```go
func main() {
    // Explicito
	const PI float32 = 3.14
	fmt.Println(PI) // 3.14

	// Inferir
	const NOMBRE = "Blog"
	fmt.Println(NOMBRE) // Blog

	// En bloque
	const (
		N = "NORTE"
		S = "SUR"
		E = "ESTE"
		O = "OESTE"
	)
	fmt.Println(N, S, E, O) // NORTE SUR ESTE OESTE
}
```

Si intentamos reasignar un valor a nuestra constante, el editor normalmente nos daria un warning, pero si no lo  hace o igual queremos ejecutar nuestro programa, el compilador nos regresara el siguiente mensaje de error.

```go
func main() {
  const NOMBRE = "Blog"
  
  NOMBRE = "Blog Go"
}
```
```bash
cannot assign to NOMBRE (declared const)
```

## Tipos de dato

En go, al ser un lenguaje tipado, tenemos que declarar que tipo de datos va a almacenar nuestra variable, el cual no podrá ser cambiado durante la ejecución de nuestro programa. Los tipos de datos que Go soporta son:

### Booleanos (bool)

Este tipo de dato representa dos estados, verdadero o falso (*true* o *false*), en caso de declarar una variable de tipo bool y no asignarle un valor, su *zero value* será falso

```go
func main() {
    var estoyAprendiendo bool
    estoyAprendiendo = true
    fmt.Println("Esto aprendiendo?", estoyAprendiendo) // Esto aprendiendo? true
}
```

### Cadenas (string)

Los *string*, son cadenas de caracteres que nos ayudan a representar textos, el valor debe ser colocado entre comillas dobles (*"valor"*), puede ser desde una cadena vacía ("") hasta lo que deseemos, él limite es la memoria.

```go
func main() {
    var nombre string
    nombre = "Gopher"
    fmt.Println("Hola", nombre) // Hola Gopher
}
```

### Enteros (int)

Los enteros nos ayuda a represent y números sin puntos decimal, hay diferentes tipos de enteros *int*, *int8*, *int16*, *int32* y *int64* cada uno marca la cantidad máxima de bits que puede almacenar, lo que quiere decir que tiene un valor máximo y mínimo que pueden llegar a guardar.

*int8* puede almacenar 8 bits (0000 0000), en binario el mayor numero posible es 0111 1111 traducido a decimal es 127, te preguntaras porque no 1111 1111 = 255. La explicación de esto es porque reservamos el último bit para el signo. Bajo esa misma lógica se basan los demás tipos de enteros. El tipo *int* es el valor por default cuando no se especifica y la cantidad e bits dependerá de la maquina, si es arquitectura de 32 o 64 bits.

```go
func main() {
	var min8, max8 int8 = -128, 127
	var min16, max16 int16 = -32768, 32767
	var min32, max32 int32 = -2147483648, 2147483647
	var min64, max64 int64 = -9223372036854775808, 9223372036854775807
	fmt.Println(min8, max8)
	fmt.Println(min16, max16)
	fmt.Println(min32, max32)
	fmt.Println(min64, max64)
}
```

### Enteros sin signo (uint)

También tenemos enteros sin signo, *uint*, *uint8*, *uint16*, *uint32* y *uint64*, al igual que los *int* el numero nos indica la cantidad máxima de bits que almacena la variable, pero en este caso al no reservar un bit para el signo nos permite un rango mayor de valor siendo el menor valor posible el 0.

```go
func main() {
	var max8 uint8 = 255
	var max16 uint16 = 65535
	var max32 uint32 = 4294967295
	var max64 uint64 = 18446744073709551615
	fmt.Println(max8)
	fmt.Println(max16)
	fmt.Println(max32)
	fmt.Println(max64)
}
```

### Flotantes (float)

Los flotantes es un tipo de que nos permite guardar numero con punto decimal, pueden ser *float32* y *float64* esto representa la precision da cada numero (cantidad de decimales que podemos guardar)

```go
func main() {
	var pi32 float32 = 3.141592653589793
	var pi64 float64 = 3.141592653589793

	fmt.Println(pi32) // 3.1415927
	fmt.Println(pi64) // 3.141592653589793
}
```

### Números complejos

Dentro del campo de las matemáticas tenemos números complejos, con Go podemos soportarlos de manera nativa. Tenemos dos opciones *complex64* y *complex128*, para poderlos usar solo es necesario que ambar partes sean del mismo tipo.

```go
func main() {
	var real float32 = 3
	var imaginario float32 = 5

	complejo := complex(real, imaginario)
	println(complejo)

	// Al no especificar el tipo de valor, por default sera complex128
	complejo2 := 4 + -5i
	println(complejo2)
}
```

### Byte (byte)

En Go un *byte* tiene la misma capacidad de memoria que un *uint8*, por lo que podemos representar valores de 0 a 255, con la ventaja de que podemos manejar ese valor como código ASCII y representar caracteres.

```go
func main() {
	var abyte byte = 'a'
	var intbyte byte = 71

	fmt.Printf("'%c' en valor ASCII es %d\n", abyte, abyte) // 'a' en valor ASCII es 97
	fmt.Printf("el valor ASCII %d representa '%c'\n", intbyte, intbyte) // el valor ASCII 71 representa 'G'
}
```

### Runa (rune)

Una runa es igual a *int32*, este valor entero nos ayuda a representar *Unicode*. 

Unicode es un código en el cual le asigna un numero a cada carácter que existe.

```go
func main() {
	var alpha rune = 'α'

	fmt.Printf("El caracter '%c'\nEn valor unicode %U\nEn valor entero%d", alpha, alpha, alpha)
    // El caracter 'α' 
    // En valor unicode U+03B1
    // En valor entero945%
}
```

## Zero values

Quizás notaste que en algunos momentos al declarar una variable no asignábamos valor y al imprimir la variable nos mostraba algo, esto es porque los valores primitivos de Go no pueden ser nulos *nil*, entonces por defecto se le asigna un valor, llamado *zero value*, estos valores son.

```go
func main() {
	var zeroBool bool
	var zeroStrg string
	var zeroiInt int32
	var zeroFloat float32
	var zeroRune rune
	var zeroByte byte

	fmt.Println(zeroBool)  // false
	fmt.Println(zeroStrg)  // ""
	fmt.Println(zeroiInt)  // 0
	fmt.Println(zeroFloat) // 0.0
	fmt.Println(zeroRune)  // 0
	fmt.Println(zeroByte)  // 0
}
```

## Explorando

Antes de terminar el post ¿Intentaste a un *int8* con valor 127 sumarle 1? Si no fue así, intentalo y mira que pasa 👀

¿Tienes idea de porque tiene ese valor? Si no, escríbeme a mí twitter y te explico el porque

## Conclusion

En este primer post aprendimos cosas básicas de Go, instalarlo, como declarar variables, constantes, los tipos de variables que existen e imprimir mensajes en consola, Acabas de dar un paso muy importante, atreverte a aprender un nuevo lenguaje o quizás, aprender a programar. Felicitaciones para ti! 

Si tienes dudas, siéntete libre de escribirme, estaré feliz de ayudate a resolverlas.

## Glosario

#### Tipado

Lenguaje en el cual es necesario especificar el tipo de datos que nuestras variables van a contener y al igual que el tipo de valores que nuestras funciones y métodos van a devolver

#### Compilado

Lenguaje que tiene que ser procesado para crear un equivalente en lenguaje maquina.

#### Función

Bloque de código con un propósito especifico, puede ser utilizado en varias partes del sistema, sirve para modularizar, reutilizar y hacer más legible nuestro código.

#### Parámetro

Son la o las variables que recibe nuestras funciones, las cuales pueden ser utilizadas y manipuladas dentro de ellas.
