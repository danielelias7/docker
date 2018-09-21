# **Repositorio de Imagenes Docker**

## **Ministerio de Salud de El Salvador**

&nbsp;

Este repositorio tiene por objetivo brindar los archivos **Docerfile**, necesarios
para la creación de las Imágenes Docker, requeridas para el funcionamiento de la
plantilla oficial utilizada en el desarrollo de sistemas informáticos propiedad del
Ministerio de Salud.

## **1) Requerimiento de Software.**
Docker 17.x o superior

&nbsp;
## **2) Instalación de Docker.**
Los pasos de instalación de docker se describen en el siguiente enlace: [**Clic aquí**][1].

&nbsp;
*minsal
 comandos según sistema operativo*

&nbsp;
## **3) Creación de la imagen.**

A continuación se listan los pasos para la creación de la imagen:

### **3.1) Clonación del Proyecto.**

Para clonar el proyecto ejecutar el siguiente comando como usuario normal:

```bash
git clone https://git.salud.gob.sv/PLANTILLAS/docker.git
```

### **3.2) Creación de la Imagen.**
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

### **3.3) Comprobación de la creación de la Imagen**
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
&nbsp;
## **4) Crear contenedor.**

### **4.1) Crear el contenedor**

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

### **4.2) Comprobación del contenedor**

Hay que verificar que no haya errores al crear el contenedor a partir de la imagen, para ello hay que ejecutar el comando siguiente:

```bash
docker ps
```

Mostrando algo similar a lo siguiente:
```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
2e5ce1a467ad        php:7.0-dtic        "docker-php-entryp..."   8 seconds ago       Up 7 seconds       0.0.0.0:8085->80/tcp    app1
a2c97f7eef06        php:5.6-dtic        "docker-php-entryp..."   8 seconds ago       Up 7 seconds       0.0.0.0:8086->80/tcp    app2
```

Si aparece el contenedor con el nombre que se le asigno al crearlo, indica que todo se ha ejecutado
correctamente, si no aparece verificar si se cerró el contenedor al nomás crearse con el comando **docker ps -a**.

&nbsp;
## **5) Acceder al Servidor Web.**

Una vez que se ha creado el contenedor, se puede acceder al aplicativo desde el navegador web, solo basta con ejecutar la ruta siguiente: **http://localhost:&lt;puerto>**, en donde **&lt;puerto&gt;** es el número de puerto que se le dio para acceder al contenedor en el paso 4.1.

[1]: https://docs.docker.com/engine/installation/linux/debian/#install-using-the-repository
