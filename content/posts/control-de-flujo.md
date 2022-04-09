---
title: "Curso Go 02: Control De Flujo"
date: 2022-04-08T19:19:23-05:00
draft: true
---

Vamos a continuar con esta pequena serie de post de introducción a Go, si no has visto el primero, te recomiendo visitar [los fundamentos de go](./curso-go-fundamentos.md). Cuando hablamos de control de flujo  en nuestros sistemas, nos referimos a la estructura de control que nos permite determinar el orden o condiciones en que se ejecutan las instrucciones, para esto nos apoyamos de condicionales y ciclos, Caracteristicas que todo lenguaje de programación tiene.

## Condicionales

Usamos las condicinales para plantear escenarios, los cuales llevaremos a cabo solo si se cumplen la o las condiciones. Veamos un ejemplo en pseudocodigo.

```bash
Serbir te en la taza
Tomar la taza
Si la taza esta caliente, entonces:
  Soplar en la taza por un segundo
Fin de la condición
Beber te de la taza
```
### IF
Este es un ejemplo verificamos el estado de una taza, basandonos su estado decidiremos que accion tomar. Para representar este tipo de toma de deciciones en go utilizamos la palbra reservada `if`, la cual su estructura completa seria:

```go 
if condicion {
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

En el ejemplo asumimos que `servirTe()` nos devuelve un entero entre 20 y 35, guardamos el valor en nuestra taza, verificamos en la temperatura de la misma, si la temperatura es mayor a 30 grados, entonces se soplara antes de beber, si no, solo se bebera. Facil ¿No?.

### Else
Ahora bien, comunmente cuando cuando plantemos una condicion a alguien podemos darle una alternativa, ejemplo: Voy a preparar te, SI NO tengo te, entonces prepararé café. Para realizar el segundo escenario en Go utilizamos la palabra reservada `else`, la cual su estructura completa seria:

```go
if condicion {
  acción si la condicion se cumple
} else {
  acción si la condicion no se cumple
}
```

Es importate notar que el `else` debe ir al final de la estructura de control, si no, nos generará un error. Veamos como representariamos nuestro escenario de que bebida prepar escrito en codigo.

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
Recuerda que todas las acciones que pongas dentro de las llaves se van a ejecutar segun la condicion que se cumpla. En este caso como aun tenemos bolsas de te, entonces preparamos una taza de te y descontaremos 1 a nuestras bolsas de te.

## Simbolos de comparaciones

Hasta el momento hemos visto las condicionales, su estructura y como funcionan. Pero tambien es importante conocer todos los simbolos que podemos usar para nuestras sentencias al momento de comparar:

| Simbolo | Descripcion |
| :----: |------|
| > | Nuestra condicion se cumple solo si nuestro primer valor es `mayor que` el segundo con el cual estamos comparando |
| < | Nuestra condicion se cumple solo si nuestro primer valor es `menor que` el segundo con el cual estamos comparando |
| >= | Nuestra condicion se cumple solo si nuestro primer valor es `mayor o igual que` el segundo con el cual estamos comparando |
| <= | Nuestra condicion se cumple solo si nuestro primer valor es `menor o igual que` el segundo con el cual estamos comparando |
| == | Nuestra condicion se cumple solo si nuestro primer valor es `igual que` el segundo con el cual estamos comparando |
| != | Nuestra condicion se cumple solo si nuestro primer valor es `diferente que` el segundo con el cual estamos comparando |

## Ciclos

Otro caso muy comun al momento de escribir nuestros sistemas, es cuando surge la necesidad de repetir una accion mas de una vez, hasta que cierta condicion se cumpla. Esto es lo que conocemos como ciclos. En nuestro primer ejemplo verificabamos si la taza estaba caliente, si lo estaba debiamos soplar, pero que pasa si la taza aun seguia caliente, solo soplamos una vez y bebimos el te, nada nos aseguraba que fue suficiente para que se pudiera beber. Pues bien si hubieramos repetido la accion de soplar hasta que estubiera no tan caliente para quemarnos pero si lo suficiente para que su temperaura siguiera siendo reconfortante habriamos tenido una mejor experiencia. Veamos como los ciclos nos pueden ayudar a beber una mejor taza de te (o café).

En pseudocodigo se veria asi:

```bash
servir te en la taza
mientras la taza esta caliente, repetir:
  soplar en la taza
Fin del ciclo
Beber la taza
```

Se ve muy similar al escenario anterior, pero en este caso nuestra palabra clave es `repetir`, la cual nos indica que vamos a estar haciendo una accion en repetidas ocaciones. Para representar eso en go usamos la palabra reservada `for`, la cual su estructura completa seria:

```go
for condicion {
  acciones
}
```

Ahora bien pasando nuestro ejemplo de pseudocodigo a go seria asi:
  
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

Podemos ver que "soplamos" 3 veces antes de poder beber de la taza, a apesar de que en nuestro codigo solo escribimos una vez ese comando. Esto es gracias a nuestro ciclo for.

Genial, hasta el momento ya sabes como ponerle condiciones a tu programa y como hacer que repita acciones mas de una vez. Estamos un paso mas cerca de aprender Go. Si ya tienes experiencia en programacion y vienes de otro lenguaje, quizas notes que el ciclo `for` es diferente a lo que normalmente conoces, tal vez tambien estes preguntando la estrucutra de `while` o `do while`, paciencia, vamos alla.

# Control de flujo intermedio