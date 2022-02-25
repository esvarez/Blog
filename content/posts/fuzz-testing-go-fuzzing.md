---
title: "Fuzz Testing - Go Fuzzing"
date: 2022-02-24T20:36:16-06:00
draft: false
categories:
    - Tutorials
tags:
    - News
    - Go    
    - Go 1.18
cover:
    image: "/img/fuzz-testing-go-fuzzing/gopher_tester.svg"
    alt: "gopher tester"    
    relative: false 
---

La nueva versión de Go (1.18) esta cerca de ser lanzada, una de las novedades, que ya podemos probar es **Fuzzing**, estamos hablando de Fuzz Testing de manera nativa en Go. Para ponernos en contexto.

## ¿Qué es Fuzz Testing?

Es una tecnica de testing automatizado con el proposito de encontrar vulnerabilidades dando entradas invalidas a un sistema o software. 

Es practicado más comunmente por hackers e ingenieros de seguridad, los primeros para aprovechar las vulneravilidades y los segundos para arreglarlas antes de que estas puedan ser explotadas.

El fuzz testing involucra cantidades masivas de informacion que es generada de manera aleatoria con la cual será puesto a prueba el sistema con el objetivo de lograrlo romper. 

Este metodo es especialmente util para encontrar bugs y casos extremos que como humanos es muy facil olvidar o no tener en cuenta al momento de que probamos nuestros servicios.

## Go Fuzzing

Ahora que sabemos de que hablamos, ¿Cómo puedo usarlo en mis proyectos en Go? Como ya mencionamos esta carateristica esta pensada para la version 1.18 de go, que actualmente esta en beta, pero afortunadamente podemos probar ya.

### Installar version 1.18 Beta

Corremos el siguiente comando para instalar la versión beta.

```bash
$ go install golang.org/dl/go1.18beta1@latest
```

Descargamos actualizaciones
```bash
$ go1.18beta1 download
```

Debemos usar la versión beta, en lugar de la ultima (si es que la tenemos instalada)
```bash
$ go1.18beta1 version
```

O usar un alias para mayor comodidad
```bash
$ alias go=go1.18beta1
$ go version
```

### Creando el codigo a testear

1. Vamos a crear una nueva carpeta

```bash
$ mkdir fuzz
$ cd fuzz
```

2. Corremos el siguiente comando para definir un modulo
```bash
$ go mod init ejemplo/fuzz
```

3. Vamos a crear un nuevo archivo `main.go` con el siguiente metodo, su función es simple, recibir una cadena e invertirla

```go {linenos=table,hl_lines=["16-22"],linenostart=1}
package main

import (
	"fmt"
)

func main() {
  input := "Anita lava la tina"
  rev := Invertir(input)
  dobleRev := Invertir(rev)
  fmt.Printf("original: %q\n", input)
  fmt.Printf("Invertida: %q\n", rev)
  fmt.Printf("Invertir una vez más: %q\n", dobleRev)
}

func Invertir(s string) string {
  b := []byte(s)
  for i, j := 0, len(b)-1; i < len(b)/2; i, j = i+1, j-1 {
    b[i], b[j] = b[j], b[i]
  }
  return string(b)
}
```

Al correr nuestro programa deberiamos obtener una salida similar
```bash
$ go run .
original: "Anita lava la tina"
Invertida: "anit al aval atinA"
Invertir una vez más: "Anita lava la tina"
```
### Añadiendo test unitarios

1. Crearemos un archivo `main_test.go` con el siguiente codigo.

```go {linenos=table,hl_lines=[],linenostart=1}
package main

import (
	"testing"
)

func TestInvertir(t *testing.T) {
	testcases := []struct {
		in, want string
	}{
		{"Hola mundo", "odnum aloH"},
		{" ", " "},
		{"!12345", "54321!"},
	}
	for _, tc := range testcases {
		rev := Invertir(tc.in)
		if rev != tc.want {
			t.Errorf("Obtuvimos: %q, resultado esperado %q", rev, tc.want)
		}
	}
}
```

En este test solo vamos a probar que nuestra función invierta correctamente la cadena, por lo que si lo corremos deveria pasar sin mucho problema

```bash
$ go test
PASS
ok      ejemplo/fuzz  0.013s
```

### Añadindo fuzz test

Puntos importantes a tener en cuenta con respecto a los fuzz test
  - Deben ser nombrados con la siguiente nomemclatura **FuzzNombre**, empezar por *Fuzz* segudio del nombre que quieras darle
  - Los argumentos deben ser:
    - string, []byte
    - int, int8, int16, int32/rune, int64
    - uint, uint8/byte, uint16, uint32, uint64
    - float32, float64
    - bool
  - Deben estar en los archivos con terminación **_test.go**

Creamos nuestro fuzz test dentro de `main_test.go`, no olvidemos importar los nuevos paquetes que vamos a utilizar.

```go {linenos=table,hl_lines=[3]}
import (
    "testing"
    "unicode/utf8"
)

...

func FuzzInvertir(f *testing.F) {
  testcases := []string{"Hola mundo", " ", "!12345"}
  for _, tc := range testcases {
    // Añadimos el tipo de valores que queremos enviar a nuestro corpus
    f.Add(tc) 
  }
  // Nuestra funcion a probar, parte fundamental del cuerpo de nuestro fuzz test
  f.Fuzz(func(t *testing.T, orig string) {
    rev := Invertir(orig)
    dobleRev := Invertir(rev)
    if orig != dobleRev {
      t.Errorf("Antes de invertir: %q, despues del invertir: %q", orig, dobleRev)
    }
    if utf8.ValidString(orig) && !utf8.ValidString(rev) {
      t.Errorf("Invertir no devolvio una cadena UTF-8 valida %q", rev)
    }
  })
}
```

A diferenencia de los unit test en los que normalmente damos una entrada y sabemos exactamente que salida esperamos, en los fuzz test, dado que recibimos datos generados de manera aleatoria, no podemos predecir la salida esperada. Es por eso que en nuestras pruebas estamos verificando que al aplicar nuevamente nuestro metodo, recibamos la cadena original.

Tenemos dos opciones para correr nuestros test:
  - La forma por default: `go test .` Si hacemos esto, nuestro fuzz test hará la prueba con los valores con que alimentamos nuestro corpus, **No va a generar ningun valor**
  - Usando la vandera `-fuzz`: `go test . -fuzz=NombreDelTest` Si usamos esta alternativa nuestro fuzz test empezara a generar datos de manera aleatoria para probar nuestros codigo.

### Probemos el codigo

1. Primero corramos nuestro codigo de manera nativa y verifiquemos que nuestros valores de corpus pasan correctamente
```bash
go test
PASS
ok  	ejemplo/fuzz	0.672s
```

2. Ahora corramos nuestro codigo con la bandera fuzz
```bash {hl_lines=[10]}
go test -fuzz=Fuzz
fuzz: elapsed: 0s, gathering baseline coverage: 0/3 completed
fuzz: elapsed: 0s, gathering baseline coverage: 3/3 completed, now fuzzing with 12 workers
fuzz: minimizing 33-byte failing input file
fuzz: elapsed: 0s, minimizing
--- FAIL: FuzzInvertir (0.06s)
    --- FAIL: FuzzInvertir (0.00s)
        main_test.go:37: Invertir no devolvio una cadena UTF-8 valida "\x8e\xcf"

    Failing input written to testdata/fuzz/FuzzInvertir/ac96f6f1a42cb9a37e2d3e4c0a98c6d43339e291d7c8f715f7254b20f00e146c
    To re-run:
    go test -run=FuzzInvertir/ac96f6f1a42cb9a37e2d3e4c0a98c6d43339e291d7c8f715f7254b20f00e146c
FAIL
exit status 1
FAIL	ejemplo/fuzz	0.609s
```

Como vemos nuestro codigo ah fallado, a partir de este momento tenemos una nueva entrada para nuestro corpus, la entrada con la que nuestro test fallo, el cual podemos ver en el archivo generado.
```txt
go test fuzz v1
string("ώ")
```
Si volvemos a correr el comando `go test .` nuestras pruebas fallaran porque ahora el test incluye la entrada invalida que encontro previamente.

### Arreglando el error

Si gustas, eres libre de buscar el problema por ti.
En este tutorial, vamos a usar la terminar para buscar el error, por el mensaje recivido sabemos que nuestra salida UTF-8 valido.

Vamos a añadir la siguente linea de codigo para obtener mas informacion.

```go {linenos=table,hl_lines=[9]}
func FuzzInvertir(f *testing.F) {
  testcases := []string{"Hola mundo", " ", "!12345"}
  for _, tc := range testcases {
    // Añadimos el tipo de valores que queremos enviar a nuestro corpus
    f.Add(tc) 
  }
  f.Fuzz(func(t *testing.T, orig string) {
    rev := Invertir(orig)
    dobleRev := Invertir(rev)
    t.Logf("Numero de runas: orig=%d, rev=%d, dobleRev=%d", utf8.RuneCountInString(orig), utf8.RuneCountInString(rev), utf8.RuneCountInString(dobleRev))
    if orig != dobleRev {
      t.Errorf("Antes de invertir: %q, despues del invertir: %q", orig, dobleRev)
    }
    if utf8.ValidString(orig) && !utf8.ValidString(rev) {
      t.Errorf("Invertir no devolvio una cadena UTF-8 valida %q", rev)
    }
  })
}
```

Corremos nuevamente con la bandera -v `go test -v .`
```bash
go test -v .
=== RUN   TestInvertir
--- PASS: TestInvertir (0.00s)
=== RUN   FuzzInvertir
=== RUN   FuzzInvertir/seed#0
    main_test.go:33: Numero de runas: orig=10, rev=10, dobleRev=10
=== RUN   FuzzInvertir/seed#1
    main_test.go:33: Numero de runas: orig=1, rev=1, dobleRev=1
=== RUN   FuzzInvertir/seed#2
    main_test.go:33: Numero de runas: orig=6, rev=6, dobleRev=6
=== RUN   FuzzInvertir/ac96f6f1a42cb9a37e2d3e4c0a98c6d43339e291d7c8f715f7254b20f00e146c
    main_test.go:33: Numero de runas: orig=1, rev=2, dobleRev=1
    main_test.go:38: Invertir no devolvio una cadena UTF-8 valida "\x8e\xcf"
--- FAIL: FuzzInvertir (0.00s)
    --- PASS: FuzzInvertir/seed#0 (0.00s)
    --- PASS: FuzzInvertir/seed#1 (0.00s)
    --- PASS: FuzzInvertir/seed#2 (0.00s)
    --- FAIL: FuzzInvertir/ac96f6f1a42cb9a37e2d3e4c0a98c6d43339e291d7c8f715f7254b20f00e146c (0.00s)
FAIL
FAIL	ejemplo/fuzz	0.411s
FAIL
```

Podemos ver que los valores con los que poblamos nuestro corpus, todos son cadenas, en los cuales los carateres necesitan un solo byte, pero con el caracter ώ requiere mas, por lo que al intentar invertirlo byte por byte resulta en un caracter invalido. Asi que vamos a solucionar el error

#### Corrigiendo el error

La solución es simple, vamos invertirlo runa por runa en lugar de usar los bytes
```go {hl_lines=[2]}
func Invertir(s string) string {
	b := []rune(s)
	for i, j := 0, len(b)-1; i < len(b)/2; i, j = i+1, j-1 {
		b[i], b[j] = b[j], b[i]
	}
	return string(b)
}
```
Si volvemos a ejecutar nuestros test:
```bash
go test .
ok  	ejemplo/fuzz	0.549s
```
Nuestros test pasaron exitosamente. Muy bien! ahora tenemos que correr nuevamente nuestros fuzz test en busca de algun error o caso que no contemplamos.
```bash
go test -fuzz=Fuzz
fuzz: elapsed: 0s, gathering baseline coverage: 0/4 completed
fuzz: elapsed: 0s, gathering baseline coverage: 4/4 completed, now fuzzing with 12 workers
fuzz: minimizing 49-byte failing input file
fuzz: elapsed: 0s, minimizing
--- FAIL: FuzzInvertir (0.04s)
    --- FAIL: FuzzInvertir (0.00s)
        main_test.go:33: Numero de runas: orig=1, rev=1, dobleRev=1
        main_test.go:35: Antes de invertir: "\xdf", despues del invertir: "�"

    Failing input written to testdata/fuzz/FuzzInvertir/6d1afba479d1e743926c35fff31a09168a87c4c416f6d927c76d506f3c63ba08
    To re-run:
    go test -run=FuzzInvertir/6d1afba479d1e743926c35fff31a09168a87c4c416f6d927c76d506f3c63ba08
FAIL
exit status 1
FAIL	ejemplo/fuzz	0.512s
```
Vemos que ahora el error es causado porque la cadena no es la misma al invertirla por segunda vez, esto es debido a que la entrada no es un caracter UTF-8 valido.

## Arreglando doble inversion

Como habiamos mencionado, la entrada es un slice de bytes con un solo byte `\xdf`, por lo que al convertirlo a un `[]rune`, Go hace un encode a UTF-8 remplazando el byte por el siguiente caracter �. Vamos a agregar las siguientes lineas para obtener mas información.

```go {hl_lines=[2,4]}
func Invertir(s string) string {
  fmt.Printf("entrada: %q\n", s)   	
  b := []rune(s)
  fmt.Printf("runas: %q\n", b)
  for i, j := 0, len(b)-1; i < len(b)/2; i, j = i+1, j-1 {
    b[i], b[j] = b[j], b[i]
  }
  return string(b)
}
```

Vamos a correr nuestro fuzz test usando la bandera `-run` para correr unicamente el test que nos interesa inspeccionar.

```bash
go test -run=FuzzInvertir/6d1afba479d1e743926c35fff31a09168a87c4c416f6d927c76d506f3c63ba08
entrada: "\xdf"
runas: ['�']
entrada: "�"
runas: ['�']
--- FAIL: FuzzInvertir (0.00s)
    --- FAIL: FuzzInvertir/6d1afba479d1e743926c35fff31a09168a87c4c416f6d927c76d506f3c63ba08 (0.00s)
        main_test.go:33: Numero de runas: orig=1, rev=1, dobleRev=1
        main_test.go:35: Antes de invertir: "\xdf", despues del invertir: "�"
FAIL
exit status 1
FAIL	ejemplo/fuzz	0.257s
```
Como vemos, podemos confirmar que la entrada no es un carater unicode valido. Vamos a solucionar ese escenario. Si detectamos que la entrada es un caracter invalido regresaremos un error, tendremos que modificar la firma de nuestro metodo y hacer ajustes en nuestro codigo para soportar la actualizacion de nuestro metodo

```go {linenos=table,hl_lines=[4, 6, "11-12", "14-12", "14-15" ,"18-21", 26],linenostart=1}
package main

import (
	"errors"
	"fmt"
	"unicode/utf8"
)

func main() {
	input := "Anita lava la tina"
	rev, revErr := Invertir(input)
	dobleRev, dobleRevErr := Invertir(rev)
	fmt.Printf("original: %q\n", input)
	fmt.Printf("Invertida: %q err: %v\n", rev, revErr)
	fmt.Printf("Invertir una vez más: %q err: %v\n", dobleRev, dobleRevErr)
}

func Invertir(s string) (string, error) {
	if !utf8.ValidString(s) {
		return s, errors.New("entrada es un caracter UTF-8 invalido")
	}
	b := []rune(s)
	for i, j := 0, len(b)-1; i < len(b)/2; i, j = i+1, j-1 {
		b[i], b[j] = b[j], b[i]
	}
	return string(b), nil
}
```

De igual manera tendremos que modificar nuestros test y si encontramos un error saltar ese escenario.

```go {linenos=table,hl_lines=[17, "31-38"],linenostart=1}
package main

import (
	"testing"
	"unicode/utf8"
)

func TestInvertir(t *testing.T) {
	testcases := []struct {
		in, want string
	}{
		{"Hola mundo", "odnum aloH"},
		{" ", " "},
		{"!12345", "54321!"},
	}
	for _, tc := range testcases {
		rev, _ := Invertir(tc.in)
		if rev != tc.want {
			t.Errorf("Reverse: %q, want %q", rev, tc.want)
		}
	}
}

func FuzzInvertir(f *testing.F) {
	testcases := []string{"Hola mundo", " ", "!12345"}
	for _, tc := range testcases {
		// Añadimos el tipo de valores que queremos enviar a nuestro corpus
		f.Add(tc)
	}
	f.Fuzz(func(t *testing.T, orig string) {
		rev, err1 := Invertir(orig)
		if err1 != nil {
			t.Skip()
		}
		dobleRev, err2 := Invertir(rev)
		if err2 != nil {
			t.Skip()
		}
		t.Logf("Numero de runas: orig=%d, rev=%d, dobleRev=%d", utf8.RuneCountInString(orig), utf8.RuneCountInString(rev), utf8.RuneCountInString(dobleRev))
		if orig != dobleRev {
			t.Errorf("Antes de invertir: %q, despues del invertir: %q", orig, dobleRev)
		}
		if utf8.ValidString(orig) && !utf8.ValidString(rev) {
			t.Errorf("Invertir no devolvio una cadena UTF-8 valida %q", rev)
		}
	})
}
```

Podemos correr nuevamente nuestros test.

```bash
go test .
ok  	ejemplo/fuzz	0.546s
```

Bien, vemos que ya quedo corregido el escenario que no soportamos. Ahora volvamos a correr nuestros fuzz test. Los Fuzz test seguirar ejecutandose hasta encontrar algun error, de no ser asi podemos detenerlos con ctrl-c
```bash
go test -fuzz=Fuzz
fuzz: elapsed: 0s, gathering baseline coverage: 0/5 completed
fuzz: elapsed: 0s, gathering baseline coverage: 5/5 completed, now fuzzing with 12 workers
fuzz: elapsed: 3s, execs: 408367 (136118/sec), new interesting: 35 (total: 35)
fuzz: elapsed: 6s, execs: 840045 (143895/sec), new interesting: 36 (total: 36)
...
fuzz: elapsed: 48s, execs: 890470 (0/sec), new interesting: 36 (total: 36)
^Cfuzz: elapsed: 51s, execs: 890470 (0/sec), new interesting: 36 (total: 36)
PASS
ok  	ejemplo/fuzz	51.083s
```

Podemos correrlos con la bander `-fuzztime` para limitar el tiempo de ejecución
```bash
go test -fuzz=Fuzz -fuzztime 30s
fuzz: elapsed: 0s, gathering baseline coverage: 0/41 completed
fuzz: elapsed: 0s, gathering baseline coverage: 41/41 completed, now fuzzing with 12 workers
fuzz: elapsed: 3s, execs: 356810 (118815/sec), new interesting: 5 (total: 41)
fuzz: elapsed: 6s, execs: 463801 (35697/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 9s, execs: 470249 (2148/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 12s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 15s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 18s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 21s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 24s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 27s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 30s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
fuzz: elapsed: 31s, execs: 470249 (0/sec), new interesting: 6 (total: 42)
PASS
ok  	ejemplo/fuzz	31.508s
```

## Conclusion

Genial! Ahora sabemos sobre fuzz testing y como podremos trabajar con el en Go. Este fue una intruduccion simple a sus caracteristicas, pero sin duda es una herramienta muy util para encontrar fallas en nuestro codigo. Si tienes una duda o comentario no dudes en contactarme por alguna de mis redes

Este post esta basado y fue traducido de la documentacion original de go [Tutorial: Getting started with fuzzing](https://go.dev/doc/tutorial/fuzz).