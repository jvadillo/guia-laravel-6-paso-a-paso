## Introduction

### Prerequisitos
Para seguir esta guía únicamente necesitarás conocimientos básicos/medios de PHP y motivación para aprender. Si todavía no dispones de estos conocimientos, puedes utilizar el material gratuito disponible en http://jonvadillo.com/learn para comenzar tu aprendizaje.

¡Comencemos!

### Conceptos básicos
Tal y como dice la guía oficial, Laravel es un framework de desarrollo de aplicaciones web con una sintaxis elegante que nos permitirá desarrollar aplicaciones web de forma rápida y segura:

> Laravel is a web application framework with expressive, elegant syntax 
that will let us to develop web applications in a fast and secure way.

En este libro aprenderás a crear aplicaciones web con Laravel desde cero, desde lo más básico hasta funcionalidades más complejas que incluyan aspectos como la seguridad o control de acceso.

### Características principales
TODO

### Ecosistema Laravel
Es importante conocer bien los actores principales del ecosistema Laravel:
- Router: recibe todas las peticiones y las envía al controlador adecuado (también puede ejecutar algún middleware específico antes de llamar al controlador).
- Controladores (Controllers): contienen toda la lógica para reaccionar a las peticiones entrantes.
- Vistas (Views): contienen el código HTML y separan la presentación de la lógica de la aplicación (controlador).
- Modelos (Models): se utilizan para interactuar con la base de datos y aplicar la lógica de negocio.

![Laravel diagram](https://raw.githubusercontent.com/jvadillo/guia-laravel-paso-a-paso/master/laravel.jpg)