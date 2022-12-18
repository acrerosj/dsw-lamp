# dsw-lamp
**Contenedores LAMP para multisitio**

> Entorno usado en el módulo DSW del ciclo de Desarrollo de Aplicaciones Web.

Este repositorio es un docker composer con tres contenedores para alojar:
- Los sitios web (Apache)
- Las bases de datos (MySQL)
- La aplicación phpMyAdmin para administrar las bases de dados

## Requisitos
Tener instalado docker y docker-composer

## Explicación de las carpetas
- **conf**  : archivos de configuración
  - **mysql** : archivos de configuración de MySQL.
  - **php** : archivos de configuración de PHP.
  - **sites** : configuraciones de los host virtuales de apache.
- **databases** : Aquí se pueden poner los sql que se importarán automáticamanente al crear el contenedor. 
  - myDB.sql : Por ejemplo, crear una base de datos para probar el sitio por defecto.
- **www** : Carpetas de las raices de los sitios webs.
  - **html** : Sitio web por defecto. 
- *Dockerfile* : Configuración de los contenedores.
- *README.md* : Este archivo
- *docker-compose.yml* : Configuración del docker-composer para la creación y vinculación de los contenedores.
- *php* : Ejecutable que enlaca las extensiones de PHP del Visual Studio Code con la ruta del contenedor donde está el interprete PHP.

## Pasos a seguir para crear un nuevo sitio web.

Supongamos que el nuevo sitio que vamos a crear se va a llamar **nuevositio**.

### 1. Crear la carpeta para el nuevo sitio web en *www*

Se puede crear directamente una carpeta (*nuevositio*) o bien crearla a través de clonar un repositorio.

### 2. Crear la configuración del nuevo sitio web.
Crear un nuevo archivo de configuración (*nuevositio.conf*) en la carpeta *conf/sites*. Para ello podemos duplicar el *000-default.conf* y modificarlo
Modificar las siguentes líneas:
```
9   ServerName www.nuevositio.es

12  DocumentRoot /var/www/nuevositio

29	<Directory /var/www/nuevositio>
```

### 3. Insertar el nombre del dominio en el HOST

Para que pueda resolver las DNS y enviar al contenedor las peticiones cuando se acceda a *www.nuevositio.es* vamos a modificar el archivo de **hosts** local.

```
En Ubuntu, debes ejecutar en un terminal el comando: 

sudo nano /etc/hosts


En el archivo, inserta el siguiente ruta:

127.0.0.1   www.nuevositio.es
```

### 4. Activar el sitio web en apache.

Debes entrar en el terminal del contenedor donde se encuentra el apache.
> Para ello lo mejor es desde Visual Studio Code, con la extensión de Docker, arrancar los contenedores y con el botón derecho en dsw-lamp_www lanzar **"Attach Shell"**

Una vez en la terminal del contenedor, se deben ejecutar los siguientes comandos:
```
# a2ensite nuevositio.conf
# service apache2 restart
```
  
