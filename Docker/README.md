# Docker

## 1. Introducción

### ¿Qué es docker?

Docker utiliza las características de aislamiento de recursos del kernel de Linux como cgroups, espacios de nombres del kernel y OverlayFS, todo dentro de la misma máquina física o virtual. OverlayFS es un sistema de archivos con capacidad de unión que combina varios archivos y directorios en uno solo para poder ejecutar múltiples aplicaciones que están aisladas y contenidas entre sí, todo dentro de la misma máquina física o virtual.

El motor Docker proporciona una abstracción abstracción del sistema subyacente, lo que facilita a los desarrolladores probar su código

Docker resuelve el problema de despliegue y modernización de nuevas aplicaciones problema reduciendo drásticamente la superficie de la incertidumbre.  
¿Su aplicación se está modernizando? No hay problema. **Construya un nuevo contenedor con el nuevo código de la aplicación y las dependencias y envíelo.** La infraestructura existente sigue siendo la misma.   
Si la aplicación no se comporta como se espera, entonces retroceder es tan simple como volver a desplegar el contenedor anterior; no es raro que todas las imágenes Docker generadas generadas en un registro Docker. Tener una forma fácil de retroceder sin  alterar la infraestructura actual reduce drásticamente el tiempo necesario para responder a los fallos.

### Diferencia respecto a virtual machines

Mucha gente asume que como los contenedores aíslan las aplicaciones, son son lo mismo que las máquinas virtuales. A primera vista lo parece, pero la diferencia fundamental es que los contenedores comparten el mismo kernel que el host.

Docker sólo aísla un único proceso (o un grupo de procesos dependiendo de cómo se construya la imagen) y **todos los contenedores se ejecutan en el mismo sistema host**.  
Cuando se pone en marcha un contenedor, el proceso o grupo de procesos seleccionado sigue ejecutándose en el mismo host, sin necesidad de
virtualizar o emular nada.

![](img/img1.png)

En cambio, cuando se pone en marcha una **máquina virtual**, el hipervisor virtualiza un sistema completo, desde la CPU hasta la RAM y el almacenamiento. Para soportar este sistema virtualizado, **es necesario instalar un sistema operativo completo**.
A efectos prácticos, el sistema virtualizado es un ordenador entero que se ejecuta en un ordenador. Ahora bien, si puedes imaginar la cantidad de gastos generales que sistema operativo, ¡imagina cómo sería si ejecutaras un ¡sistema operativo anidado! La Figura muestra una representación de las tres aplicaciones que se ejecutan en tres máquinas virtuales diferentes en un único host físico.

![](img/img2.png)


## 4. Dockerfile

### ¿Qué es Dockerfile?
Para una aplicación tradicionalmente desplegada, construir y empaquetar una aplicación era a menudo bastante tediosa. Con el objetivo de automatizar la construcción y empaquetado de la aplicación, la gente recurrió a diferentes utilidades como GNU Make, maven, gradle, etc., para construir el paquete de la aplicación. Del mismo modo, en el mundo Docker, un Dockerfile es una **forma automatizada de construir tus imágenes Docker.**

El Dockerfile contiene **instrucciones especiales**, que indican al motor Docker Engine sobre los **pasos necesarios para construir una imagen**.  
Para invocar una construcción usando Docker, se emite el comando de Docker **`build`**.

Un archivo Dockerfile típico sería así:
```
FROM ubuntu:latest
LABEL author="sathyabhat"
LABEL description="An example Dockerfile"
RUN apt-get install python
COPY hello-world.py
CMD python hello-world.py
```

Mirando el Dockerfile, es fácil comprender lo que le estamos diciendo al motor Docker que construya. Sin embargo, no dejes que la simplicidad te engañe:  
el Dockerfile te permite construir condiciones complejas al generar tu imagen Docker. Cuando se emite un comando de construcción Docker, se construyen las imágenes Docker desde el Dockerfile dentro del contexto.

### Build Context

"Build Context" es un archivo o conjunto de archivos disponibles en una ruta o URL específica. Para entenderlo mejor, podemos tener algunos archivos de apoyo que necesitamos durante la construcción de una imagen Docker, por ejemplo, un archivo de configuración específico de la aplicación que se generó anteriormente y que necesita ser parte del contenedor.

"Build Context" puede ser local o remoto, incluso podemos establecer el "Build Context" a la URL de un repositorio Git, que puede ser útil si los archivos de origen no se encuentran en el mismo host que el demonio Docker o si queremos probar las ramas de características (feature branches). Simplemente establecemos el contexto en la rama. El comando de construcción tiene el siguiente aspecto:
```
docker build https://github.com/sathyabhat/sample-repo.
git#mybranch
```

Del mismo modo, para construir imágenes basadas en sus etiquetas Git, el comando de construcción se vería así:
```
docker build https://github.com/sathyabhat/sample-repo.
git#mytag
```

¿Trabajando en una característica a través de un pull request? ¿Quieres probar ese pull request?
No hay problema; incluso puedes establecer el contexto de una solicitud de extracción de la siguiente manera:
```
docker build https://github.com/sathyabhat/sample-repo.
git#pull/1337/head
```
**El comando `build` establece el contexto a la ruta o URL proporcionada**, subiendo los archivos al demonio Docker y permitiéndole construir la imagen. No estás limitado al contexto de construcción de la URL o la ruta. Si pasas una URL a un tarball remoto, el tarball de la URL se descarga en el demonio Docker y el comando de construcción se emite con eso como el contexto de construcción.

> **¡Atención!  **
> Si proporcionas el Dockerfile en el directorio raíz (/) y lo estableces como contexto, esto transferirá el contenido de tu disco duro al demonio Docker


### Dockerignore

Ahora deberías entender que el contexto de construcción transfiere el contenido del directorio actual al demonio Docker durante la construcción. Considere el caso en el que el directorio de contexto tiene un montón de archivos/directorios que no son relevantes para el proceso de construcción. La carga de estos archivos puede causar un aumento significativo del tráfico. **`Dockerignore`, al igual que gitignore, permite definir los archivos que están exentos de ser transferidos durante el proceso de construcción.**

La lista de ignorados es proporcionada por un archivo conocido como `.dockerignore` y cuando la CLI de Docker encuentra este archivo, modifica el contexto para excluir los archivos/patrones proporcionados en el archivo.  
Todo lo que comienza con una almohadilla (#) se considera un comentario y se ignora.  
Este es un ejemplo de archivo `.dockerignore` que excluye un directorio temporal, un directorio `.git`, y el directorio `.DS_Store` y el directorio `.DS_Store`:

```
*/temp*
.DS_Store
.git
```

### Building Using Docker Build

Volveremos al ejemplo de Dockerfile un poco más tarde. Intentemos primero un simple Dockerfile primero. Copia el siguiente contenido en un archivo y guárdalo como Dockerfile:

```
FROM ubuntu:latest
CMD echo Hello World!
```

Ahora, construye esa imagen
```
docker build .
```
Podrás ver una respuesta como esta:
```
Sending build context to Docker daemon 2.048kB
Step 1/2 : FROM ubuntu:latest
latest: Pulling from library/ubuntu
22dc81ace0ea: Pull complete
1a8b3c87dba3: Pull complete
91390a1c435a: Pull complete
07844b14977e: Pull complete
b78396653dae: Pull complete
Digest: sha256:e348fbbea0e0a0e73ab0370de151e7800684445c509d4619
5aef73e090a49bd6
Status: Downloaded newer image for ubuntu:latest
---> f975c5035748
Step 2/2 : CMD echo Hello World!
---> Running in 26723ca45a12
Removing intermediate container 26723ca45a12
---> 7ae54947f6a4
Successfully built 7ae54947f6a4
```

pag 57
-------