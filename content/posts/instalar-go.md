---
title: "Instalar Go"
date: 2022-03-11T19:13:37-06:00
draft: false
categories:
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

Quise hacer este post para ser más especifico en como instalar **Go** en cada uno de los sistemas operativos y no tengas problemas en seguir cada uno de los post relacionados con el lenguaje.

## Instalar Go
Hay un paso general para los 3 sistemas operativos, es necesario ir a la [pagina oficial](https://go.dev/doc/install), descargar el paquete de acuerdo a tu sistema operativo.

### Windows

Como es normal, en windows, descargaremos un ejecutable que tenemos que abrir, podemos elegir la ruta a instalar o si queremos modificarla.

Una vez que este completa la instalación debemos cerrar nuestras líneas de comando para que tomen los cambios realizados. Abrimos la línea de comando de nuestra preferencia, `cmd` o `PowerShell`. Para abrir cualquiera de estas terminales tienes dos opciones:

  - En el menu de inicio ir a la barra de búsqueda y escribir el nombre de la terminal que quieres abrir.
  - Usar el atajo `Windos + R`, en la ventana que se abre escribir `cmd` o `PowerShell` y presionar enter
	
Dentro de nuestra terminal escribiremos:

```bash
$ go version
```

Este comando nos debe devolver la version que tenemos instalada de go si todo se instalo correctamente.

#### Configurar espacio de trabajo (workspace)

Cuando hablamos de espacio de trabajo en `go` nos referimos a los directorios donde tendremos nuestro código y ejecutables, que serian nuestros directorios `go/src` y `go/bin`
  - `src`: Es el directorio donde tendremos nuestro código de origen escrito en go.
  - `bin`: Este directorio contiene los ejecutables de nuestro código escrito en go o las librerías que descarguemos.  
Los cuales crearemos dentro de nuestro directorio *HOME*

```bash
$ cd ~
```

Creamos las carpetas

```bash
mkdir go/bin, go/src
```

Si usamos el instalador nuestro `GOPATH` ya debe de estar instalado, para comprobarlo escribiremos.

```bash
$ go env
```

Nos tiene que dar un resultado similar:

```bash {linenos=table,hl_lines=[2]}
set GOOS=windows
set GOPATH=C:\Users\usuario\go
set GOPRIVATE=
set GOPROXY=https://proxy.golang.org,direct
set GOROOT=C:\Program Files\Go
set GOSUMDB=sum.golang.org
```

Agregamos a nuestro path los programas que compilemos de GO

```bash
setx PATH "$($env:path);$GOPATH\bin"
```

### MacOS
Al abrir el paquete descargado es necesario seguir las instrucciones de instalación, por defecto el paquete se instala en `/usr/local/go`. Para ver los cambios efectuados es necesario reiniciar o abrir la terminal de comandos y escribir el siguiente comando para verificar la instalación.

```bash
$ go version
```

Este comando nos debe devolver la version que tenemos instalada de go si todo se instalo correctamente.

Ahora vamos a crear nuestro espacio de trabajo, desde la terminal vamos a nuestro *HOME*.

```bash
$ cd ~
```

Dentro vamos a crear el siguiente directorio `go/src` y `go/bin`
  - `src`: Es el directorio donde tendremos nuestro código de origen escrito en go.
  - `bin`: Este directorio contiene los ejecutables de nuestro código escrito en go o librerías que descarguemos.

```shell
$ mkdir -p ~/go/{bin,src}
```

#### Configurar espacio de trabajo (workspace)

El GOPATH, en el paso anterior creamos nuestro GOPATH ahora es necesario hacerle saber a go en donde esta, con nuestras variables de entorno.

```bash
$ export GOPATH=$HOME/go
```

Al momento de instalar dependencias, los ejecutables se guardaran, como ya mencionamos, en `~/go/bin` por lo que tenemos que agregar esta ruta nuestro *path* para poder ejecutarlos.

```bash
$ export PATH=$PATH:$GOPATH/bin
```

Estas dos líneas las debemos agregar a nuestro `~/.profile` o en el archivo donde tengamos la configuración de nuestro *bash*

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Para actualizar los cambios escribimos.

```bash
$ . ~/.profile
```

### Linux


Extraemos el archivo descargado en `/usr/local` con el siguiente comando:

```bash
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
```

Agregamos la ruta ` /usr/local/go/bin` a nuestras variables de ambiente

```bash
export PATH=$PATH:/usr/local/go/bin
```

Este cambio se vera reflejado hasta que reiniciemos nuestra terminal, una vez que lo hagamos verificamos nuestra instalación de Go, el siguiente comando nos debe imprimir nuestra version actual:

```bash
$ go version
```

#### Configurar espacio de trabajo (workspace)

Ahora vamos a crear nuestro espacio de trabajo, desde la terminal vamos a nuestro *HOME*.

```bash
$ cd ~
```

Dentro vamos a crear el siguiente directorio `go/src` y `go/bin`
  - `src`: Es el directorio donde tendremos nuestro código de origen escrito en go.
  - `bin`: Este directorio contiene los ejecutables de nuestro código escrito en go o librerías que descarguemos.

```shell
$ mkdir -p ~/go/{bin,src}
```

El GOPATH, en el paso anterior creamos nuestro GOPATH ahora es necesario hacerle saber a go en donde esta, con nuestras variables de entorno.

```bash
$ export GOPATH=$HOME/go
```

Al momento de instalar dependencias, los ejecutables se guardaran, como ya mencionamos, en `~/go/bin` por lo que tenemos que agregar esta ruta nuestro *path* para poder ejecutarlos.

```bash
$ export PATH=$PATH:$GOPATH/bin
```

Estas dos líneas las debemos agregar a nuestro `~/.profile` o en el archivo donde tengamos la configuración de nuestro *bash*

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Para actualizar los cambios escribimos.

```bash
$ . ~/.profile
```

## Estructura

Ahora que tenemos instalado go en nuestro sistema operativo, la forma en como debemos trabajar, es colocar nuestro código en la carpeta correspondiente.

```
$GOPATH/src/github.com/usuario/proyecto
```

En mi caso seria, por ejemplo:

```
$GOPATH/src/github.com/esvarez/blog
```

Si vamos a usar paquetes de terceros, por ejemplo el framework `gin`, de la organización `gin-gonic` al descargarlo se guardará en:
```
$GOPATH/src/github.com/gin-gonic/gin
```

Si aún no tienes una cuenta en `github.com` recomiendo crearte una. 

## Primer programa

Ahora si estamos listos para escribir nuestro primer programa, vamos a nuestro directorio principal y creamos un archivo llamado `hola.go`

Dentro escribiremos el siguiente código.

```go
package main

import "fmt"

func main() {
	fmt.Println("Hola mundo!")
}
```

Al correrlo con el comando

```bash
$ go run hola.go
```

Debemos obtener la salida

```bash
Hola mundo!
```

¿Cómo te sientes después de escribir tus primeras líneas de código?

## Conclusion.

!Felicidades! ya tienes instalado y configurado go. Estas a punto de empezar en el bonito mundo de este lenguaje.

Si quieres saber más te recomiendo [los fundamentos de go](../curso-go-fundamentos)
