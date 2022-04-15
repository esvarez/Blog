---
title: "Curso Go: Instalando y configurando el entorno"
date: 2022-03-10T19:13:37-06:00
draft: false
categorías:
    - Tutorial
    - Golang
tags:
    - Tutorial
    - Go
    - Golang    
    - Básico
cover:
    image: "/img/instalar-go/go-logo.jpeg"
    alt: "go logo"    
    relative: false 
---

Para poder trabajar en **Go** es necesario configurar nuestro ambiente de desarrollo, en este post vamos a cubrir esos pasos, asi como diferentes formas para ejecutar nuestro código.

## Instalar Go
Para instalar Go, en el caso de Mac y Windows es muy sencillo, basta con ir a la [página oficial](https://go.dev/doc/install), descargar el paquete de acuerdo a tu sistema operativo, *.pkg* para mack y *.msi* para Windows.

En ambos casos al ejecutar el archivo descargado, la instalación nos hará la configuración por defecto de nuestras variables de entorno.

> También podemos instalar Go usando un gestor de paquetes como *Homebrew*, para Mac `brew install go` y *Chocolatey* para Windows `choco install golang`.

En el caso de Linux, tiene los archivos comprimidos. La siguiente lista de comando eliminará versiones anteriores de Go, instalará la versión que descargaste y configurará el path. 

```bash
$ rm -rf /usr/local/go 
$ tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
$ echo 'export PATH=$PATH:/usr/local/go/bin' >> $HOME/.profile
$ source $HOME/.profile
```

Para verificar que está configurado correctamente, ejecutar el siguiente comando:

```bash
$ go version
```

Si este comando devuelve la version y arquitectura del sistema, es que go fue instalado y configurado correctamente.

## Entorno de trabajo

Cuando hablamos de espacio de trabajo en go nos referimos a los directorios donde tendremos nuestro código y ejecutables, ya sean propios o de terceros, que serian nuestros directorios `$HOME/go/src` y `$HOME/go/bin`.

- src: Es el directorio donde tendremos el código de origen escrito en go.
- bin: Este directorio contiene los binarios escritos en go o las librerías que descarguemos.

Para sistemas Unix (Mac y linux) usamos el siguiente comando para configurar nuestro entorno de trabajo:

```bash
$ export GOPATH=$HOME/go
$ export PATH=$PATH:$GOPATH/bin
```

En windows, usamos el siguiente comando:

```bash
$ setx GOPATH %USERPROFILE%\go
$ setx path "%path%;%USERPROFILE%\bin"
```

## Comando Go

Al momento de instalar Go, este nos provee de varias herramientas para poder trabajar con el lenguaje. Para poder correr nuestro código, tenemos dos opciones `go run` o `go build`. Vamos a crear nuestro primer programa en Go y veamos como funcionan estos dos comandos.

Creamos una nueva carpeta y dentro de ella creamos un archivo llamado `hola.go`, vamos a escribir (o copiar 😛) el siguiente código:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hola mundo!")
}
```

### go run

Vamos a abrir nuestra consola dentro de la carpeta donde creamos nuestro programa, una vez guardado. 

```bash
go run hola.go
```

Vamos a ver en consola: `Hola mundo!`, a pesar de que go es un lenguaje compilado si observamos dentro de nuestra carpeta no hay ningún archivo binario. La forma en como `go run` funciona, crea el archivo binario en memoria y lo ejecuta. Este comando es muy util al momento de desarrollar o estar probando nuestras aplicaciones.

### go build

Una vez que nuestro programa está listo, lo más común es que querríamos crear nuestro archivo binario, en este caso usaremos el comando `go build hola.go`, el cual creara un archivo ejecutable `hola` (`hola.exe` en el caso de windows). 

Cuando usamos el comando `go build` nos creara un archivo binario con el nombre igual a nuestro archivo `.go`, si queremos controlar la salida debemos ejecutar el comando con la bandera `-o`, si queremos que nuestro ejecutable se llame "saludo", el comando a usar debería ser `go build -o saludo hola.go`.

```bash
go build -o saludo hola.go
```

¿Cómo te sientes después de escribir tus primeras líneas de código en go?

## Formateando nuestro código

Cuando trabajamos en equipo es muy común que nuestro código tenga diferentes formatos, debido al estilo de programar de cada uno de los integrantes. **Go** nos provee de herramienta que nos facilitan formatear y ayudarnos a seguir las mejores prácticas en nuestro código.

El comando `go fmt` nos permite formatear nuestro código para hacer más fácil su lectura, si queremos formatear todos nuestros archivos debemos usar `go fmt ./...`.

Es muy importante la estructura de nuestro código, ya que a pesar de que no escribamos el punto y coma `;`, go si lo necesita, pero este es agregado por el entorno de Go antes de compilar, es por eso que debemos seguir una buena estructura de nuestro código. Ejemplo:

```go
func main() {
  fmt.Println("Hola mundo!")
}
```

```go
func main() 
{
  fmt.Println("Hola mundo!")
}
```

A pesar de que ambos codigos parecen verse similar y pensaramos que tendrán el mismo resultado, la realidad es que uno nos marcará error, ya que Go pondra los puntos y comas de la siguiente manera:

```go
func main() {
  fmt.Println("Hola mundo!");
};
```

```go
func main();
{
  fmt.Println("Hola mundo!");
};
```

En el segundo ejemplo es mas claro porque el mal formato terminara por causarnos problemas al momento de compilar.

Otro comando bastante útil que nos ayuda a tener mejores prácticas es `go vet`, este comando nos ayuda a verificar si nuestro código tiene errores, si lo tiene nos muestra el error y nos permite corregirlos corriendo el siguiente comando `go vet ./...`.

## Editor de código

Para trabajar con go tenemos 2 herramientas bastante populares.

El editor de código [**VSCode**](https://code.visualstudio.com/) (Visual studio code) Es una herramienta gratuita que nos permite trabajar con el lenguaje si descargamos algunos plugins. Y el IDE [**GoLand**](https://www.jetbrains.com/go/download/#section=windows), desarrollado por JetBrains, la desventaja es que es de paga, aunque si ya tienes una licencia de IntelliJ, puedes adecuarlo a Go usando algunos plugins.

## Conclusion.

¡Felicidades! Ya tienes instalado y configurado go. Estas a punto de empezar en el bonito mundo de este lenguaje.

Si quieres saber más te recomiendo leer [los fundamentos de go](../curso-go-fundamentos)
