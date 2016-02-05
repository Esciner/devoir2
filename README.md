# Slim Framework Skeleton Application

Use this skeleton application to quickly setup and start working on a new Slim Framework application. This application uses the latest Slim and Slim-Views repositories. It also uses Sensio Labs' [Twig](http://twig.sensiolabs.org) template library.

This skeleton application was built for Composer. This makes setting up a new Slim Framework application quick and easy.

## Install Composer

If you have not installed Composer, do that now. I prefer to install Composer globally in `/usr/local/bin`, but you may also install Composer locally in your current working directory. For this tutorial, I assume you have installed Composer locally.

<http://getcomposer.org/doc/00-intro.md#installation>

## Install the Application

After you install Composer, run this command from the directory in which you want to install your new Slim Framework application.

    php composer.phar create-project slim/slim-skeleton [my-app-name]

Replace <code>[my-app-name]</code> with the desired directory name for your new application. You'll want to:
* Point your virtual host document root to your new application's `public/` directory.
* Ensure `logs/` and `templates/cache` are web writeable.

That's it! Now go build something cool.

## How to Contribute

### Pull Requests

1. Fork the Slim Skeleton repository
2. Create a new branch for each feature or improvement
3. Send a pull request from each feature branch to the **develop** branch

It is very important to separate new features or improvements into separate feature branches, and to send a
pull request for each branch. This allows us to review and pull in new features or improvements individually.

### Style Guide

All pull requests must adhere to the [PSR-2 standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md).

# basicmvc
## Basic MVC for Slim Framework Usage

An example directory structure is following. You can change your structure what you want. You will specify your directories in your configurations.

    app/
        public/
            controller/
                ...
            model/
                ...
            view/
                ...
    .htaccess
    index.php
index.php file content is following. Define your directory constants and firstly, initialize Slim after that initalize BasicMVC. Thats all.

<?php 
/************** Some Configurations ****************/
define("APP_DIR", __DIR__ . "/app/");
define("APP_THEME", "default");
define("APP_TEMPLATE", APP_DIR . "public/view/theme/".APP_THEME."/");
/********** End of Some Configurations *************/


require_once __DIR__ . '/../vendor/autoload.php';

use BasicMVC\BasicMVC;

$twigView = new \Slim\Views\Twig();

$app = new \Slim\Slim(
    array(
        'mode'              => 'development', // development, test, and production
        'debug'             => true,
        'view'              => $twigView,
        'templates.path'    => APP_TEMPLATE
        )
    );

$basicmvc = new BasicMVC($app, array(
    "controllers_path"   => APP_DIR . "public/controller/",
    "models_path"        => APP_DIR . "public/model/",
    "library_path"       => APP_DIR . "public/system/library/",
    "template_constants" => array(
        "APP_THEME"     => APP_THEME
    )
));

$app->map('/(:directory(/:controller(/:method(/:args+))))', 
            function (
                $directory = "home", 
                $controller = "home", 
                $method = "index", 
                $args = array()
            ) use($app, $basicmvc) {

    $route = array(
        "directory"       => $directory,
        "controller"      => $controller,
        "method"          => $method,
        );

    echo $basicmvc->run($route, $args);

})->via('GET', 'POST', 'PUT', 'DELETE');

$app->run();