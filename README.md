# Aprende Laravel 6: guía paso a paso :rocket:
[![Open Source Love png2](https://badges.frapsoft.com/os/v2/open-source.png?v=103)](https://www.jonvadillo.com) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://www.jonvadillo.com) [![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://www.jonvadillo.com)

Esto es una guía práctica para aprender a paso a paso a desarrollar aplicaciones web con Laravel 6.

## Table of Contents
- [Introduction](#introduction)
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
  * [Práctica 6](#práctica-6)
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
### Paso 1 - Crea tu primer proyecto
Accede a tu máquina virtual utilizando el comando `vagrant ssh` y ejecuta el siguiente comando para crear un nuevo proyecto:

```
composer create-project --prefer-dist laravel/laravel articulos-app
```

Este comando inicializará un nuevo proyecto creado en el directorio `noticias-app`. Puedes acceder a la aplicación entrando a http://homestead.test (o el dominio que hayas indicado en la configuración) desde tu navegador favorito.

De forma alternativa también puedes utilizar el comando `laravel new` que también creará un nuevo proyecto de Laravel en la carpeta especificada:
```
laravel new articulos-app
```

### Step 2 - Configura el proyecto
#### La clave de la aplicación (Application Key)
Laravel utiliza una clave para securizar tu aplicación. La clave de aplicación es un string de 32 caracteres utilizado para encriptar datos como la sesión de usuario. Cuando se instala Laravel utilizando Composer o el instalador de Laravel, la clave se genera automáticamente, por lo que no es necesario hacer nada. Comprueba que existe un valor para APP_KEY en el fichero de configuración `.env`. En caso de no tener una clave generada, créala utilizando el siguiente comando:

```
php artisan key:generate
```

#### Permisos de directorio
Homestead realiza este paso por nosotros, por lo que los permisos deberían estar correctamente establecidos. Si no estás utilizando Homestead o quieres desplegar tu aplicación en un servidor, no olvides establecer permisos de escritura para el servidor web en los directorios `storage` y  `bootstrap/cache`.

### Paso 3 - Crear un Router
Cada vez que un usuario hace una petición a una de las rutas de la aplicación, Laravel trata la petición mediante un Router definido en el directorio `routes`, el cual será el encargado de direccionar la petición a un Controlador. Las rutas accesibles para navegadores estarán definidas en el archivo `routes/web.php` y aquellas accesibles para servicios web (webservices) estarán definidas en el archivo `routes/api.php`. A continuación se muestra un ejemplo:

```php
Route::get('/articles', function () {
    return 'Let's read the articles!';
});

```

El código anterior muestra cómo se define una ruta básica. En este caso, cuando el usuario realice una petición sobre `/articles`, nuestra aplicación enviará una respuesta al usuario con el string 'Let's read the articles!'.

Aparte de ejecutar las acciones definidas para cada ruta, Laravel ejecutará middlewere específico en función del Router utilizado (por ejemplo, el middlewere relacionado con las peticiones web proveerá de funcionalidades como el estado de la sesión o la protección (CSRF)[https://es.wikipedia.org/wiki/Cross-site_request_forgery]).

#### Ver rutas creadas
Artisan incluye un comando para mostrar todas las rutas de una aplicación de forma rápida. Basta con ejecutar el siguiente comando en la consola:

```
php artisan route:list
```

#### Devolver un JSON
También es posible devolver un JSON. Laravel convertirá automáticamente un array a JSON:

```php
Route::get('/articles', function () {
    return ['foo' => 'bar'];
});

```

#### Parámetros en la ruta
Una URL puede contener información de nuestro interés. Laravel permite acceder a esta información de forma sencilla utilizando los parámetros de ruta:

```php
Route::get('articles/{id}', function ($id) {
    return 'You want the article '.$id;
});
```

Los parámetros de ruta vienen definidos entre llaves `{}` y se inyectan automáticamente en las callbacks. Es posible utilizar más de un parámetro de ruta:

```php
Route::get('articles/{id}/user/{name}', function ($id, $name) {
    // Code here.
});
```

#### Acceder a la información de la petición
También es posible acceder a la información enviada en la petición. Por ejemplo, el siguiente código devolverá el valor enviado para el parámetro 'date' de la URL `/articles?date=today`:

```php
Route::get('/articles', function () {
    $date = request('date');
    return $date;
});
```

### Paso 4 - Crear una vista

#### Definiendo una vista sencilla
Las vistas contienen el HTML que sirve nuestra aplicación a los usuarios. Se almacenan en el directorio `resources/views` de nuestro proyecto.

```html
<!-- vista almacenada en resources/views/articles.blade.php -->
<html>
    <body>
        <h1>Let's read some articles!</h1>
    </body>
</html>
```

#### Devolver una vista
Cargar y devolver una vista al usuario es tan sencillo como utilizar la función global (helper) `view()`:

```php
Route::get('/articles', function () {
    return view('articles');
});
```

#### Acceder a datos desde la vista

Laravel utiliza el motor de plantillas [Blade](https://laravel.com/docs/6.x/blade) por defecto.

Mostrar datos almacenados en variables es muy sencillo:

```html
<html>
    <body>
	<h1>My name is {{ $name }}</h1>
        </ul>
    </body>
</html>
```

También es posible iterar por los datos de una colección o array. El siguiente ejemplo muestra como iterar por un array de strings de forma rápida:

```html
<!-- View stored in resources/views/articles.blade.php -->
<html>
    <body>
	<h1>My name is {{ $name }}</h1>
        <h2>These are our latest articles:</h2>
        <ul>
            @foreach ($articles as $article)
                <li>{{ $article }}</li>
            @endforeach
        </ul>
    </body>
</html>
```

Para que la vista pueda acceder a los datos, es necesario proporcionárselos en la llamada al método `view()`:

```php
Route::get('/articles', function () {
    $articles = array('First', 'Second','Third', 'Last');
    return view('articles', [
    	'name' => 'Mike James',
        'articles' => $articles
    ]);
});
```

Los datos se le pasarán como un array de tipo clave-valor.

Blade es un potente motor de plantillas que permite el uso de todo tipo de estructuras:

```html
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile

@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif

@unless (Auth::check())
    You are not signed in.
@endunless
```

Puedes encontrar toda la información acerca de Blade en la [documentación oficial](https://laravel.com/docs/6.x/blade).


### Paso 5 - Crear un Controlador
Los controladores contienen la lógica para atender las peticiones recibidas. En otras palabras, un Controlador es una clase que agrupa el comportamiento de todas las peticiones relacionadas con una misma entidad. Por ejemplo, el ArticleController será el encargado de definir el comportamiento de acciones como: creación de un artículo, modificación de un artóculo, búsqueda de artículos, etc.

#### Creando un Controller
Existen dos formas de crear un controlador:
- Crear manualmente una clase que extienda de la clase Controller de Laravel dentro del directorio `app/Http/Controllers`.
- Utilizar el comando de Artisan `make:controller`. Artisan es una herramienta que nos provee Laravel para interactuar con la aplicación y ejecutar instrucciones.

En este caso escogeremos la segunda opción y ejecutaremos el siguiente comando:
```
php artisan make:controller ArticleController
```
De este modo Laravel creará automáticamente el controlador:.

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\User;

class ArticleController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return View
     */
    public function show($id)
    {
        return view('show', ['article' => 'My first article about Laravel');
    }
}
```
Añadiendo `--resource` al comando anterior, Artisan añadirá al controlador creado los siete métodos más comunes: `index()`, `create()`, `store()`, `show()`, `edit()`, `update()`, `destroy()`.

#### Enrutar el Controlador
El siguiente paso es añadir al Router la llamada a los métodos del Controlador.

```php
Route::get('articles/', 'ArticleController@index');
Route::get('articles/{id}', 'ArticleController@show');
Route::get('articles/{id}/create', 'ArticleController@create');
Route::post('articles/', 'ArticleController@store');
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
DB_DATABASE=here your database name(articulos)
DB_USERNAME=here database username(root)
DB_PASSWORD=here database password(root)
```
Todas estas variables de configuración serán referenciadas desde el archivo de configuración `database.php`.

#### Creación de la base de datos

Accede al cliente de MySQL como root:
```
mysql -u root
```

Crea la base de datos si no está creada:
```
CREATE DATABASE articulos;
```

### Paso 7 - Crear la Migración (Migration)
Las Migraciones (Migrations) se utilizan para construir el esquema de la base de datos. Ejecuta el siguiente comando de Artisan para crear una nueva Migración para una tabla que llamaremos "articles". 

```
php artisan make:migration create_articles_table --create=articles
```
Laravel creará una nueva migración automáticamente en el directorio `database/migrations`.

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
  
class CreateArticlesTable extends Migration

{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->text('body');
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
        Schema::dropIfExists('articles');
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
php artisan make:model Article
```

El comando anterior ha creado una clase llamada Article en el directorio `app` (es el directorio por defecto para los modelos de Eloquent).

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    //
}
```

Por defecto un modelo de Eloquent almacena los registros en una tabla con el mismo nombre pero en plural. En este caso, `Article` interactuará con la tabla llamada 'articles'.

#### Recuperando datos de la base de datos
Los modelos de Eloquent se utilizan para recuperar información de las tablas relacionadas con el modelo. Proporcionan métodos como los siguientes:

```php

// Recupera todos los modelos
$articles = App\Article::all();

// Recupera un modelo a partir de su clave
$article = App\Article::find(1);

// Recupera el primer modelo que cumpla con los criterios indicados
$article = App\Article::where('active', 1)->first();

// Recupera los modelos que cumplan con los criterios indicados y de la forma indicada:
$articles = App\Article::where('active', 1)
               ->orderBy('title', 'desc')
               ->take(10)
               ->get();

// Iterar sobre los resultados:
foreach ($articles as $article) {
    echo $articles->title;
}

```

#### Insertar, actualizar y borrar modelos

El siguiente controlador contiene toda la lógica para insertar, actualizar y borrar modelos:

```php
<?php
   
namespace App\Http\Controllers;
   
use App\Article;
use Illuminate\Http\Request;
use Redirect;
   
class ArticleController extends Controller
{

    /**
     * Create a new article instance.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validar la petición...

        $article = new Article;

        $article->title = request('title');

        $article->save();
    }
   
    /**
     * Update the specified resource in storage.
     *
     */
    public function update(Request $request, $id)
    {         
        $article = App\Article::find(1);

        $article->title = request('title');
        $article->body = request('body');

        $article->save();
    }
   
    /**
     * Remove the specified resource from storage.
     *
     */
    public function destroy($id)
    {
        // Encuentra el artículo por ID y lo elimina
        $article = App\Article::find(1);
        $article->delete();
        
        // Alternativa
        App\Article::destroy(1);
        
        //Borrar modelos por consulta (borra todos los que encuentre en la consulta)
        Article::where('id',$id)->delete();        
    }
     
}
```

Laravel proporciona alternativas para realizar operaciones más complejas, como creaciones en "masa" o crear un modelo solo si no existe:

```php
// Retrieve flight by name, or create it if it doesn't exist...
$article = App\Article::firstOrCreate(['name' => 'Laptop']);
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
>>> $article = new App\Article
=> App\Article {#3014}
>>> $article->title="AA";
=> "AA"
>>> $article->body="BBBB";
=> "BBBB"
>>> $article
=> App\Article {#3014
     title: "Articulo numero 2",
     body: "Lorem ipsum...",
   }
>>> $article->save();
>>> \App\Article::count();
=> 2
>>> $articles = App\Article::all();
=> Illuminate\Database\Eloquent\Collection {#3035
     all: [
       App\Article {#3036
         id: 1,
         title: "Articulo numero 1",
         body: "Lorem ipsum...",
         created_at: null,
         updated_at: null,
       },
       App\Article {#3046
         id: 2,
         title: "Articulo numero 2",
         body: "Lorem ipsum...",
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
php artisan make:model Article -mcr
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
- Crear una ruta GET específica para el borrado. Por ejemplo: `/removeArticle/{id}`
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
Route::get('articles/create', 'ArticleController@create');
Route::post('articles/', 'ArticleController@store');
```
De igual forma que hemos necesitado dos nuevas rutas, también necesitaremos dos nuevos métodos en nuestro Controller:

```php
class ArticleController extends Controller
{
    /**
     * Show the form to create a new article.
     *
     * @return Response
     */
    public function create()
    {
        return view('create');
    }

    /**
     * Store a new article.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validate the Article
        
        // Create the article
        $prodcut = new Article();
        
        $article->title = request('title');
        $article->body = request('body');
        
        $article->save();
        
        return redirect('/articles');
    }
}
```

El último paso es crear la vista que contenga el formulario HTML y que será mostrada al usuario al invocar el método `create()` del controlador:

```html
<!-- View stored in resources/views/create.blade.php -->
<html>
    <body>
        <h1>Let's write a new article:</h1>
        <form action="/articles/create" method="POST">
            <div>
                <label for="title">Name:</label>
                <input type="text" id="title" name="title">
            </div>
            <div>
              <label for="body">Text:</label>
              <textarea id="body" name="body"></textarea>
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
        <title>App Name - @yield('title')</title>
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

@section('title', 'Page Title')

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
Route::get('articles/', 'ArticleController@index')->name('articles.index');
Route::get('articles/create', 'ArticleController@index')->name('articles.create');
Route::get('articles/{id}', 'ArticleController@show')->name('articles.show');
```
Como se puede deducir del ejemplo anterior, utlizamos el método `name()` para definir el nombre que queremos establecer para cada ruta.
De este modo, podremos cargar las rutas utilizando el método global `route()`:
```php
// Generar una url
$url = route('articles.index');

// Redireccionando a una ruta:
return redirect()->route('articles.show');
```

En ocasiones necesitaremos indicar al método route los parámetros que necesitará insertar:
```php
Route::get('articles/{id}', function ($id) {
    //
})->name('articles.show');

$url = route('articles.show', ['id' => 12);
// /articles/12
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
Las relaciones en Eloquent ORM se definen como métodos. Supongamos que tenemos dos entidades, `User` y `Article`. Podríamos decir que un `User` tiene ('has') varios `Article` o que un `Article` pertenece a ('belongs to') un `User`. Por lo tanto, podemos definir la relación en cualquiera de los dos modelos, incluso en los dos.

```php
class User extends Model
{
    /**
     * Get the articles records associated with the user.
     */
    public function articles()
    {
        return $this->hasMany('App\Article');
    }
}
```

```php
class Article extends Model
{
    /**
     * Get the user record associated with the article.
     */
    public function user()
    {
        return $this->belongsTo('App\User');
    }
}
```
#### Acceder a los modelos de una relación
El acceso se podrá hacer como propiedades del propio modelo, es decir, mediante `$user->articles` o `$article->user`. Esto es gracias a que Eloquent utiliza lo que conocemos como 'dynamic properties' y acceder a los métodos de las relaciones como si fuesen propiedades:

```php
$user = App\User::find(1); // Ejecuta la sentencia: select * from users where id = 1

$user_articles = $user->articles; // Ejecutara la sentencia: select * from articles where user_id = 1

foreach ($user_articles as $article) {
    //
}
```
En el ejemplo anterior, la variable `$user_articles` contiene una colección de objetos de la clase `Article`. 

#### Foreign keys
Por defecto, si no indicamos lo contrario, Eloquent utilizará como foreign key el nombre del modelo que contiene la colección añadiendo el sufijo `'\_id'`. Es decir, en el caso anterior la tabla de `Article` deberá contener una columna llamada `'user_id'`.

```php
    public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->unsignedBigInteger('user_id');
            $table->string('title');
            $table->text('body');
            $table->timestamps();
        });
    }
}
```

#### Consejo: cómo añadir columnas a modelo existente
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
php artisan migrate:make add_category_to_articles --table="articles"
```

```php
public function up()
{
    Schema::table('articles', function($table)
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
	    Schema::table('articles', function ($table) {
		$table->dropColumn('category');
	    });
    }
}
```

### Práctica 6
La vista de detalle de artículo mostrará los comentarios del artículo e incluirá la posibilidad de añadir nuevos comentarios.

## Referencias
* [Laravel docs](https://laravel.com/docs/6.x) - Laravel Documentation
* [Laracats](https://laracasts.com) - Laravel screencasts

## Licencia

![Licencia_img](http://mirrors.creativecommons.org/presskit/buttons/80x15/png/by-nc-sa.png)

Esta guía esta licenciada como [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es_ES) aunque no necesariamente las imágenes de su interior.

El código que contiene este [repositorio](https://github.com/jvadillo/guia-laravel-paso-a-paso) se encuenta bajo la licencia [GNU GPL-3.0](https://github.com/jvadillo/guia-laravel-paso-a-paso/blob/master/LICENSE)

