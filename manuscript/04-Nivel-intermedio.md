# Level up: Laravel nivel intermedio

## Borrado de registros
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

## Práctica 3
Añade la posibilidad de eliminar artículos mediante un enlace o botón en cada uno de los registros mostrados.

## Formularios
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
<!-- Vista almacenada en resources/views/create.blade.php -->
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

## Práctica 4
Añade la posibilidad de añadir nuevos artículos mediante un formulario ubicado en una nueva vista.

## Construir layouts
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

## Práctica 5
Crea un layout que englobe la parte común que contienen todas las vistas de la aplicación.

## Asignar nombres a las rutas
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

## Utilizar Bootstrap en tu proyecto
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

## Relaciones One-to-Many

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

## Consejo: cómo añadir columnas a modelo existente
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

## Generar datos de prueba
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


## Práctica 6
La vista de detalle de artículo mostrará los comentarios del artículo e incluirá la posibilidad de añadir nuevos comentarios.

## Práctica 7
Asigna nombres a las rutas y utilízalos desde los controladores y vistas de la aplicación.

## Práctica 8
Añade estilo a la aplicación mediante Bootstrap 4.

## Autenticación
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

## Práctica 9
- Añade las funciones de login, registro y logout a la aplicación.
- Protege la ruta empleada para escribir un nuevo artículo (solo usuarios autenticados podrán acceder).
- La opción de borrar un artículo únicamente estará visible para usuarios autenticados.
- Muestra los comentarios de los artículos únicamente a usuarios autenticados. A los usuarios no identificados muéstrales un mensaje con un enlace a la página de login.

## Manejo de sesiones
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
 
## Práctica 10
Añade las siguientes funcionalidades a la aplicación:
- Guardar en sesión los artículos leídos: cuando un usuario entre a ver un artículo, se almacenará en sesión que ya lo ha leído.
- Guardar en sesión los artículos favoritos de un usuario: el usuario podrá hacer click en un enlace/botón que guarde en sesión ese artículo como favorito.

Los artículos marcados como favoritos se podrán distinguir visualmente (mediante un icono, texto en negrita o similar). Igualmente, los artículos leídos se mostrarán también de forma especial.