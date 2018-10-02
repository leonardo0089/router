# Router @CoffeeCode

[![Maintainer](http://img.shields.io/badge/maintainer-@robsonvleite-blue.svg?style=flat-square)](https://twitter.com/robsonvleite)
[![Source Code](http://img.shields.io/badge/source-coffeecode/router-blue.svg?style=flat-square)](https://github.com/robsonvleite/router)
[![Source Code](http://img.shields.io/badge/source-coffeecode/router-blue.svg?style=flat-square)](https://github.com/robsonvleite/router)
[![Latest Version](https://img.shields.io/github/release/robsonvleite/router.svg?style=flat-square)](https://github.com/robsonvleite/router/releases)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)
[![Build](https://img.shields.io/scrutinizer/build/g/robsonvleite/router.svg?style=flat-square)](https://scrutinizer-ci.com/g/robsonvleite/router)
[![Quality Score](https://img.shields.io/scrutinizer/g/robsonvleite/router.svg?style=flat-square)](https://scrutinizer-ci.com/g/robsonvleite/router)
[![Total Downloads](https://img.shields.io/packagist/dt/coffeecode/router.svg?style=flat-square)](https://packagist.org/packages/coffeecode/router)

###### Small, simple and uncomplicated. The router is a PHP route components with abstraction for MVC. Prepared with RESTfull verbs (GET, POST, PUT, PATCH and DELETE), works on its own layer in isolation and can be integrated without secrets to your application.

Pequeno, simples e descomplicado. O router é um componentes de rotas PHP com abstração para MVC. Preparado com verbos RESTfull (GET, POST, PUT, PATCH e DELETE), trabalha em sua própria camada de forma isolada e pode ser integrado sem segredos a sua aplicação.

## About CoffeeCode

###### CoffeeCode is a set of small and optimized PHP components for common tasks. Held by Robson V. Leite and the UpInside team. With them you perform routine tasks with fewer lines, writing less and doing much more.

CoffeeCode é um conjunto de pequenos e otimizados componentes PHP para tarefas comuns. Mantido por Robson V. Leite e a equipe UpInside. Com eles você executa tarefas rotineiras com poucas linhas, escrevendo menos e fazendo muito mais.

### Highlights

- Router class with all RESTful verbs (Classe router com todos os verbos RESTful)
- Optimized dispatch with total decision control (Despacho otimizado com controle total de decisões)
- Requesting Spoofing for Local Verbalization (Falsificador (Spoofing) de requisição para verbalização local)
- It's very simple to create routes for your application or API (É muito simples criar rotas para sua aplicação ou API)
- Trigger and data carrier for the controller (Gatilho e transportador de dados para o controloador)
- Composer ready and PSR-2 compliant (Pronto para o composer e compatível com PSR-2)

## Installation

Router is available via Composer:

```bash
"coffeecode/router": "^1.0"
```

or run

```bash
composer require coffeecode/router
```

## Documentation

###### For details on how to use the router, see the sample folder with details in the component directory

Para mais detalhes sobre como usar o router, veja a pasta de exemplo com detalhes no diretório do componente

##### Routes

```php
<?php
require __DIR__ . "/../vendor/autoload.php";

use CoffeeCode\Router\Router;

$router = new Router("https://www.youdomain.com");

/**
 * routes
 * The controller must be in the namespace Test\Controller
 * this produces routes for route, route/$id, route/{$id}/profile, etc.
 */
$router->namespace("Test");

$router->get("/route", "Controller:method");
$router->post("/route/{id}", "Controller:method");
$router->put("/route/{id}/profile", "Controller:method");
$router->patch("/route/{id}/profile/{photo}", "Controller:method");
$router->delete("/route/{id}", "Controller:method");

/**
 * group by routes and namespace
 * this produces routes for /admin/route and /admin/route/$id
 * The controller must be in the namespace Dash\Controller
 */
$router->group("admin")->namespace("Dash");
$router->get("/route", "Controller:method");
$router->post("/route/{id}", "Controller:method");

/**
 * Group Error
 * This monitors all Router errors. Are they: 400 Bad Request, 404 Not Found, 405 Method Not Allowed and 501 Not Implemented
 */
$router->group("error")->namespace("Test");
$router->get("/{errcode}", "Coffee:notFound");

/**
 * This method executes the routes
 */
$router->dispatch();

/*
 * Redirect all errors
 */
if ($router->error()) {
    $router->redirect("/error/{$router->error()}");
}
```

##### Callable

```php
/**
 * GET httpMethod
 */
$router->get("/", function ($data) {
    $data = ["realHttp" => $_SERVER["REQUEST_METHOD"]] + $data;
    echo "<h1>GET :: Spoofing</h1>", "<pre>", print_r($data, true), "</pre>";
});

/**
 * POST httpMethod
 */
$router->post("/", function ($data) {
    $data = ["realHttp" => $_SERVER["REQUEST_METHOD"]] + $data;
    echo "<h1>POST :: Spoofing</h1>", "<pre>", print_r($data, true), "</pre>";
});

/**
 * PUT spoofing and httpMethod
 */
$router->put("/", function ($data) {
    $data = ["realHttp" => $_SERVER["REQUEST_METHOD"]] + $data;
    echo "<h1>PUT :: Spoofing</h1>", "<pre>", print_r($data, true), "</pre>";
});

/**
 * PATCH spoofing and httpMethod
 */
$router->patch("/", function ($data) {
    $data = ["realHttp" => $_SERVER["REQUEST_METHOD"]] + $data;
    echo "<h1>PATCH :: Spoofing</h1>", "<pre>", print_r($data, true), "</pre>";
});

/**
 * DELETE spoofing and httpMethod
 */
$router->delete("/", function ($data) {
    $data = ["realHttp" => $_SERVER["REQUEST_METHOD"]] + $data;
    echo "<h1>DELETE :: Spoofing</h1>", "<pre>", print_r($data, true), "</pre>";
});

$router->dispatch();
```

##### Form Spoofing

###### This example shows how to access the routes (PUT, PATCH, DELETE) from the application. You can see more details in the sample folder. From an attention to the _method field, it can be of the hidden type.

Esse exemplo mostra como acessar as rotas (PUT, PATCH, DELETE) a partir da aplicação. Você pode ver mais detalhes na pasta de exemplo. De uma atenção para o campo _method, ele pode ser do tipo hidden.

```html
<form action="" method="POST">
    <select name="_method">
        <option value="POST">POST</option>
        <option value="PUT">PUT</option>
        <option value="PATCH">PATCH</option>
        <option value="DELETE">DELETE</option>
    </select>

    <input type="text" name="first_name" value="Robson"/>
    <input type="text" name="last_name" value="Leite"/>
    <input type="text" name="email" value="cursos@upinside.com.br"/>

    <button>CoffeeCode</button>
</form>
```

##### PHP cURL exemple

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "http://localhost/coffeecode/router/exemple/spoofing/",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "PUT",
  CURLOPT_POSTFIELDS => "first_name=Robson&last_name=Leite&email=cursos%40upinside.com.br",
  CURLOPT_HTTPHEADER => array(
    "Cache-Control: no-cache",
    "Content-Type: application/x-www-form-urlencoded"
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

## Contributing

Please see [CONTRIBUTING](https://github.com/robsonvleite/router/blob/master/CONTRIBUTING.md) for details.

## Support

###### Security: If you discover any security related issues, please email cursos@upinside.com.br instead of using the issue tracker.

Se você descobrir algum problema relacionado à segurança, envie um e-mail para cursos@upinside.com.br em vez de usar o rastreador de problemas.

Thank you

## Credits

- [Robson V. Leite](https://github.com/robsonvleite) (Developer)
- [UpInside Treinamentos](https://github.com/upinside) (Team)
- [All Contributors](https://github.com/robsonvleite/router/contributors) (This Rock)

## License

The MIT License (MIT). Please see [License File](https://github.com/robsonvleite/router/blob/master/LICENSE) for more information.