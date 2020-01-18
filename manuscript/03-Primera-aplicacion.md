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
Los controladores contienen la lógica para atender las peticiones recibidas. En otras palabras, un Controlador es una clase que agrupa el comportamiento de todas las peticiones relacionadas con una misma entidad. Por ejemplo, el ArticleController será el encargado de definir el comportamiento de acciones como: creación de un artículo, modificación de un artóculo, búsqueda de artículos, etc.

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

Por defecto un modelo de Eloquent almacena los registros en una tabla con el mismo nombre pero en plural. En este caso, `Article` interactuará con la tabla llamada 'articulos'.

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
   
use App\Article;
use Illuminate\Http\Request;
use Redirect;
   
class ArticuloController extends Controller
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
>>> $article->titulo="AA";
=> "AA"
>>> $articulo->contenido="BBBB";
=> "BBBB"
>>> $article
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
