# Aprende Laravel 6: guía paso a paso :rocket:
[![Open Source Love png2](https://badges.frapsoft.com/os/v2/open-source.png?v=103)](https://www.jonvadillo.com) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://www.jonvadillo.com) [![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://www.jonvadillo.com)

Esto es una guía práctica para aprender a paso a paso a desarrollar aplicaciones web con Laravel 6.

## Table of Contents
- [Introducción](#introduction)
  * [Prerequisitos](#prerequisitos)
  * [Conceptos básicos](#conceptos-b-sicos)
  * [Preparar el entorno de desarrollo](#preparar-el-entorno-de-desarrollo)
- [Aprende lo imprescindible en 9 pasos](#aprende-lo-imprescindible-en-9-pasos)
  * [Paso 1 - Crea tu primer proyecto](#paso-1---crea-tu-primer-proyecto)
  * [Step 2 - Configura el proyecto](#step-2---configura-el-proyecto)
  * [Paso 3 - Crear un Router](#paso-3---crear-un-router)
  * [Paso 4 - Crear una vista](#paso-4---crear-una-vista)
  * [Paso 5 - Crear un Controlador](#paso-5---crear-un-controlador)
  * [Paso 6 - Configurar la base de datos](#paso-6---configurar-la-base-de-datos)
  * [Paso 7 - Crear la Migración (Migration)](#paso-7---crear-la-migraci-n--migration-)
  * [Paso 8 - Crear un Modelo](#paso-8---crear-un-modelo)
  * [Bonus - Opciones (flags) de Artisan](#bonus---opciones--flags--de-artisan)
  * [Práctica 1](#práctica-1)
  * [Práctica 2](#práctica-2)
- [Level up: Laravel nivel intermedio](#level-up--laravel-nivel-intermedio)
  * [Borrado de registros](#borrado-de-registros)
  * [Práctica 3](#práctica-3)
  * [Formularios](#formularios)
  * [Práctica 4](#práctica-4)
  * [Construir layouts](#construir-layouts)
  * [Práctica 5](#práctica-5)
  * [Asignar nombres a las rutas](#asignar-nombres-a-las-rutas)
  * [Utilizar Bootstrap en tu proyecto](#utilizar-bootstrap-en-tu-proyecto)
  * [Relaciones One-to-Many](#relaciones-one-to-many)
  * [Generar datos de prueba](#generar-datos-de-prueba)
  * [Prácticas 6-7-8](#práctica-6)
  * [Autenticación](#autenticación)
  * [Práctica 9](#práctica-9)
  * [Manejo de sesiones](#manejo-de-sesiones)
  * [Práctica 10](#práctica-9)
- [Referencias](#referencias)
- [Licencia](#licencia)


## Introduction

### Prerequisitos
Para seguir esta guía únicamente necesitarás conocimientos básicos/medios de PHP y motivación para aprender.
¡Comencemos!

### Conceptos básicos
Tal y como dice la guía oficial, Laravel es un framework de desarrollo de aplicaciones web con una sintaxis elegante que nos permitirá desarrollar aplicaciones web de forma rápida y segura:

> Laravel is a web application framework with expressive, elegant syntax 
that will let us to develop web applications in a fast and secure way.

Es importante conocer bien los actores principales del ecosistema Laravel:
- Router: recibe todas las peticiones y las envía al controlador adecuado (también puede ejecutar algún middleware específico antes de llamar al controlador).
- Controladores (Controllers): contienen toda la lógica para reaccionar a las peticiones entrantes.
- Vistas (Views): contienen el código HTML y separan la presentación de la lógica de la aplicación (controlador).
- Modelos (Models): se utilizan para interactuar con la base de datos y aplicar la lógica de negocio.

![Laravel diagram](https://raw.githubusercontent.com/jvadillo/guia-laravel-paso-a-paso/master/laravel.jpg)


### Preparar el entorno de desarrollo
Puedes lanzar una aplicación desarrollada con Laravel en cualquier máquina o servidor con Apache, PHP y Composer. Para facilitar la creación del entorno de desarrollo, utilizaremos Laravel Homestead. Homestead es una Vagrant box que provee de un entorno de desarrollo con todo el software necesario: PHP, servidor web, base de datos, gestor de dependencias, etc.

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

* Puedes detener la máquina virtual con el comando `vagrant halt`.

## Aprende lo imprescindible en 9 pasos
### Introducción
Crearemos una aplicación web con Laravel paso a paso. Al finalizar los 9 pasos obtendremos como resultado una revista online a la que hemos llamado RevistApp y que mostrará los artículos almacenados en una base de datos. ¡Comencemos!

### Paso 1 - Crea tu primer proyecto
Accede a tu máquina virtual utilizando el comando `vagrant ssh` y ejecuta el siguiente comando para crear un nuevo proyecto:

```
composer create-project --prefer-dist laravel/laravel revistapp
```

Este comando inicializará un nuevo proyecto creado en el directorio `revistapp`. Puedes acceder a la aplicación entrando a http://homestead.test (o el dominio que hayas indicado en la configuración) desde tu navegador favorito.

De forma alternativa también puedes utilizar el comando `laravel new` que también creará un nuevo proyecto de Laravel en la carpeta especificada:
```
laravel new revistapp
```

### Step 2 - Configura el proyecto
#### La clave de la aplicación (Application Key)
Laravel utiliza una clave para securizar tu aplicación. La clave de aplicación es un string de 32 caracteres utilizado para encriptar datos como la sesión de usuario. Cuando se instala Laravel utilizando Composer o el instalador de Laravel, la clave se genera automáticamente, por lo que no es necesario hacer nada. Comprueba que existe un valor para `APP_KEY` en el fichero de configuración `.env`. En caso de no tener una clave generada, créala utilizando el siguiente comando:

```
php artisan key:generate
```

#### Establecer los permisos de directorio
Homestead realiza este paso por nosotros, por lo que los permisos deberían estar correctamente establecidos. Si no estás utilizando Homestead o quieres desplegar tu aplicación en un servidor, no olvides establecer permisos de escritura para el servidor web en los directorios `storage` y  `bootstrap/cache`.

### Paso 3 - Crear un Router
Cada vez que un usuario hace una petición a una de las rutas de la aplicación, Laravel trata la petición mediante un Router definido en el directorio `routes`, el cual será el encargado de direccionar la petición a un Controlador. Las rutas accesibles para navegadores estarán definidas en el archivo `routes/web.php` y aquellas accesibles para servicios web (webservices) estarán definidas en el archivo `routes/api.php`. A continuación se muestra un ejemplo:

```php
Route::get('/articulos', function () {
    return 'Let's read the articulos!';
});

```

El código anterior muestra cómo se define una ruta básica. En este caso, cuando el usuario realice una petición sobre `/articulos`, nuestra aplicación enviará una respuesta al usuario con el string 'Let's read the articulos!'.

Aparte de ejecutar las acciones definidas para cada ruta, Laravel ejecutará middlewere específico en función del Router utilizado (por ejemplo, el middlewere relacionado con las peticiones web proveerá de funcionalidades como el estado de la sesión o la protección (CSRF)[https://es.wikipedia.org/wiki/Cross-site_request_forgery]).

#### Ver rutas creadas
Artisan incluye un comando para mostrar todas las rutas de una aplicación de forma rápida. Basta con ejecutar el siguiente comando en la consola:

```
php artisan route:list
```

#### Devolver un JSON
También es posible devolver un JSON. Laravel convertirá automáticamente un array a JSON:

```php
Route::get('/articulos', function () {
    return ['foo' => 'bar'];
});

```

#### Parámetros en la ruta
Una URL puede contener información de nuestro interés. Laravel permite acceder a esta información de forma sencilla utilizando los parámetros de ruta:

```php
Route::get('articulos/{id}', function ($id) {
    return 'Vas a leer el artículo: '.$id;
});
```

Los parámetros de ruta vienen definidos entre llaves `{}` y se inyectan automáticamente en las callbacks. Es posible utilizar más de un parámetro de ruta:

```php
Route::get('articulos/{id}/user/{name}', function ($id, $name) {
    // Tu código aquí.
});
```

#### Acceder a la información de la petición
También es posible acceder a la información enviada en la petición. Por ejemplo, el siguiente código devolverá el valor enviado para el parámetro 'fecha' de la URL `/articulos?fecha=hoy`:

```php
Route::get('/articulos', function () {
    $date = request('fecha');
    return $date;
});
```

### Paso 4 - Crear una vista

#### Definiendo una vista sencilla
Las vistas contienen el HTML que sirve nuestra aplicación a los usuarios. Se almacenan en el directorio `resources/views` de nuestro proyecto.

```html
<!-- vista almacenada en resources/views/articulos.blade.php -->
<html>
    <body>
        <h1>¡Vamos a leer unos artículos!</h1>
    </body>
</html>
```

#### Devolver una vista
Cargar y devolver una vista al usuario es tan sencillo como utilizar la función global (helper) `view()`:

```php
Route::get('/articulos', function () {
    return view('articulos');
});
```

#### Acceder a datos desde la vista

Laravel utiliza el motor de plantillas [Blade](https://laravel.com/docs/6.x/blade) por defecto.

Mostrar datos almacenados en variables es muy sencillo:

```html
<html>
    <body>
	<h1>Vamos a leer al escritor {{ $nombre }}</h1>
        </ul>
    </body>
</html>
```

También es posible iterar por los datos de una colección o array. El siguiente ejemplo muestra como iterar por un array de strings de forma rápida:

```html
<!-- View stored in resources/views/articulos.blade.php -->
<html>
    <body>
	<h1>Vamos a leer al escritor {{ $nombre }}</h1>
        <h2>Estos son sus últimos artículos:</h2>
        <ul>
            @foreach ($articulos as $articulo)
                <li>{{ $articulo }}</li>
            @endforeach
        </ul>
    </body>
</html>
```

Para que la vista pueda acceder a los datos, es necesario proporcionárselos en la llamada al método `view()`:

```php
Route::get('/articulos', function () {
    $articulos = array('Primero', 'Segundo','Tercero', 'Último');
    return view('articulos', [
    	'nombre' => 'Ane Aranceta',
        'articulos' => $articulos
    ]);
});
```

Los datos se le pasarán como un array de tipo clave-valor.

Blade es un potente motor de plantillas que permite el uso de todo tipo de estructuras:

```html
@for ($i = 0; $i < 10; $i++)
    El valor actual es {{ $i }}
@endfor

@foreach ($users as $user)
    <p>El usuario: {{ $user->id }}</p>
@endforeach

@forelse($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>Eso es un bucle infinito.</p>
@endwhile

@if (count($articulos) === 1)
    Hay un artículo.
@elseif (count($articulos) > 1)
    Hay varios artículos.
@else
    No hay ninguno.
@endif

@unless (Auth::check())
    No estas autenticado.
@endunless
```

Puedes encontrar toda la información acerca de Blade en la [documentación oficial](https://laravel.com/docs/6.x/blade).


### Paso 5 - Crear un Controlador
Los controladores contienen la lógica para atender las peticiones recibidas. En otras palabras, un Controlador es una clase que agrupa el comportamiento de todas las peticiones relacionadas con una misma entidad. Por ejemplo, el ArticuloController será el encargado de definir el comportamiento de acciones como: creación de un artículo, modificación de un artóculo, búsqueda de artículos, etc.

#### Creando un Controller
Existen dos formas de crear un controlador:
- Crear manualmente una clase que extienda de la clase Controller de Laravel dentro del directorio `app/Http/Controllers`.
- Utilizar el comando de Artisan `make:controller`. Artisan es una herramienta que nos provee Laravel para interactuar con la aplicación y ejecutar instrucciones.

En este caso escogeremos la segunda opción y ejecutaremos el siguiente comando:
```
php artisan make:controller ArticuloController
```
De este modo Laravel creará automáticamente el controlador:.

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\User;

class ArticuloController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return View
     */
    public function show($id)
    {
        return view('show', ['articulo' => 'Mi primer artículo de Laravel');
    }
}
```
Añadiendo `--resource` al comando anterior, Artisan añadirá al controlador creado los siete métodos más comunes: `index()`, `create()`, `store()`, `show()`, `edit()`, `update()`, `destroy()`.

#### Enrutar el Controlador
El siguiente paso es añadir al Router la llamada a los métodos del Controlador.

```php
Route::get('articulos/', 'ArticuloController@index');
Route::get('articulos/{id}', 'ArticuloController@show');
Route::get('articulos/{id}/create', 'ArticuloController@create');
Route::post('articulos/', 'ArticuloController@store');
```
De esta forma direccionaremos las peticiones a los métodos de los controladores.


### Paso 6 - Configurar la base de datos

Es muy difícil de imaginar una aplicación web que no haga uso de una base de datos para almacenar la información. A continuación veremos como preparar la base de datos para nuestra aplicación.

#### Configuración de la base de datos

El fichero `.env` de Laravel contiene la configuración relacionada con la aplicación y el entorno, como por ejemplo la configuración de la base de datos. Abre el fichero `.env` con un editor y modifica las siguientes credenciales de la base de datos:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nombre de la base de datos(revistapp)
DB_USERNAME=nombre de usuario de la base de datos(root)
DB_PASSWORD=contraseña del usuario(root)
```
Todas estas variables de configuración serán referenciadas desde el archivo de configuración `database.php`.

#### Creación de la base de datos

Accede al cliente de MySQL como root:
```
mysql -u root
```

Crea la base de datos si no está creada:
```
CREATE DATABASE revistapp;
```

### Paso 7 - Crear la Migración (Migration)
Las Migraciones (Migrations) se utilizan para construir el esquema de la base de datos. Ejecuta el siguiente comando de Artisan para crear una nueva Migración para una tabla que llamaremos "articulos". 

```
php artisan make:migration create_articulos_table --create=articulos
```
Laravel creará una nueva migración automáticamente en el directorio `database/migrations`.

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
  
class CreateArticulosTable extends Migration

{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('articulos', function (Blueprint $table) {
            $table->increments('id');
            $table->string('titulo');
            $table->text('contenido');
            $table->timestamps();
        });
    }

  

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('articulos');
    }
}
```

Tal y como puedes deducir del código anterior, una migración contiene 2 métodos:
- `up()`: se utiliza para crear nuevas tablas, columnas o índices a la base de datos.
- `down()`: se utiliza para revertir operaciones realizadas por el método `up()`.

Existen una gran variedad de tipos de columnas disponibles para definir las tablas. Puedes encontrarlas en la [documentación oficial](https://laravel.com/docs/4.2/schema#adding-columns).

Para ejecutar las migraciones simplemente ejecuta el comando `migrate` de Artisan:
```
php artisan migrate
```
#### Crear usuario de base de datos
Es recomendable utilizar un usuario de base de datos diferente a `root`. Para ello, desde MySQL ejecuta lo siguiente:

```sql
CREATE USER 'nombre_usuario'@'localhost' IDENTIFIED BY 'tu_contrasena';
```
El comando anterior crea el usuario con la contraseña indicada. El siguiente paso es otorgar permisos al usuario:
```sql
GRANT ALL PRIVILEGES ON * . * TO 'nombre_usuario'@'localhost';
```
Para que los cambios surjan efecto ejecuta lo siguiente:
```sql
FLUSH PRIVILEGES;
```

No olvides actualizar los datos de acceso a base de datos en el fichero de configuración `.env`.

### Paso 8 - Crear un Modelo
Laravel incluye por defecto Eloquent ORM, el cual hace de la interacción con la base de datos una tarea fácil.Tal y como dice la documentación oficial:

>Each database table has a corresponding "Model" which is used to interact with that table. Models allow you to query for data in your tables, as well as insert new records into the table.

En otras palabras, cada tabla de la base de datos corresponde a un Modelo, el cual permite ejecutar operaciones sobre esa tabla (insertar o leer registros por ejemplo).

#### Creando un Modelo
Vamos a crear un Modelo:
```
php artisan make:model Articulo
```

El comando anterior ha creado una clase llamada Articulo en el directorio `app` (es el directorio por defecto para los modelos de Eloquent).

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Articulo extends Model
{
    //
}
```

Por defecto un modelo de Eloquent almacena los registros en una tabla con el mismo nombre pero en plural. En este caso, `Articulo` interactuará con la tabla llamada 'articulos'.

#### Recuperando datos de la base de datos
Los modelos de Eloquent se utilizan para recuperar información de las tablas relacionadas con el modelo. Proporcionan métodos como los siguientes:

```php

// Recupera todos los modelos
$articulos = App\Articulo::all();

// Recupera un modelo a partir de su clave
$articulo = App\Articulo::find(1);

// Recupera el primer modelo que cumpla con los criterios indicados
$articulos = App\Articulo::where('active', 1)->first();

// Recupera los modelos que cumplan con los criterios indicados y de la forma indicada:
$articulos = App\Articulo::where('active', 1)
               ->orderBy('titulo', 'desc')
               ->take(10)
               ->get();

// Iterar sobre los resultados:
foreach ($articulos as $articulo) {
    echo $articulo->titulo;
}

```

#### Insertar, actualizar y borrar modelos

El siguiente controlador contiene toda la lógica para insertar, actualizar y borrar modelos:

```php
<?php
   
namespace App\Http\Controllers;
   
use App\Articulo;
use Illuminate\Http\Request;
use Redirect;
   
class ArticuloController extends Controller
{

    /**
     * Create a new articulo instance.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validar la petición...

        $articulo = new Articulo;

        $articulo->titulo = request('titulo');

        $articulo->save();
    }
   
    /**
     * Update the specified resource in storage.
     *
     */
    public function update(Request $request, $id)
    {         
        $articulo = App\Articulo::find(1);

        $articulo->titulo = request('titulo');
        $articulo->contenido = request('contenido');

        $articulo->save();
    }
   
    /**
     * Remove the specified resource from storage.
     *
     */
    public function destroy($id)
    {
        // Encuentra el artículo por ID y lo elimina
        $articulo = App\Articulo::find($id);
        $articulo->delete();
        
        // Alternativa
        App\Articulo::destroy($id);
        
        //Borrar modelos por consulta (borra todos los que encuentre en la consulta)
        Articulo::where('id',$id)->delete();        
    }
     
}
```

Laravel proporciona alternativas para realizar operaciones más complejas, como creaciones en "masa" o crear un modelo solo si no existe:

```php
// Retrieve flight by name, or create it if it doesn't exist...
$articulo = App\Articulo::firstOrCreate(['name' => 'Laptop']);
```

Puedes encontrar todas las posibilidades en la [documentación oficial](https://laravel.com/docs/6.x/eloquent).

#### Tinker: un potente REPL para Laravel
Tinker es un potente [REPL](https://es.wikipedia.org/wiki/REPL) o consola interactiva que viene por defecto en Laravel. Resulta muy útil durante el desarrollo ya que permite interactuar con nuestra aplicación Laravel y probar cantidad de cosas: eventos, acceso a datos, etc.
Para iniciar Tinker hay que ejecutar el siguiente comando:
```
php artisan tinker
```
A partir de ese momento se puede comenzar a interactuar con nuestra aplicación, como muestra el ejemplo a continuación:
```bash
>>> $articulo = new App\Articulo
=> App\Articulo {#3014}
>>> $articulo->titulo="AA";
=> "AA"
>>> $articulo->contenido="BBBB";
=> "BBBB"
>>> $articulo
=> App\Articulo {#3014
     titulo: "Articulo numero 2",
     contenido: "Lorem ipsum...",
   }
>>> $articulos->save();
>>> \App\Articulo::count();
=> 2
>>> $articulos = App\Articulo::all();
=> Illuminate\Database\Eloquent\Collection {#3035
     all: [
       App\Articulo {#3036
         id: 1,
         titulo: "Articulo numero 1",
         contenido: "Lorem ipsum...",
         created_at: null,
         updated_at: null,
       },
       App\Articulo {#3046
         id: 2,
         titulo: "Articulo numero 2",
         contenido: "Lorem ipsum...",
         created_at: "2019-12-15 15:32:04",
         updated_at: "2019-12-15 15:32:04",
       },
     ],
   }

```
Para salir se ejecuta el comando `exit`.


### Bonus - Opciones (flags) de Artisan
Existen opciones muy útiles para generar archivos relacionados con los modelos. El siguiente ejemplo crea un modelo junto con su controlador y migración utilizando un único comando:

```
php artisan make:model Articulo -mcr
```
- -m indica la creación de una migración
- -c indica la creación de un controlador
 -r indica que el controlador creado será "resourceful" (incializado con los métodos).



### Práctica 1
Crea una aplicación Laravel que muestre un listado de artículos de la base de datos.

### Práctica 2
Añade a la aplicación anterior una nueva vista que muestre el detalle de un artículo. Se accederá desde la vista del listado de artículos.

## Level up: Laravel nivel intermedio

### Borrado de registros
El borrado de registros es un tema que suele traer complicaciones, debido a que desde una página HTML solo es posible enviar peticiones `GET` y `POST` (desde formularios). Por lo tanto, las alternativas son las siguientes:
- Crear una ruta GET específica para el borrado. Por ejemplo: `/removeArticulo/{id}`
- Hacer la petición utilizando AJAx y especificando en la llamada el tipo de método: `'type': 'DELETE'`
- Emular la llamada `DELETE` mediante el campo oculto `_method`. Para ello podemos utilizar los helpers o directivas de Laravel en un formulario para notificar que se trata de una petición de tipo `DELETE`:

```php	
<form method="POST">
    {{ csrf_field()}}
    {{ method_field("DELETE")}}
    <!--  ...  -->
</form>
```
A partir de Laravel 5.6, se pueden utilizar las siguientes directivas de Blade:

```php	
<form method="POST">
     @csrf
     @method("DELETE")
     <!-- ... -->
</form>
```

### Práctica 3
Añade la posibilidad de eliminar artículos mediante un enlace o botón en cada uno de los registros mostrados.

### Formularios
Los formularios HTML son la forma más habitual de recoger datos introducidos por los usuarios y enviarlos a la aplicación.

Para este cometido necesitaremos crear dos nuevas rutas en nuestro Router: una para mostrar al usuario el formulario de recogida de datos y otra para recibir y tratar los datos introducidos por el usuario.

```php
Route::get('articulos/create', 'ArticuloController@create');
Route::post('articulos/', 'ArticuloController@store');
```
De igual forma que hemos necesitado dos nuevas rutas, también necesitaremos dos nuevos métodos en nuestro Controller:

```php
class ArticuloController extends Controller
{
    /**
     * Show the form to create a new articulo.
     *
     * @return Response
     */
    public function create()
    {
        return view('create');
    }

    /**
     * Store a new articulo.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validate the Articulo
        
        // Create the articulo
        $articulo = new Articulo();
        
        $articulo->titulo = request('titulo');
        $articulo->contenido = request('contenido');
        
        $articulo->save();
        
        return redirect('/articulos');
    }
}
```

El último paso es crear la vista que contenga el formulario HTML y que será mostrada al usuario al invocar el método `create()` del controlador:

```html
<!-- View stored in resources/views/create.blade.php -->
<html>
    <body>
        <h1>Let's write a new articulo:</h1>
        <form action="/articulos/create" method="POST">
            <div>
                <label for="titulo">Name:</label>
                <input type="text" id="titulo" name="titulo">
            </div>
            <div>
              <label for="contenido">Text:</label>
              <textarea id="contenido" name="contenido"></textarea>
            </div>
            <div>
              <input type="submit" value="Submit">
            </div>
        </form>
    </body>
</html>
```

### Práctica 4
Añade la posibilidad de añadir nuevos artículos mediante un formulario ubicado en una nueva vista.

### Construir layouts
Las aplicaciones siempre contienen varias parte de la interfaz que son comunes en todas las páginas (la cabecera, menú de navegación, footer, etc.). Blade permite el uso de Layouts, los cuales permiten de forma muy sencilla compartir entre distintas vistas las partes que tienen en común y así evitar repetir lo mismo múltiples veces. La idea consiste en separar en un archivo distinto la parte común de nuestras vistas y especificar en ella las zonas que albergarán los contenidos específicos de cada vista (lo que no es común).
	
Empecemos por definir un layout básico:

```html
<html>
    <head>
        <title>App Name - @yield('titulo')</title>
    </head>
    <body>
        <div class="container">
            @yield('content')
        </div>
    	<div class="big-footer">
            @yield('footer')
        </div>
    </body>
</html>
```

La directiva @yield se utiliza para especificar el lugar donde se mostrarán los contenidos de cada sección.

Ahora crearemos la vista concreta que especificará el contenido a introducir en el layout. Es por esto que decimos que la vista extiende (extends) del layout, es decir, la vista heredará toda la estructura definida en el layout y sobreescribirá las partes concretas que defina (las secciones).

```html
@extends('layouts.master')

@section('titulo', 'Page Title')

@section('content')
    <h1>Hello World!</h1>
    <p>This is my body content.</p>
@endsection

@section('footer')
    <p>Built by @JonVadillo.</p>
@endsection

```

`@section` indica la sección del padre donde será introducido el contenido especificado entre las etiquetas `@section` y `@endsection`.

### Práctica 5
Crea un layout que englobe la parte común que contienen todas las vistas de la aplicación.

### Asignar nombres a las rutas
Una práctica muy habitual es asignar nombres a las rutas, lo cual es realmente sencillo:

```php
Route::get('articulos/', 'ArticuloController@index')->name('articulos.index');
Route::get('articulos/create', 'ArticuloController@index')->name('articulos.create');
Route::get('articulos/{id}', 'ArticuloController@show')->name('articulos.show');
```
Como se puede deducir del ejemplo anterior, utlizamos el método `name()` para definir el nombre que queremos establecer para cada ruta.
De este modo, podremos cargar las rutas utilizando el método global `route()`:
```php
// Generar una url
$url = route('articulos.index');

// Redireccionando a una ruta:
return redirect()->route('articulos.show');
```

En ocasiones necesitaremos indicar al método route los parámetros que necesitará insertar:
```php
Route::get('articulos/{id}', function ($id) {
    //
})->name('articulos.show');

$url = route('articulos.show', ['id' => 12);
// /articulos/12
```

Es posible comprobar las todas las rutas y sus nombres mediante el comande Artisan `php artisan route:list`.

### Utilizar Bootstrap en tu proyecto
A diferencia de versiones anteriores, en su versión 6, Laravel no incluye por defecto las dependencias necesarias para Bootstrap. Por lo tanto, tendremos 3 opciones para utilizar Bootstrap:

a) Referenciar las dependecias JS y CSS utilizando BootstrapCDN (enlaces disponibles en la [documentación oficial](https://getbootstrap.com/docs/4.4/getting-started/introduction/))

b) Descargar las dependecias ([enlace](https://getbootstrap.com/docs/4.4/getting-started/download/)) e incluirlas manualmente en las carpetas `/public/css` y `/public/js`.

c) Utilizar Laravel Mix para compilar nuestros archivos JS y CSS.

A continuación describiremos los pasos a seguir para incluir Bootstrap mediante la última opción descrita.

#### 1. Instalar el paquete Laravel/UI mediante composer.
```bash
composer require laravel/ui
```

#### 2. Añadir bootstrap a nuestro proyecto
```bash
php artisan ui bootstrap
```

#### 3. Instalar las dependencias
```bash
npm install
```
En ocasiones puede haber problemas si la máquina virtual se ejecuta en un host con Windows 10, por lo que en ese caso lanzar:

```bash
npm install --no-bin-links
```
```bash
npm install --global cross-env
```
#### 4. Compilar el código JS y CSS mediante Webpack:
```bash
npm run dev
```

#### 5. Incluir los ficheros generados (`public/js/app.js` y `public/css/app.css`) en nuestras vistas, utilizando el método helper `asset()`:
```html
<html>
<head>
    <meta charset="utf-8">
    <title>Mi aplicación</title>
    <link href="{{ asset('css/app.css') }}" rel="stylesheet" type="text/css" />
</head>
<body>
    <h1>Bienvenido a mi app</h1>
    <div class="content">
        @yield('content')
    </div>
    <script src="{{ asset('js/app.js') }}" type="text/js"></script>
</body>
</html>
```
El método `asset()` generará una URL a nuestros recursos en la carpeta `public/`. Si cambiamos la ubicación de nuestros recursos lo tendremos que especificar en la variable `ASSET_URL` del fichero `.env`.

### Relaciones One-to-Many
#### Definir una relación
Las relaciones en Eloquent ORM se definen como métodos. Supongamos que tenemos dos entidades, `User` y `Articulo`. Podríamos decir que un `User` tiene ('has') varios `Articulo` o que un `Articulo` pertenece a ('belongs to') un `User`. Por lo tanto, podemos definir la relación en cualquiera de los dos modelos, incluso en los dos.

```php
class User extends Model
{
    /**
     * Get the articulos records associated with the user.
     */
    public function articulos()
    {
        return $this->hasMany('App\Articulo');
    }
}
```

```php
class Articulo extends Model
{
    /**
     * Get the user record associated with the articulo.
     */
    public function user()
    {
        return $this->belongsTo('App\User');
    }
}
```
#### Acceder a los modelos de una relación
El acceso se podrá hacer como propiedades del propio modelo, es decir, mediante `$user->articulos` o `$articulo->user`. Esto es gracias a que Eloquent utiliza lo que conocemos como 'dynamic properties' y acceder a los métodos de las relaciones como si fuesen propiedades:

```php
$user = App\User::find(1); // Ejecuta la sentencia: select * from users where id = 1

$user_articulos = $user->articulos; // Ejecutara la sentencia: select * from articulos where user_id = 1

foreach ($user_articulos as $articulo) {
    //
}
```
En el ejemplo anterior, la variable `$user_articulos` contiene una colección de objetos de la clase `Articulo`. 

#### Claves foráneas (Foreign keys)
Por defecto, si no indicamos lo contrario, Eloquent utilizará como foreign key el nombre del modelo que contiene la colección añadiendo el sufijo `'_id'`. Es decir, en el caso anterior la tabla de `Articulo` deberá contener una columna llamada `'user_id'`.

```php
    public function up()
    {
        Schema::create('articulos', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->unsignedBigInteger('user_id');
            $table->string('titulo');
            $table->text('contenido');
            $table->timestamps();
        });
    }
}
```

#### Crear la restricción de las claves foráneas en la Base de Datos
Laravel también permite la creación de las restricciones de claves foráneas a nivel de base de datos. De esta forma podemos asegurarnos la integridad referencial en nuestra base de datos relacional. El siguiente ejemplo muestra cómo hacerlo de forma sencilla:

```php
Schema::table('articulos', function (Blueprint $table) {
    $table->unsignedBigInteger('user_id');

    $table->foreign('user_id')->references('id')->on('users');
});
```

Además, en función de la base de datos utilizada, también podemos especificar qué acciones llevar a cabo cuando borremos un registro cuyo ID es utilizado como FK en otros registros. Siguiendo el ejemplo anterior, cuando eliminemos un usuario, podremos especificar el comportamiento de la base de datos:
- Indicando que también se eliminen automáticamente todos sus artículos (conocido como 'borrado en cascada').
- Impidiendo que se realice el borrado del usuario si tiene artículos asociados (evitando así una inconsistencia referencial).
- Actualizar el valor `user_id` a `NULL` en los artículos que tengan como FK el usuario que será eliminado.

Para ello utilizaremos el método `onDelete`:

```php
$table->foreign('user_id')
      ->references('id')->on('users')
      ->onDelete('cascade');
 
 // O por ejemplo:
 $table->foreign('user_id')
      ->references('id')->on('users')
      ->onDelete('set null');
```

### Consejo: cómo añadir columnas a modelo existente
Existen dos escenarios posibles en los cuales queremos realizar cambios sobre modelos existentes:
- Estamos desarrollando una nueva aplicación y no nos importa borrar los datos existentes.
- Tenemos una aplicación en uso y queremos añadir columnas sin perder ningún registro.

En el primer caso, es suficiente con modificar la migración de la tabla correspondiente y ejecutar el comando:

```bash
php artisan migrate:fresh
```
Este comando eliminará todas las tablas de la base de datos y volverá a crearlas desde cero.

Para el segundo caso (modificar una tabla sin perder datos), lo recomendable es crear una nueva migración y ejecutar el comando `php artisan migrate` para lanzar los cambios. Normalmente se incluyen los cambios en el propio nombre de la migración, por ejemplo:

```bash
php artisan migrate:make add_category_to_articulos --table="articulos"
```

```php
public function up()
{
    Schema::table('articulos', function($table)
    {
        $table->string('category');
    });
    }
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
	    Schema::table('articulos', function ($table) {
		$table->dropColumn('category');
	    });
    }
}
```

### Generar datos de prueba
Laravel incluye un mecanismo llamado Seeder que sirve para rellenar la base de datos con datos de prueba. Por defecto nos incluye la clase DatabaseSeeder en la que podemos incluir el código que genere los datos de prueba:

```php
class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        $faker = Faker\Factory::create();

        for($i=0;$i<10;$i++){
            DB::table('articulos')->insert([
                'titulo' => $faker->realText(50,2),
                'contenido' => $faker->realText(400,2)
            ]);
        }
    }
}
```

Una vez creado nuestro Seeder es probable que necesites regenerar el fichero autoload.php:
```
composer dump-autoload
```
Por último, sólo nos quedaría lanzar el proceso de 'seeding':
```
php artisan db:seed
```
Si lo que quieres es lanzar el proceso de creación de base de datos y el de seeding a la vez, puedes utilizar el siguiente comando:
```
php artisan migrate:fresh --seed
```


#### Generar Seeders específicos
Es recomendable crear un seeder específico por cada entidad. Para ello, puedes utilizar el siguiente comando:

php artisan make:seeder UsersTableSeeder

Por último, tendrás que modificar la clase DatabaseSeeder para que lance nuestros Seeders:

```php
**
 * Run the database seeds.
 *
 * @return void
 */
public function run()
{
    $this->call([
        UsersTableSeeder::class,
        PostsTableSeeder::class,
        CommentsTableSeeder::class,
    ]);
}
```


### Práctica 6
La vista de detalle de artículo mostrará los comentarios del artículo e incluirá la posibilidad de añadir nuevos comentarios.

### Práctica 7
Asigna nombres a las rutas y utilízalos desde los controladores y vistas de la aplicación.

### Práctica 8
Añade estilo a la aplicación mediante Bootstrap 4.

### Autenticación
La autenticación es una funcionalidad presente en la gran mayoría de aplicaciones. Básicamente se trata de asegurar que un usuario es quién dice ser mediante un control de acceso a la aplicación.

Laravel 6 nos trae de serie los elementos necesarios para implementar la autenticación en nuestras aplicaciones y no tener que preocuparnos de hacer todas las tareas por nosotros mismos (login, registro, recuperación de contraseña, validación de usuario, etc.).

En concreto necesitaremos lo siguiente:
- Generar las vistas (login, registro, etc.), rutas y sus respectivas implementaciones.
- Especificar las partes de nuestra web (rutas) que queramos proteger.

#### Paso 1: Crear la estructura necesaria
El primer paso es instalar el paquete de laravel/ui mediante Composer:
```
composer require laravel/ui
```

Este paquete nos permitirá generar de forma automática todo lo relacionado con la interfaz de usuario: vistas, rutas y un nuevo controlador llamado `HomeController`. Para ello utilizaremos el siguiente comando:

```
php artisan ui:auth
```

Puesto que las vistas autogeneradas utilizan bootstrap, debemos o bien añadir manualmente bootstrap al layout generado `app.blade.php`:

```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

<script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>

```

o indicar la opción `bootstrap` al ejecutar el comando artisan:

```
php artisan ui bootstrap --auth
```

Si te fijas bien, el router `web.php` incluye dos nuevas líneas:

```php
Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');
```

La primera genera de forma automática las rutas necesarias para el proceso de autenticación (prueba a acceder a `/login` o `/register` para comprobarlo). Las vistas que se cargan son las autogeneradas en `resources/views/auth` (también se ha creado un layout).

La segunda es simplemente un nuevo controlador autogenerado como ejemplo. Si intentas acceder a la ruta creada `/home` verás como la aplicación nos lleva a la página de login. Esto es porque el controlador `HomeController` está definido como 'seguro' y solo usuarios autenticados podrán acceder.

¡Felicidades! Ya tienes la estructura básica de la aplicación creada. Puedes probar a registrar nuevos usuarios y realizar las acciones de login o logout con ellos.

#### Paso 2: Configuración básica
Una de las cosas que tendrás que configurar es la ruta a la que se envía al usuario tras autenticarse. Esto puede especificarse  mediante la variable `HOME` del archivo `RouteServiceProvider`:

```php
 public const HOME = '/home';
 ```
 
 Laravel utiliza por defecto el campo `email` para identificar a los usuarios. Puedes cambiar esto creando un método `username()` en el controlador `LoginController`.
 
 ```php
 public function username()
{
    return 'username';
}
```

Otro aspecto que podremos configurar es la ruta a la que enviaremos al usuario cuando intente acceder a una ruta protegida sin autenticarse. Por defecto Laravel le enviará a `/login`, pero podemos cambiar esto modificando el método `redirectTo()` del archivo `app/Http/Middleware/Authenticate.php`:

 ```php
protected function redirectTo($request)
{
    return route('login');
}
 ```
 
#### Paso 3: Securizar rutas
Indicaremos las rutas que queramos proteger directamente en nuestro ruter `web.php`:

```php
Route::get('profile', function () {
    // Solo podrán acceder usuarios autenticados.
})->middleware('auth');
 ```

También podremos indicarlo directamente en el constructor de un controlador de la siguiente forma:

```php
public function __construct()
{
    $this->middleware('auth');
}
```

#### Paso 4: Conseguir el usuario autenticado

Existen distintas formas de acceder al objeto del usuario autenticado. Desde cualquier punto de la aplicación podremos acceder utilizado la facade `Auth`:

 ```php
use Illuminate\Support\Facades\Auth;

// Conseguir el usuario autenticado:
$user = Auth::user();

// Conseguir el ID del usuario autenticado:
$id = Auth::id();
 ```
 
También podremos conseguirlo desde cualquier petición:

 ```php
public function update(Request $request)
{
    $request->user(); //  devuelve una instancia del usuario autenticado.
}
 ```
 
 Para comprobar si un usuario está autenticado, podemos emplear el método `check()`:

 ```php
use Illuminate\Support\Facades\Auth;

if (Auth::check()) {
    // El usuario está autenticado.
}
 ```

### Práctica 9
- Añade las funciones de login, registro y logout a la aplicación.
- Protege la ruta empleada para escribir un nuevo artículo (solo usuarios autenticados podrán acceder).
- La opción de borrar un artículo únicamente estará visible para usuarios autenticados.
- Muestra los comentarios de los artículos únicamente a usuarios autenticados. A los usuarios no identificados muéstrales un mensaje con un enlace a la página de login.

### Manejo de sesiones
HTTP es un protocolo sin estado (stateless), es decir, no guarda ninguna información sobre conexiones anteriores. Esto quiere decir que nuestra aplicación no tiene "memoria", y cada petición realizada por un usuario es nueva para la aplicación. Las sesiones permiten afrontar este problema, ya que son un mecanismo para almacenar información entre las peticiones que realiza un usuario al navegar por nuestra aplicación. Laravel implementa las sesiones de forma que su uso es muy sencillo para los desarrolladores.

#### Configuración
Laravel soporta el manejo de sesiones con distintos backends (bases de datos, ficheros, etc.). Esta configuración se indica en el fichero `config/session.php`, en le que podemos indicar el driver a utilizar ("file", "cookie", "database", "apc", "memcached", "redis", "dynamodb" o "array"). La opción utilizada por defecto es "cookie", la cual es suficiente para la mayoría de aplicaciones.

Más información sobre la configuración en la [documentación oficial](https://laravel.com/docs/6.x/session).

#### Uso de las sesiones
Existen dos formas principales de acceder a la inforamación de la sesión de usuario:
-  El helper global `session`
 ```php
    // Obtener un valor de la sesión
    $value = session('key');

    // Podemos indicar un valor por defecto
    $value = session('key', 'default');

    // Para almacenar un valor, le pasamos un Array:
    session(['key' => 'value']);
 ```
-  Mediante la instancia `Request` (inyectada en los métodos de nuestros controladores)

 ```php
    public function show(Request $request, $id)
    {
        $value = $request->session()->get('key');
	
        // También es posible indicar un valor por defecto si no existe ninguno:
	$value = $request->session()->get('key', 'default');
	
	// Almacenar un valor
	$request->session()->put('key', 'value');
	
	// Recuperar un valor y eliminarlo de la sesión
	$value = $request->session()->pull('key', 'default');
    }
 ```
 
 ### Práctica 10
Añade las siguientes funcionalidades a la aplicación:
- Guardar en sesión los artículos leídos: cuando un usuario entre a ver un artículo, se almacenará en sesión que ya lo ha leído.
- Guardar en sesión los artículos favoritos de un usuario: el usuario podrá hacer click en un enlace/botón que guarde en sesión ese artículo como favorito.

Los artículos marcados como favoritos se podrán distinguir visualmente (mediante un icono, texto en negrita o similar). Igualmente, los artículos leídos se mostrarán también de forma especial.
 
## Referencias
* [Laravel docs](https://laravel.com/docs/6.x) - Laravel Documentation
* [Laracats](https://laracasts.com) - Laravel screencasts

## Licencia

![Licencia_img](http://mirrors.creativecommons.org/presskit/buttons/80x15/png/by-nc-sa.png)

Esta guía esta licenciada como [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es_ES) aunque no necesariamente las imágenes de su interior.

El código que contiene este [repositorio](https://github.com/jvadillo/guia-laravel-paso-a-paso) se encuenta bajo la licencia [GNU GPL-3.0](https://github.com/jvadillo/guia-laravel-paso-a-paso/blob/master/LICENSE)

