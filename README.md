# Imágenes Docker
### Ministerio de Salud de El Salvador (MINSAL)



<div align="center">
	<a href="http://codigo.salud.gob.sv/">
		<img alt="SUIS" title="SUIS" src="https://next.salud.gob.sv/index.php/s/qjCqXR7eCBJciLY/preview" width="450" style="width: 450px;">
	</a>
</div>



## Tabla de Contenido

* [Descripción](#descripción)
* [Instalación](#instalación)
* [Forma de uso](#forma-de-uso)
* [Colaboradores](#colaboradores)
* [Enlaces de ayuda](#enlaces-de-ayuda)
* [Licencia](#licencia)



## Descripción

Este repositorio tiene por objetivo brindar los archivos **Dockerfile**, necesarios
para la creación de las Imágenes Docker, requeridas para el funcionamiento de la
plantilla oficial utilizada en el desarrollo de sistemas informáticos propiedad del
Ministerio de Salud.



## Instalación

A continuación se listan los pasos para la instalación del software necesario para el correcto funcionamiento del repositorio Docker:

### 1 Requerimiento de Software

| Software | Versión |
| -------- | ------- |
| Docker   | \>=18.x |
| Git      | \>=2.20 |


### 2 Instalación de Docker
Los pasos de instalación de Docker se describen en el siguiente enlace: [**Clic aquí**](https://docs.docker.com/install/linux/docker-ce/debian/), se recomienda la configuración del uso de Docker como usuario no root en Linux.

### 3 Instalación de Git

Los pasos para la instalación de Git se describen en el siguiente enlace: [**Clic aquí**](https://git-scm.com/book/es/v1/Empezando-Instalando-Git).


### 4 Clonación del Proyecto

Para clonar el proyecto ejecutar el siguiente comando como usuario normal:

**Repositorio Interno (MINSAL)**

```bash
git clone https://codigo.salud.gob.sv/plantillas/docker.git
```

**Repositorio Público**

```bash
git clone https://github.com/klb-rodriguez/docker.git
```

### 5 Creación de la Imagen

Dirigirse al directorio de la imagen que se desea crear, por ejemplo si se desea crear la imagen
de apache utilizando php 7.0 ingresar al directorio `Dockerfile/php/7.0`, si en cambio se desea crear la imagen de apache utilizando php 5.6 ingresar al directorio `Dockerfile/php/5.6`, desde el directorio
raíz del proyecto clonado y ejecutar el siguiente comando.

**Para Apache y php 7.0**
```bash
docker build -t php:7.0-dtic .
```
**Para Apache y php 5.6**
```bash
docker build -t php:5.6-dtic .
```
**Nota**: *No omitir el punto al final del comando.*

### 6 Comprobación de la creación de la Imagen
Para comprobar que la imagen se ha creado exitosamente ejecutar el siguiente comando:

```bash
docker images
```
Tendrá que aparecer una imagen con el nombre php:7.0-dtic o php:5.6-dtic, algo similiar a lo siguiente ( el IMAGE ID, CREATED y SIZE pueden variar).

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
php                 7.0-dtic            128c7f78f17c        18 seconds ago      746MB
php                 5.6-dtic            f3be9430ca74        5 minutes ago       509MB
```



## Forma de uso

A continuación se describen los pasos para el uso de las imágenes Docker:

### 1 Crear el contenedor

Para crear el contenedor a partir de la imagen es necesario irse al directorio raíz de donde se tiene el
proyecto que se desea correr y ejecutar el siguiente comando:

```bash
docker run -d --name <nombre_contenedor> -p <puerto>:80 -v "$PWD":/var/www/html/ -v "$PWD"/../apache_log/:/var/log/apache2/ <repositorio>:<tag>
```

En donde:

* **&lt;nombre_contenedor&gt;**: Es el nombre que se le dará al contenedor a crear.
* **&lt;puerto&gt;**: Es el puerto local por el cual se accederá al puerto 80 del contenedor,
dicho puerto puede ser cualquier número de puerto válido, solamente es de tener en cuenta
de que dicho puerto no este siendo usado por otra aplicación.
* Además debe de existir la carpeta **apache_log** fuera del directorio raíz del proyecto para
que en ese directorio se almacenen los logs de apache del contenedor, la ruta se puede cambiar
por otra que sea de su conveniencia.
* **&lt;repositorio&gt;**: Es el nombre del repositorio de docker en este caso como ambas son iamgenes
php el nombre es php debido a que cuando se crearon las imágenes se nombraron con ese repositorio.
* **&lt;tag&gt;**: Es el nombre que se le da a la version del repositorio para nuestro caso puede se 7.0-dtic o 5.6-dtic, ahí dependerá de que imagen se haya creado y de cuál se quiera generar el contenedor.

Ejemplo Pŕactico:

**Para Apache y php 7.0**
```bash
docker run -d --name app1 -p 90:80 -v "$PWD":/var/www/html/ -v "$PWD"/../apache_log/:/var/log/apache2/ php:7.0-dtic
```
**Para Apache y php 5.6**
```bash
docker run -d --name app2 -p 91:80 -v "$PWD":/var/www/html/ -v "$PWD"/../apache_log/:/var/log/apache2/ php:5.6-dtic
```

### 2 Comprobación del contenedor

Hay que verificar que no haya errores al crear el contenedor a partir de la imagen, para ello hay que ejecutar el comando siguiente:

```bash
docker ps
```

Mostrando algo similar a lo siguiente:
```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS             PORTS                 NAMES
2e5ce1a467ad        php:7.0-dtic        "docker-php-entryp..."   8 seconds ago       Up 7 seconds       0.0.0.0:90->80/tcp    app1
a2c97f7eef06        php:5.6-dtic        "docker-php-entryp..."   8 seconds ago       Up 7 seconds       0.0.0.0:91->80/tcp    app2
```

Si aparece el contenedor con el nombre que se le asigno al crearlo, indica que todo se ha ejecutado
correctamente, si no aparece verificar si se cerró el contenedor al nomás crearse con el comando **docker ps -a**.

### 3 Acceder al Servidor Web

Una vez que se ha creado el contenedor, se puede acceder al aplicativo desde el navegador web, solo basta con ejecutar la ruta siguiente: **http://localhost:puerto**, en donde **puerto** es el número de puerto que se le dio para acceder al contenedor en el [**paso 1**](#1-crear-el-contenedor).



## Colaboradores
El proyecto es de propiedad intelectual del Ministerio de Salud de El Salvador y ha sido desarrollado en colaboración con las siguientes personas:

<div align="center">
    <table>
        <tr>
            <td align="center">
                <div align="center">
                    <a href="http://codigo.salud.gob.sv/crorozco"  target="_blank"><img  style="width: 90px; height: 90px;" width="90" src="http://codigo.salud.gob.sv/uploads/-/system/user/avatar/8/avatar.png"></a><br />
                    Caleb Rodriguez<br/>
                    <a href="mailto:crorozco@salud.gob.sv">crorozco@salud.gob.sv</a>
                </div>
            </td>
            <td align="center">
                <div align="center">
                    <a href="http://codigo.salud.gob.sv/vmoran"  target="_blank"><img  style="width: 90px; height: 90px;" width="90" src="https://www.gravatar.com/avatar/4f8e2e1f185a6a1903c359e022f11f52?s=180&d=identicon"></a><br />
                    Vladimir Morán<br/>
                    <a href="mailto:vmoran@salud.gob.sv">vmoran@salud.gob.sv</a>
                </div>
            </td>
        </tr>
    </table>
</div>
<div align="center">
    <b>Dirección de Tecnologías de Información y Comunicaciones (DTIC).</b><br />
    <b>Ministerio de Salud</b><br />
    <a href="http://www.salud.gob.sv" alt="minsal" target="_blank">www.salud.gob.sv</a>
</div>


## Enlaces de ayuda
A continuación se presentan enlaces externos de ayuda referentes a tecnologías utilizadas para el desarrollo del proyecto:

* Gestor de contenedores [Docker](https://docs.docker.com/).
* Gestor de control de cambios [Git](https://git-scm.com/doc).


## Licencia
<a rel="license" href="https://www.gnu.org/licenses/gpl-3.0.en.html"><img alt="Licencia GNU GPLv3" style="border-width:0" src="https://next.salud.gob.sv/index.php/s/qxdZd5iwcqCyJxn/preview" width="96" /></a>

Este proyecto está bajo la <a rel="license" href="http://codigo.salud.gob.sv/plantillas/docker/blob/master/LICENSE">licencia GNU General Public License v3.0</a>