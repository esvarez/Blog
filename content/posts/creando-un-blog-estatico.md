---
title: "Creando Un Blog Estático"
date: 2022-02-02T18:17:42-06:00
draft: false
categories:
    - Tech
tags:
    - Tech
    - Go
    - Servers
cover:
    image: "/img/creando-un-blog-estatico/cover.jpeg"
    alt: "Hello world"    
    relative: false     
---

# Creando un blog estático

Quizás has tenido en mente la idea de crear un blog, las razones pueden ser varias y distintas dependiendo de la persona. Pero siempre hay algo que nos detiene, ya sea el no atrevernos a dar ese primer paso, el no saber como hacer un blog e imaginarnos la cantidad enorme de trabajo que hay detrás nos asusta. Pero la realidad es que hay muchas alternativas, desde páginas que te permiten tener tu perfil, hasta herramientas que te crear tu blog en minutos y montarlo sobre un dominio, incluso, ¿Porque no? crear tu propio blog, si estas en el mundo de la tecnología al igual que yo, seguramente más de una vez te paso esta idea por la cabeza.

Hoy te mostrare una opción intermedia, sin la necesidad de hacer todo desde cero, pero teniendo la libertad de hacer los ajustes necesarios. Un blog **estático**.

### Problema

Al momento de crear un blog debes de tener varias cosas en cuenta:

- **Lenguaje de desarrollo:** Aquí tenemos un montón de posibilidades para elegir, ya sea por el deseo de aprender un nuevo lenguaje o seguir practicando alguno, eres libre de elegir el prefieras  
- **Servidor o hosting:** Al momento de montar nuestro blog solo tenemos dos opciones. Un hosting compartido, con la desventaja de limitar nuestras opciones de desarrollo y almacenamiento, pero ahorrándonos trabajo de configuraciones o un servidor privado, en el cual somos libre de elegir las tecnologías que más nos gusten, pero con la necesidad de invertir tiempo en su configuración.  
- **Base de datos:** Toda aplicación requiere un lugar donde almacenar su información y un blog no es la excepción
- **Guardar texto formateado:** Al momento de guardar nuestros post en una base de datos es necesario ver la forma de guardar ese texto formateado y saber que se respetara el formato una vez que consultemos la información, si bien hay herramientas que ayudan en esa tarea, siempre es una parte del desarrollo que toma su tiempo.

En mi caso estaba buscando una alternativa y ver estos cuatro problemas fue lo que me motivo a elegir un sitio estático, si bien también tiene sus desventajas en mi caso me aportaba más ventajas que desventajas. Si en este punto te estas preguntando que es un blog o sitio estático te lo voy a explicar. 

### Solución

Un sitio estático es en el cual no se consulta la información desde una base datos y todo lo que vemos son archivos, así es, cada nuevo post es un archivo *.html*, en este punto eso podría parecer algo tedioso y más difícil de manejar, en principio lo es, pero en mi caso decidí usar una herramienta llamada Hugo que facilita esa tarea, de esta manera resolví el problema de: la base de datos, el lenguaje y el texto formateado, ya que Hugo soporta sintaxis **Markdown** así sé exactamente lo que estoy "guardando".

Si bien no hay solución perfecta, en este caso esto fue lo mejor para mí, porque estaba buscando una opción rápida y flexible. 

#### Links 

- [Hugo](https://gohugo.io/) Framework para crear sitios estáticos
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod/): Tema de blog
