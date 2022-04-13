---
title: "Curso Go 02: Control De Flujo"
date: 2022-04-12T19:19:23-05:00
draft: false
categorías:
    - Curso
    - Golang
tags:
    - Curso
    - Golang
    - Básico
    - If
    - For
cover:
    image: "/img/control-de-flujo/gopher-if.png"
    alt: "gopher control"    
    relative: false
---

Vamos a continuar con esta pequeña serie de post de introducción a Go, si no has visto el primero, te recomiendo visitar [los fundamentos de go](../curso-go-fundamentos). 

Cuando hablamos de control de flujo en nuestros sistemas, nos referimos a la estructura de control que nos permite determinar el orden o condiciones en que se ejecutan las instrucciones, para esto nos apoyamos de condicionales y ciclos, Característica que todo lenguaje de programación tiene.

## Condicionales

Usamos las condicionales para plantear escenarios, los cuales llevaremos a cabo solo si se cumplen la o las condiciones. Veamos un ejemplo en pseudocódigo.

```bash
Serbir te en la taza
Tomar la taza
Si la taza esta caliente, entonces:
  Soplar en la taza por un segundo
Fin de la condición
Beber te de la taza
```
### IF
Este es un ejemplo verificamos el estado de una taza, basándonos su estado decidiremos que acción tomar. Para representar este tipo de toma de decisiones en go utilizamos la palabra reservada `if`, la cual su estructura completa seria:

```go 
if condición{
  acciones
}
```
Usando nuestro ejemplo en pseudo código, veamos como se ve escrito en go.

```go
func main() {
  taza := servirTe()
  if taza > 30 {
    fmt.Println("La taza esta caliente, debo soplar")
    taza = soplar(taza)
  }
  fmt.Println("Ahora puedo beber mi te")
}
```

En el ejemplo asumimos que `servirTe()` nos devuelve un entero entre 20 y 35, guardamos el valor en nuestra taza, verificamos la temperatura de la misma, si la temperatura es mayor a 30 grados, entonces se soplara antes de beber, si no, solo se beberá. Fácil ¿No?.

### Else
Ahora bien, comúnmente cuando plantemos una condición a alguien podemos darle una alternativa, ejemplo: Voy a preparar té, SI NO tengo té, entonces prepararé café. Para realizar el segundo escenario en Go utilizamos la palabra reservada `else`, la cual su estructura completa seria:

```go
if condición {
  acción si la condición se cumple
} else {
  acción si la condición no se cumple
}
```

Es importante notar que él `else` debe ir al final de la estructura de control, si no, nos generará un error. Veamos como representaríamos nuestro escenario de que bebida preparar escrito en código.

```go
func main() {
  bolsasDeTe := 2

  if bolsasDeTe > 0 {
    fmt.Println("Tengo bolsas de te, voy a preparar uno")
    prepararTe()
  } else {
    fmt.Println("No tengo bolsas de te, voy a preparar café")
    prepararCafe()
  }
}
```
Recuerda que todas las acciones que pongas dentro de las llaves se van a ejecutar según la condición que se cumpla. En este caso como aún tenemos bolsas de té, entonces preparamos una taza de té y descontaremos 1 a nuestras bolsas de té.

## Símbolos de comparaciones

Hasta el momento hemos visto las condicionales, su estructura y como funcionan. Pero también es importante conocer todos los símbolos que podemos usar para nuestras sentencias al momento de comparar:

| Símbolo  | Descripción                                                                                                               |
|:--------:|---------------------------------------------------------------------------------------------------------------------------|
|    >     | Nuestra condición se cumple solo si nuestro primer valor es `mayor que` el segundo con el cual estamos comparando         |
|    <     | Nuestra condición se cumple solo si nuestro primer valor es `menor que` el segundo con el cual estamos comparando         |
|    >=    | Nuestra condición se cumple solo si nuestro primer valor es `mayor o igual que` el segundo con el cual estamos comparando |
|    <=    | Nuestra condición se cumple solo si nuestro primer valor es `menor o igual que` el segundo con el cual estamos comparando |
|    ==    | Nuestra condición se cumple solo si nuestro primer valor es `igual que` el segundo con el cual estamos comparando         |
|    !=    | Nuestra condición se cumple solo si nuestro primer valor es `diferente que` el segundo con el cual estamos comparando     |

## Ciclos

Otro caso muy común al momento de escribir nuestros sistemas, es cuando surge la necesidad de repetir una acción más de una vez, hasta que cierta condición se cumpla. Esto es lo que conocemos como ciclos. En nuestro primer ejemplo verificábamos si la taza estaba caliente, si lo estaba debíamos soplar, pero que pasa si la taza aún seguía caliente, solo soplamos una vez y bebimos el té, nada nos aseguraba que fue lo suficiente para que se pudiera beber. Pues bien si hubiéramos repetido la acción de soplar hasta que estuviera no tan caliente para quemarnos, pero si lo suficiente para que su temperatura siguiera siendo reconfortante habríamos tenido una mejor experiencia. Veamos como los ciclos nos pueden ayudar a beber una mejor taza de té (o café).

En pseudocódigo se vería asi:

```bash
servir te en la taza
mientras la taza esta caliente, repetir:
  soplar en la taza
Fin del ciclo
Beber la taza
```

Se ve muy similar al escenario anterior, pero en este caso nuestra palabra clave es `repetir`, la cual nos indica que vamos a estar haciendo una acción en repetidas ocasiones. Para representar eso en go usamos la palabra reservada `for`, la cual su estructura completa seria:

```go
for condición {
  acciones
}
```

Ahora bien pasando nuestro ejemplo de pseudocódigo a go seria asi:
  
```go
func main() {
	taza := 35
	fmt.Printf("La taza tiene una temperatura de %d grados \n", taza)
	for taza > 30 {
		fmt.Printf("La taza aun esta caliente %d, debo soplar\n", taza)
		taza = taza - 2
	}
	fmt.Printf("La taza tiene una temperatura de %d, ahora puedo beber mi te", taza)
}
```

Si corremos nuestro código, obtendremos el siguiente resultado:

```bash
La taza tiene una temperatura de 35 grados 
La taza aun esta caliente 35, debo soplar
La taza aun esta caliente 33, debo soplar
La taza aun esta caliente 31, debo soplar
La taza tiene una temperatura de 29, ahora puedo beber mi te
```

Podemos ver que "soplamos" 3 veces antes de poder beber de la taza, a pesar de que en nuestro código solo escribimos una vez ese comando. Esto es gracias a nuestro ciclo for.

Asi es como se ve nuestro programa completo, con condicionales y ciclos:

```go
package main

import "fmt"

func main() {
	bolsasDeTe := 2
	var bebida string

	if bolsasDeTe > 0 {
		fmt.Println("Tengo bolsas de te, voy a preparar uno")
		bebida = "te"
	} else {
		fmt.Println("No tengo bolsas de te, voy a preparar café")
		bebida = "café"
	}

	taza := 35
	fmt.Printf("La taza tiene una temperatura de %d grados \n", taza)
	for taza > 30 {
		fmt.Printf("La taza aun esta caliente %d, debo soplar\n", taza)
		taza = taza - 2
	}
	fmt.Printf("La taza tiene una temperatura de %d, ahora puedo beber mi %s", taza, bebida)
}

```

Genial, hasta el momento ya sabes como ponerle condiciones a tu programa y como hacer que repita acciones más de una vez. Estamos un paso más cerca de aprender Go. 

Si ya tienes experiencia en programación y vienes de otro lenguaje, quizás notes que el ciclo `for` es diferente a lo que normalmente conoces, tal vez también estes preguntando la estructura de `while` o `do while`, paciencia, vamos alla.

## Control de flujo intermedio

En algunas ocasiones es probable que necesitemos plantear más de 2 escenarios. Para lo cual necesitaremos usar los `if` anidados, Si queremos evaluar la etapa en la que se encuentra una persona dependiendo de su edad, para este ejemplo nos limitaremos a tres, infante, adolescence y adulto. Son 3 posibles etapas, si solo usamos `if else` sería algo como esto:

```go
func main() {
  edad := 18
  if edad < 12 {
		fmt.Println("Eres un infante")
	} else {
		if edad < 18 {
			fmt.Println("Eres un adolescente")
		} else {
			fmt.Println("Eres un adulto")
		}
	}
}
```

Si bien, esto es una solución válida. También es importante mencionar que mientras más condicionales anidemos dentro de nuestras sentencias, más difícil se volverá de leer y mantener nuestro código. Tenemos dos opciones, usar `else if` o `switch`:

### Else if

```go 
func main() {
	edad := 18

	if edad < 12 {
		fmt.Println("Eres un infante")
	} else if edad < 18 {
		fmt.Println("Eres un adolescente")
	} else {
		fmt.Println("Eres un adulto")
	}
}
```

En este ejemplo tenemos un solo bloque de condicionales, en el cual podemos agregar más escenarios. Cuando usamos este tipo de soluciones es impórtate tener cuidado el orden de nuestras condiciones, ya que si una es verdadera, el resto de condiciones no se evaluaran.

### Switch

Otra opción que tenemos, es usar `switch` para evaluar una condición. Suele ser la mejor opción si tenemos más de dos condiciones, ya que su sintaxis hace más legible los diferentes escenarios que tenemos. Las características de `switch` es que usa la palabra reservada `case` para evaluar una condición, y en el caso que no se cumpla ninguna de las condiciones, se ejecutara el escenario por defecto `default`.

```go
func main() {
	switch {
	case edad < 12:
		fmt.Println("Eres menor de edad")
	case edad < 18:
		fmt.Println("Eres mayor de edad")
	default:
		fmt.Println("Eres menor de edad")
	}
}
```

## Ciclo for

A diferencia de otros lenguajes que tienen los ciclos `for`, `while` y `do while`, en Go hay solo uno, que como ya sabes es `for`. Pero este ciclo puede ser usado de diferentes maneras para emular el comportamiento de un ciclo `while` o `do while`.

### Sintaxis de for

La sintaxis de un ciclo for se compone de secciones, todas separadas por punto y coma:

- **Declaración**: En esta sección podemos nuestras variables, normalmente usamos una, pero si lo necesitamos, podemos inicializar más de una. Esta parte del código se ejecuta una sola vez, antes de que comience el ciclo.
- **Condición**: En esta sección podemos evaluar si cumplimos la condición para poder ejecutar o no las acciones de nuestro ciclo, es evaluada cada vez antes de "entrar" a nuestro ciclo.
- **Incremento**: En esta sección normalmente aumentamos nuestras variables que nos sirven como control del ciclo, esta parte del código se ejecuta cada vez al finalizar nuestro ciclo y antes de evaluar nuestra condición.

```go
for declaracion; condicion; incremento {
	acciones
}
```

Las 3 secciones son opcionales y es esta característica la que nos permite emular otros formatos de ciclos.

#### While

La estructura de un ciclo while se compone solo de una sección, la cual es la condición que evaluamos para saber si el ciclo se ejecuta o no. Usando `for` en go se puede usar de la siguiente manera:

```go
for condición{
	acciones
}
```

#### Do while

Los ciclos `do while` son usados para asegurarnos que el código que está dentro de nuestro ciclo se ejecuta al menos una vez, ya que la condición se encuentra al final de nuestro ciclo. Para poder emular este comportamiento en go necesitaremos usar una palabra reservada llamada `break`, esta palabra nos permite salir de un ciclo.

```go
for {
	acciones
	if condición{
		break
	}
}
```

### Break y Continue

Como acabamos de mencionar `break` es usado para interrumpir la ejecución de un ciclo justo en el momento que se ejecuta. Lo que quiere decir que el código que este después de `break` no se ejecutara. Esto es muy útil, por ejemplo, cuando estamos buscando un valor en una lista y una vez que lo encontramos podemos salir del ciclo.

```go
func main() {
	for i := 1; i <= 10; i++ {
		fmt.Println(i)
		if i == 5 {
			break
		}
	}
}
```

Es ciclo, normalmente si no tuviera él `break`, nos mostraría una lista del uno al diez. Pero si lo corres, verás que gracias a la condición terminara la ejecución después de imprimir el "5".

En cambio `continue` es usado para ir al inicio de nuestro ciclo y volver a evaluarlo. Esto no quiere decir que volveremos a iniciar el estado de nuestro ciclo, solo nos iremos directamente a la sección de incremento (si es que la declaramos) en el momento en que `continue` se ejecute.

```go
func main() {
	for i := 1; i <= 10; i++ {
		if (i % 2) != 0 {
			continue
		}
		fmt.Println(i)
	}
}
```

De igual manera, el ejemplo anterior, normalmente nos imprimiría una lista de los números pares del 1 al 10, pero si lo corres, verás que gracias a la condición, nos imprimirá solo los pares. Ya que nuestra condición le dice que si detecta un número impar, volverá al inicio, justo antes de poder imprimir el número.

## Tablas de verdad

Por último, si notaste tanto los ciclos como las condicionales tienen algo en común y es que evalúan sentencias. Para poder ejecutar sus acciones verifican que un valor sea, igual, mayor, menor, etc. Si queremos sacar mayor provecho de esta característica es importante conocer las tablas de verdad. La cual nos ayudara para poder usar más de una condición en nuestras sentencias.

|   Operador   |  Nombre  | Descripción                                         |
|:------------:|:--------:|-----------------------------------------------------|
|      &&      |   AND    | Se evalúa si ambas condiciones son verdaderas       |
| &#124;&#124; |    OR    | Se evalúa si alguna de las condiciones es verdadera |

### AND &&

Para que las condiciones en esta sentencia se ejecuten, ambas condiciones deben cumplirse. Ejemplo, imagina que a ti solo te gusta el té de hierba buena y a pesar de aun tener bolsas de té, solo lo preparas, si es de tu sabor favorito. Tendríamos un código como el siguiente:

```go
func main() {
	bolsasDeTe := 2
	Sabor := "Durazno"

	if bolsasDeTe > 0 && Sabor == "Hierba buena" {
		fmt.Println("Voy a disfrutar un de de mi sabor favorito", Sabor)
	} else if bolsasDeTe > 0 {
		fmt.Println("Si hay te, pero solo hay sabor", Sabor)
	} else {
		fmt.Println("No tengo bolsas de te")
	}
}
```

Si ejecutamos el código, veremos que no nos fue posible preparar un té, ya que a pesar de tener bolsas de té, no es de nuestro sabor favorito. En este caso el resultado fue `verdadero`, `falso` y dado que usamos el operador AND ambas condiciones tenían que ser verdaderas para poder entrar a la primera condición.

### OR ||

El operador OR nos permite evaluar si alguna de las condiciones es verdadera. Ahora somos más permisivos y nos aventuramos a probar diferentes sabores por lo que si nos ofrecen una taza de té, la beberemos si es de hierba buena o de durazno. En este caso usaremos el operador OR para representar ese escenario.

```go
func main() {	
	Sabor := "Durazno"

	if Sabor == "Durazno" || Sabor == "Hierba buena" {
		fmt.Println("Voy a beber un te sabor", Sabor)
	} else  {
		fmt.Println("No hay ningún sabor que me guste, solo hay", Sabor)
	} 
}
```

## Conclusion

Felicidades por llegar hasta aquí, ahora sabes como controlar el flujo de la información en tu aplicación. Algo realmente útil, si tienes una duda recuerda que puedes escribirme en mi twitter [@esuarezdev](https://twitter.com/esuarezdev)
