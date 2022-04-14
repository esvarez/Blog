---
title: "Curso Go: Arrays and Slices"
date: 2022-04-13T13:45:06-05:00
draft: true
---

## Arreglos
Arreglos o *array*, a pesar de que este tema va de la mano con las [variables](../curso-go-fundamentos.md), quise tomarme el tiempo para dedicar un post completo sobre este tema y asi poder ir mas a fondo. Empecemos con lo básico, **¿Qué es un arreglo?**. Un arreglo es una colección de datos que se pueden acceder por medio de un índice. Es como si tubieramos una fila de cajones, dentro de cada cajon hay un valor y cada cajon es representado por un indice. Que normalmente empieza en 0 y termina en el tamaño del arreglo menos.

Asi es como se vería un arreglo de 5 elementos, si accedemos al indice 2, nos devolvería el valor 17:
```
Valor:  [5][10][17][13][23]
Indice:  0  1   2   3   4
```
Bien, ya tenemos en nuestra menta la representación de un arreglo, pero ¿Como se declara en go?.

```go
// Estructura: 
// var nombre [tamaño]tipo

// Declaración de un arreglo, tamaño 5
var numeros [5]int

// Declara e inicializa un arreglo de 5 elementos
palabras := [5]string{"arreglos", "y", "slices", "en", "Go"}
```

Si bien existe la posibilidad de incializar nuestro arreglo, es no nos impode modificarlo durante la ejecución del programa.

```go
func main() {
	var numeros [5]int // [0, 0, 0, 0, 0]
	numeros[0] = 1     // [1, 0, 0, 0, 0]
	numeros[4] = 10    // [1, 0, 0, 0, 10]
}

```

Si un arreglo no es inicializado, todos sus campos serán poblados con [zero values](../curso-go-fundamentos/#zero-values), para comprobarlo escribamos el siguiente código, vamos a recorrer todo nuestro arreglo haciendo uso de los [ciclos](../control-de-flujo):

```go
func main() {
	var numeros [5]int

	for index := 0; index < len(numeros); index++ {
		fmt.Printf("En indice %d contiene el valor %d\n", index, numeros[index])
	}
}
```
Salida:

```bash
En indice 0 contiene el valor 0
En indice 1 contiene el valor 0
En indice 2 contiene el valor 0
En indice 3 contiene el valor 0
En indice 4 contiene el valor 0
```

El metodo `len` nos devuelve el tamaño del arreglo. Lo usaremos para saber nuestra condición de salida del ciclo, necesitamos que el el ultimo numero sea 4, ya que si llegamos el 5 estaremos accediendo a un indice que no existe. Recuerda que los indices empiezan en 0, por lo que en un arreglo de tamaño 5, su ultimo indice es el 4.

Otra forma que existe para recorrer un arreglo es usando `range`, range va a iterar sobre nuesto arreglo devolviendonos el indice y el valor de cada elemento. El cual podemos usar dependiendo de nuestra necesidad.

```go
func main() {
	palabras := [5]string{"arreglos", "y", "slices", "en", "Go"}

  // Obteniendo solo el indice
  for index := range palabras {
		fmt.Printf("En indice %d contiene el valor: %s\n", index, palabras[index])
	}
  // Obteniendo solo el valor
	for _, palabra := range palabras {
		fmt.Println(palabra)
	}
  // Obteniendo indece y el valor del indice
	for index, palabra := range palabras {
		fmt.Printf("En indice %d contiene el valor: %s\n", index, palabra)
	}  
}
```

