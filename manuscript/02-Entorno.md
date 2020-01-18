# Preparar el entorno de desarrollo
Puedes lanzar una aplicación desarrollada con Laravel en cualquier máquina o servidor con [Apache](https://httpd.apache.org/)/Nginx, [PHP](https://www.php.net/) y [Composer](https://getcomposer.org/). Para facilitar la creación del entorno de desarrollo, utilizaremos [Laravel Homestead](https://github.com/laravel/homestead). Homestead es una Vagrant box que provee de un entorno de desarrollo con todo el software necesario: PHP, servidor web, base de datos, gestor de dependencias, etc.

No obstante, si te sientes más cómodo utilizando tu propio servidor o entorno de desarrollo, puedes hacerlo perfectamente siempre que cumplas con los requerimientos citados.

En esta sección veremos paso a paso como preparar nuestro entorno de desarrollo con Homestead.

#### 1. Instalar VirtualBox 6.x
Descarga VirtualBox desde la página web oficial [www.virtualbox.org](https://www.virtualbox.org/) e instálalo en tu ordenador. También puedes utilizar VMWare, Parallels o Hyper-V en lugar de Virtual Box.

#### 2. Instalar [Vagrant](https://www.vagrantup.com/downloads.html).
Descarga e instala Vagrant tal y como lo indica la documentación de la página web oficial [https://www.vagrantup.com]((https://www.vagrantup.com/downloads.html)).

#### 3. Añadir laravel/homestead box a tu instalación de Vagrant
Una Vagrant box es una imagen base utilizada para clonar de forma rápida y sencilla una máquina virtual. Ejecuta el siguiente comando para descargar e instalar la Vagrant box the Larabel y así poder utilizarla para crear tantos entornos como quieras.

```
vagrant box add laravel/homestead
```

Puedes comprobar que se ha añadido correctamente utilizando el comando `vagrant box list`.

#### 4. Instalar Homestead
Clona el repositorio oficial de Homestead en un directorio. Como recomendación, puedes clonarlo en una carpeta llamada Homestead en la raíz de tu sistema, y utilizar esta máquina virtual para todos tus proyectos de Laravel.

```
git clone https://github.com/laravel/homestead.git ~/Homestead
```

#### 5. Crea el archivo de configuración Homestead.yaml
Ejecuta el comando init.sh en un terminal dentro del directorio donde clonaste Homestead. Este comando creará el archivo de configuración Homestead.yaml.

```
// Mac / Linux...
bash init.sh

// Windows...
init.bat
```

El archivo Homestead.yaml creado tendrá el siguiente aspecto:

```
ip: "192.168.10.10"
memory: 2048
cpus: 2
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/code
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/public

databases:
    - homestead
```

#### 6. Genera las claves ssh
Si no dispones de unas clasves ssh en el sistema necesitarás generarlas desde un terminal. Para saber si ya dispones de ellas, intanta encontrar el directorio `.ssh` en tu sistema operativo (normalmente suele encontrarse en C:\Users\USER_NAME en Windows y el el directorio raíz ~ en Linux/Mac ) y busca dos archivos llamados `id_rsa` y `id_rsa.pub`. Si los has encontrado, puedes saltarte este paso. En caso contrario, lanza el siguiente comando desde la consola:

```
ssh-keygen -t rsa -C "your_email@example.com"
```

El script te realizará algunas preguntas, simplemente pulsa ENTER y te creará los archivos `id_rsa` y `id_rsa.pub` en un directorio con nombre `.ssh`.

#### 7. Configurar Homestead
En esta instalación no tendrás que tocar el archivo `Vagrantfile` (probablemente estés familiarizado con él ya que es típicamente el utilizado para configurar el entorno). Homestead delega la configuración en el archivo `Homestead.yaml`,  por lo que a continuación te mostraremos las opciones más importantes que tendrás que configurar:

- **ssh keys**: el fichero de configuración Homestead.yaml tendrá la configuración realizada correctamente tanto para Mac como Linux. Si estás utilizando Windows, modifica los siguientes valores:

```
authorize: c:/Users/USER_NAME/.ssh/id_rsa.pub

keys:
    - c:/Users/USER_NAME/.ssh/id_rsa
```

- **Shared folders**: en este apartado se indican los directorios de la máquina local que se mantendrán sincronizados con la máquina virtual creada. Modifica el valor de 'map' y escribe la carpeta de tu proyecto (p.ej. `/Users/USER_NAME/dev/my-project` en Linux/Mac o `c:/dev/my-project` en Windows).
```
folders:
    - map: ~/code/project1
      to: /home/vagrant/project1

    - map: ~/code/project2
      to: /home/vagrant/project2
```
- **Sites**: La propiedad sites permite mapear fácilmente un dominio con un directorio de nuestro entorno virtual. De este forma podremos utilizar el dominio indicado para acceder a nuestra aplicación desde el navegador:
```
sites:
    - map: homestead.test
      to: /home/vagrant/project1/public
    - map: miapp.test
      to: /home/vagrant/project2/public
```

#### 8. Configurar el archivo local hosts
Es probable que vayas a utilizar el entorno virtual creado para múltiples proyectos o aplicaciones, por lo que necesitarás añadir los dominios indicados en el anterior apartado de 'sites' al archivo `hosts` de tu ordenador. De esta forma puedes redirigir las peticiones a dominios concretos a aplicaciones de tu entorno virtual Homestead.

Modifica el archivo `hosts` (lo encontrarás en `/etc/hosts` en Mac/Linux y en `C:\Windows\System32\drivers\etc\hosts` en Windows):

```
192.168.10.10  homestead.test
192.168.10.10  miapp.test
```

Para evitar otro tipo de problemas, la recomendación general es utilizar dominios de tipo ".localhost", ".invalid", ".test", or ".example".

#### 9. Arrancar y prueba el entorno creado
Una vez ya tenemos Homestead configurado y el archivo `hosts` modificado, ya podemos ir al directorio `Homestead` y ejecutar el siguiente comando para arranchar la máquina virtual (la primera vez tardará más tiempo al tener que realizar la preparación del entorno):

```
vagrant up
```

Puedes acceder mediante SSH a tu máquina virtual:

```
vagrant ssh
```

¡Enhorabuena! Ya estás preparado para comenzar a crear tu primera aplicación web con Laravel.


Puedes detener la máquina virtual con el comando `vagrant halt`.