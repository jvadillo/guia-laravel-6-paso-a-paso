# Aprende Laravel 6: guía paso a paso

Esto es una guía práctica para aprender a paso a paso a desarrollar aplicaciones web con Laravel 6.

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
	- [Prerequisites](#prerequisites)
	- [Understanding basic concepts](#understanding-basic-concepts)
	- [Preparing you local development environment](#preparing-you-local-development-environment)
- [Learn the basics in 8 steps](#learn-the-basics-in-9-steps)
	- [Step 1 - Create your first project](#step-1---create-your-first-project)
	- [Step 2 - Configuring the project](#step-2---configuring-the-project)
	- [Step 3 - Create a Router](#step-3---create-a-router)
	- [Step 4 - Create a View](#step-4---create-a-view)
	- [Step 5 - Create a Controller](#step-5---create-a-controller)
	- [Step 6 - Set up the database](#step-6---set-up-the-database)
	- [Step 7 - Create Migration](#step-7---create-migration)
	- [Step 8 - Create a Model](#step-8---create-a-model)
	- [Bonus - Artisan flags](#bonus---artisan-flags)
	- [Practice: Creating a Newspaper App](#practice-creating-a-newspaper-app)
	- [Hands on: Creating a Newspaper App](#hands-on-creating-a-newspaper-app)
- [Forms](#forms)
	- [Step 1 - Create the routes and the controller methods](#step-1---create-the-routes-and-the-controller-methods)
	- [Step 2 - Create the view](#step-2---create-the-view)
- [References](#references)

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

![Laravel diagram](https://upload.wikimedia.org/wikipedia/commons/9/94/%E0%A6%B2%E0%A6%BE%E0%A6%B0%E0%A6%BE%E0%A6%AD%E0%A7%87%E0%A6%B2_%E0%A6%86%E0%A6%B0%E0%A7%8D%E0%A6%95%E0%A6%BF%E0%A6%9F%E0%A7%87%E0%A6%95%E0%A6%9A%E0%A6%BE%E0%A6%B0_.jpg)


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
If you don't have ssh keys uou will need to generate them from the terminal. Try to find the .ssh folder on your system (typically under C:\Users\USER_NAME in Windows and under the root folder ~ in Linux/Mac ) and look if two files named id_rsa and id_rsa.pub are present. You can skip this step if you find them. If not, run the following command from the console:

```
ssh-keygen -t rsa -C "your_email@example.com"
```

It will ask you some questions, so simply press ENTER and It will create two files named `id_rsa` and `id_rsa.pub` in a folder named `.ssh`.

#### 7. Configure Homestead
There are many things you can configure in Homestead, but we will mention just the most inportant ones:

- **ssh keys**: the automatically created configuration will be OK for both Mac and Linux. If you are using Windows, edit the following values:

```
authorize: c:/Users/USER_NAME/.ssh/id_rsa.pub

keys:
    - c:/Users/USER_NAME/.ssh/id_rsa
```

- **Shared folders**: the folders listed will be kept in sync between your local machine and the Homestead environment. Modify the value of the 'map' key and write your project's folder (i.e. `/Users/USER_NAME/dev/my-project` in Linux/Mac or `c:/dev/my-project` in Windows).
```
folders:
    - map: ~/code/project1
      to: /home/vagrant/project1

    - map: ~/code/project2
      to: /home/vagrant/project2
```
- **Nginx Sites**: The sites property allows you to easily map a "domain" to a folder on your Homestead environment.
```
sites:
    - map: homestead.test
      to: /home/vagrant/project1/public
```

#### 8. Configure local hosts
If you use the same development environmet for multiple applications, you will need to add the "domains" to the hosts file of your computer, in order to redirect the requests for your Homestead sites into your Homestead machine.
Edit the hosts file (located at /etc/hosts on Mac/Linux and at C:\Windows\System32\drivers\etc\hosts on Windows):

```
192.168.10.10  homestead.test
```

The general recommendation is tu use ".localhost", ".invalid", ".test", or ".example" domains.

#### 9. Run and test your virtual machine

Once we have edited the Homestead configuration file and the hosts file on our local machine, go to the Homestead directory and run the following command to boot the virtual machine:

```
vagrant up
```

You can now use SSH to log in your virtual machine:

```
vagrant ssh
```

Congratulations! You are ready to create your first Laravel application.

## Learn the basics in 9 steps
### Step 1 - Create your first project
Access to the vagrant machine using the `vagrant ssh` command and create a new project running the following command:

```
composer create-project --prefer-dist laravel/laravel my-laravel-project
```
This will initialize a project in the my-laravel-project folder. You can now access the site, http://homestead.test, using your favourite browser.

Alternatively you can use `laravel new` command that will also create a new Laravel installation in the directory you specify:
```
laravel new my-laravel-project
```

### Step 2 - Configuring the project
#### Application Key
Laravel uses an application key to secure you application. The application key is a 32 character string used to encrypt data like user sessions. When Laravel is installed using Composer or the Laravel installer, the key is automatically set for you. Check if there is a value for the APP_KEY value of your .env file and execute the following command if you need to generate it:

```
php artisan key:generate
```

#### Directory Permissions
As Homestead does this step for us, permissions should already be correctly set. If you are not using Homestead or you want to deploy your appllication in a server, don't forget setting `storage` and  `bootstrap/cache` directories as writable by the web server.

### Step 3 - Create a Router
When the user makes a request for a route, Laravel will handle the request by a router located in the `routes` directory, who will dispatch the request to a controller. Routes for web interfaces will be defined in the `routes/web.php` file and the ones defined for webservices will be placed in the `routes/api.php` file.

```php
Route::get('/articles', function () {
    return 'Let's read the articles!';
});

```

The code above shows how to define a basic route. In this case, when the user makes a request for the /articles, our application will send a response with the 'Let's read the articles!' string.

Apart executing the actions defined for each route, Laravel will run the route specific middlewere depending on the router userd (for example, the middlewere related to the web router will provide features like session state and CSRF protection).

#### Returnin a JSON
It is also possible to return a JSON. Laravel will automatically convert arrays to JSON:

```php
Route::get('/articles', function () {
    return ['foo' => 'bar'];
});

```

#### Route parameters
A URL may contain some information of our interest. Laravel allows you to do this easily using route parameters:

```php
Route::get('articles/{id}', function ($id) {
    return 'You want the article '.$id;
});
```

Route parameters are encased within {} braces and are injected into route callbacks. You can use as many route parameters as you want:

```php
Route::get('articles/{id}/user/{name}', function ($id, $name) {
    // Code here.
});
```

#### Accesing to request data
Is it also possible to acces to request data. The following code will return the value for the 'date' parameter of the `/articles?date=today` URL:

```php
Route::get('/articles', function () {
    $date = request('date');
    return $date;
});
```

### Step 4 - Create a View

#### Defining a basic view
Views contain the HTML served by our application and are stored in the `resources/views` directory.

```html
<!-- View stored in resources/views/articles.blade.php -->
<html>
    <body>
        <h1>Let's read some articles!</h1>
    </body>
</html>
```

#### How to return a view
Views can be returned to the user using the global `view()` helper:

```php
Route::get('/articles', function () {
    return view('articles');
});
```

#### Accessing data from the view

Laravel uses the [Blade](https://laravel.com/docs/6.x/blade) templating engine by default.

Echoing Data in Blade is really easy:
```html
<html>
    <body>
	<h1>My name is {{ $name }}</h1>
        </ul>
    </body>
</html>
```

It is also possible to make a loop and iterate an array of data. Let's modify the previous view in order to return a real list of articles.

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

All the data needed by the view must be provided within the view helper call:

```php
Route::get('/articles', function () {
    $articles = array('First', 'Second','Third', 'Last');
    return view('articles', [
    	'name' => 'Mike James',
        'articles' => $articles
    ]);
});
```
The data must be an array with key / value pairs.

Blade is a powerful templating engine that allows all kind of structures:

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

You can find all you need about Blade in the [official documentation](https://laravel.com/docs/6.x/blade).


### Step 5 - Create a Controller
Controllers contain the logic for handling incoming requests. In other words, a Controller is a class that groups the behaviour of requests related to an entity. For example, a ArticleController will be in charge of handling action like: create an article, edit an article, find articles and so on.

#### Create a Controller
There are 2 ways of creating a Controller:
- Create manually a class which extends the Laravel Controller class inside `app/Http/Controllers` directory.
- Use the `make:controller` Artisan command.

In this case we will choose the second option and run the following command:
```
php artisan make:controller ArticleController
```
And Laravel will create the controller automatically for us.

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
Adding `--resource` to the previous command will order Artisan to add to the Controller the most common seven methods: `index()`, `create()`, `store()`, `show()`, `edit()`, `update()`, `destroy()`.

#### Rounting the Controller

The next step is to add in the Router the Controller's methods.

```php
Route::get('articles/', 'ArticleController@index');
Route::get('articles/{id}', 'ArticleController@show');
Route::get('articles/{id}/create', 'ArticleController@create');
Route::post('articles/', 'ArticleController@store');
```
This will address the requests to the methods implemented in the Controller.


### Step 6 - Set up the database

It is hard to imagine a web application that doesn't make use of a Database to manage the information within it.

#### Database configuration

Laravel `.env` file contains environment configurations such as the database configuration. Open the `.env` file and set database credentials as bellow:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=here your database name(newspaper)
DB_USERNAME=here database username(root)
DB_PASSWORD=here database password(root)
```
All these configuration variables will be referenced from the `database.php` configuration file.

#### Database creation

Sign in into MySQL using the root user:
```
mysql -u root
```

Create the database if it is not created:
```
CREATE DATABASE newspaper;
```

### Step 7 - Create Migration
Migrations are used to build application's database schema. Run the following Artisan command in order to create a migration for "articles" table. 

```
php artisan make:migration create_articles_table --create=articles
```
Laravel will create a new migration placed in `database/migrations` directory.

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

As you can see in the code above, a migration contains 2 methods:
- `up()`: used to add new tables, columns, or indexes to the database
- `down()`: used to reverse the operations defined in the `up()` method.

There is a wide variety of column types available in order to define tables.

To run the migrations execute the `migrate` Artisan command:
```
php artisan migrate
```

### Step 8 - Create a Model
Laravel includes Eloquent ORM, which makes the database interaction really easy. As the official documentation says, with Eloquent:

>Each database table has a corresponding "Model" which is used to interact with that table. Models allow you to query for data in your tables, as well as insert new records into the table.

#### Creating a Model
Let's create a Model:
```
php artisan make:model Article
```

The previous command creates an Article in the `app` directory (the default directory for Eloquent Models).

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    //
}
```

By default an Eloquent Model will store records in the table with the Model name in plural. In this case, `Article` will interact with 'articles' table.

#### Retrieving data from the Database
Eloquent Models will be used to query the database tables associated with them. They provide methods like the followings:

```php

// Retrieve all models
$articles = App\Article::all();

// Retrieve a model by its primary key
$article = App\Article::find(1);

// Retrieve the first model matching the query constraints
$article = App\Article::where('active', 1)->first();

// Retrieve models matching the query constraints
$articles = App\Article::where('active', 1)
               ->orderBy('title', 'desc')
               ->take(10)
               ->get();

// Loop over the collection like an array:
foreach ($articles as $article) {
    echo $articles->title;
}

```

#### Inserting, Updating and Deleting models

The following Controller contains the all the logic for inserting, updating and deleting models:

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
        // Validate the request...

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
        // Find article by id and delete it
        $article = App\Article::find(1);
        $article->delete();
        
        // Alternative
        App\Article::destroy(1);
        
        //Deleting Models By Query (deletes all the matching elements)
        Article::where('id',$id)->delete();        
    }
     
}
```

Laravel provides alternative methods for doing more complex operations like mass creations or creaing only if the model doesn't exist:

```php
// Retrieve flight by name, or create it if it doesn't exist...
$article = App\Article::firstOrCreate(['name' => 'Laptop']);
```

You can find all the possibilities explained in the official documentation.

### Bonus - Artisan flags
There are useful flags to generate related files to the model. The following example creates a model, migration and controller using only one command:

```
php artisan make:model Article -mcr
```
- -m will create a migration file
- -c will create a controller
 -r will indicate that controller should be resourceful



### Practice: Creating a Newspaper App
Create a Laravel app that shows a article list from de Database.
#### Solution
1. Create a new project named laravel-article list.
2. Create a Model with its corresponding Controller and Migration.
```
php artisan make:model Article -mcr
```
3. Define the article model attributes modifying the migration.
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
            $table->timestamp('publish_date')->useCurrent();
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

4. Run the migration:
```
php artisan migrate
```

5. Edit the `routes/web.php` file and add the route to the router:
```php
Route::get('/', 'ArticleController@index');
```

6. Implement the `index()` method of the Controller:

```php
public function index()
{
    $articles = Article::all();
    return view('articles', [
        'articles' => $articles
    ]);
}
```

7. Create the `articles` view:

```html
<!-- View stored in resources/views/articles.blade.php -->
<html>
    <body>
        <h1>These are our articles:</h1>
        <ul>
            @foreach ($articles as $article)
                <li>{{ $articles->title }} ({{ $articles->id }})</li>
            @endforeach
        </ul>
    </body>
</html>
```
8. Populate the database: edit the DatabaseSeeder class and launching the `php artisan db:seed` command.

```php
<?php

use Illuminate\Support\Str;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB;

class DatabaseSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $faker = Faker\Factory::create();

        for($i=0;$i<10;$i++){
            DB::table('articles')->insert([
                'title' => $faker->realText(50,2),
                'body' => $faker->realText(400,2)
            ]);
        }
    }
}
```
7. Create the `articles` view:

```html
<!-- View stored in resources/views/show.blade.php -->
<html>
    <body>
        <h1>Article detail:</h1>
        <ul>
            <li>Title: {{ $article->title }}</li>
            <li>Body: {{ $article->body }}</li>
        </ul>
    </body>
</html>
```
9. Open the app in the browser!

### Hands on: Creating a Newspaper App
Add to the previous app a new view that shows the detail of a article.

1. Add the new route in the `routes/web.php` file:
```php
Route::get('/{id}', 'ArticleController@show');
```

2. Implement the `show()` method of the Controller:

```php
public function show($id)
{
    $articles = Article::find($id);
    return view('show', [
        'article' => $article
    ]);
}
```
## Intermediate Laravel

### Delete requests

### Forms
HTML forms are the traditional way to collect data from users and send it to the application. 

We will need to create 2 routes in order to add a form to our application: one for displaying the form to the user and another one to handle the response.

```php
Route::get('articles/create', 'ArticleController@create');
Route::post('articles/', 'ArticleController@store');
```
In the same way we created two routes, we will also add two methods to the controller:

```php
class PostController extends Controller
{
    /**
     * Show the form to create a new blog post.
     *
     * @return Response
     */
    public function create()
    {
        return view('create');
    }

    /**
     * Store a new blog post.
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

The last step will be to create a traditional form will that be used to collect the information about the model.

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

### Building layouts
There is always some piece of code that is being repeated in every view. We are talking about the <head> of the view and other parts of the body such as a header, navigation menu or footer. Blade layouts make it possible to share common html markup and assets across multiple views. We idea is to separate into a different file the part of the view that is common and specify same container in the view that must be filled with specific content.
	
Let's start creating a layout:

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

The @yield directive is used to specify the place where the contents of a given section will be displayed.

Now we'll build a unique view that extends from the previus layout. Extending a view means that the parent will be used and overriden with the content we specify in the child:

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

@section indicates the section of the parent layout that will be filled with the content.

### Add bootstrap to Laravel
TODO

### Eloquest relationships
TODO

### Sessions and flash messaging
TODO

### Laravel take and paginate
TODO

## Advanced Laravel
TODO

### Authentication
TODO

### User notifications
TODO

### The front-end in Laravel
TODO

## References
* [Laravel docs](https://laravel.com/docs/5.5) - Laravel Documentation

