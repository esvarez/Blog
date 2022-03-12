---
title: "Instalar Go"
date: 2022-03-11T19:13:37-06:00
draft: true
---

Quise hacer este post para ser mas especifico en como instalar **Go** en cada uno de los sistemas opertativos y no tengas problemas en seguir cada uno de los post relacionados al lenguaje.

Al momento de instalar Go tenemos dos opciones, usar un administrador de paquetes (En lo personal, el modo que yo recomiendo) o descargar el paquete y hacer la configuracion manual. Te animo a que intentes usar un manejador de dependencias y te vayas familiarizando con la consola, la estaremos usando mucho. 🤓

## Instalar Go
Hay un paso general para los 3 sistemas operativos que es necesario ir a la [pagina oficial](https://go.dev/doc/install), descargar el paquete de acuerdo a tu sistema operativo. Ahora seguir las instrucciones de acuerdo a tu sistema operativo.

### Windows

Como es normal, en windows, descargaremos un ejecutable que tenemos que abrir, podemos elegir la ruta a instalar o si queremos modificarla.

Una vez que se completo la instalacion debemos cerrar nuestras lineas de comando para que tomen los cambios realizados. Abrimos la linea de comando de nuestra preferencia, `cmd` o `PowerShell`. Para abrir cualquiera de estas terminales tienes dos opciones:

  - En el menu de inicio ir a la barra de busqueda y escribir l nombre de la terminal que quieres abrir.
  - Usar el atajo `Windos + R`, en la ventana que se abre escribir `cmd` o `PowerShell` y precionar enter
	
Dentro de nuestra terminal escribirmos:

```bash
$ go version
```

Este comando nos debe devolver la version que tenemos instalada de go si todo se instalo correctamente.

### MacOS
Al abrir el paquete descargado es necesario seguir las instrucciones de instalacion, por defecto el paquete se instala en `/usr/local/go`. Para ver los cambios efectuados es necesario reiniciar o abrir la terminal de comandos y escribir el siguiente comando para verificar la instalcion.

```bash
$ go version
```

Este comando nos debe devolver la version que tenemos instalada de go si todo se instalo correctamente.

Ahora vamos a crear nuestro espacio de trabajo, desde la terminal vamos a nuestro *HOME*.

```bash
$ cd ~
```

Dentro vamos a crear el siguiente directorio `go/src` y `go/bin`
  - `src`: Es el directorio donde tendremos nuestro codigo de origen escrito en go.
  - `bin`: Este directorio contiene los ejecutables de nuestro codigo escrito en go o librerias que descarguemos.

```shell
$ mkdir -p ~/go/{bin,src}
```

#### Configurar GOPATH

El GOPATH, en el paso anterior creamos nuestro GOPATH ahora es necesario hacerle saber a go en donde esta, con nuestras variables de entorno.

```bash
$ export GOPATH=$HOME/go
```

Al momento de instalar dependencias, los ejecutables se guardaran, como ya mencionamos, en `~/go/bin` por lo que tenemos que agregar esta ruta nuestro *path* para poder ejecutarlos.

```bash
$ export PATH=$PATH:$GOPATH/bin
```

Estas dos lineas las debemos agregar a nuestro `~/.profile` o en el archivo donde tengamos la condiguracion de nuestro *bash*

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Para actualizar los cambios escribimos.

```bash
$ . ~/.profile
```

### Linux

## Estructura

Ahora que tenemos instalado go en nuestro sistema operativo, la forma en como debemos trabajar, es colocar nuestro codigo en la carpeta correspondiente.

```
$GOPATH/src/github.com/usuario/proyecto
```

En mi caso seria, por ejemplo:

```
$GOPATH/src/github.com/esvarez/blog
```

Si vamos a usar un pakete de terceros, por ejemplo el framework `gin`, de la organizacion `gin-gonic` al descargarlo se guardará en:
```
$GOPATH/src/github.com/gin-gonic/gin
```

Si aun no tienes una cuenta en `github.com` recomiendo crearte una. 

## Primer programa

Ahora si estamos listos para escribir nuestro primer programa, vamos a nuestro directorio principal y creamos un archivo llamado `hola.go`

Dentro de el pondremos el siguiente codigo.

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

¿Cómo te sientes despues de escrbir tus primeras lineas de codigo?

## Conclusion.

!Felicidades! ya tienes instalado y configurado go. Estas a punto de empezar en el bonito mundo de este lenguaje.

Si quieres saber más te recomiendo [los fundamentos de go](./curso-go-fundamentos.md)