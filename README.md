## Building a PHP MVC Framework from Scratch

Learning to build a PHP Model-View-Controller (MVC) framework from scratch. MVC is a design pattern commonly used in web development to separate the application logic, data, and presentation.


### Step 1: Setting up the Project
Create a new directory for your project and navigate to it using the command line:
```
mkdir php-mvc-framework
cd php-mvc-framework

```

Initialise a new Composer project
```
composer init

```

### Step 2: Creating the Directory Structure
Create the following directory structure for your project:
```
src/
    Controllers/
    Models/
    Views/
```

### Step 3: Building the Core Components Router
Create a new file named ```Router.php``` in the ```src/``` directory. This file will contain the main routing logic for your framework.

```
<?php

namespace MVC;

class Router {
    protected $routes = [];

    public function addRoute($route, $controller, $action) {
        $this->routes[$route] = ['controller' => $controller, 'action' => $action];
    }

    public function dispatch($uri) {
        if (array_key_exists($uri, $this->routes)) {
            $controller = $this->routes[$uri]['controller'];
            $action = $this->routes[$uri]['action'];

            $controller = new $controller();
            $controller->$action();
        } else {
            throw new \Exception("No route found for URI: $uri");
        }
    }
}
    
```

#### Base Controller
Create a new file named ```Controller.php``` in the ```src/``` directory. This file will contain the base controller class that all other controllers will extend.

```
<?php

namespace MVC;

class Controller {
    protected function render($view, $data = []) {
        extract($data);

        include "Views/$view.php";
    }
}
    
```

### Step 4: Implementing an Example Application
#### Create a Model
Create a new file named ```User.php``` in the  ```src/Models/``` directory. This model will represent a user in your application.
```
<?php

namespace MVC\Models;

class User {
    public $name;
    public $email;

    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
    }
}
    
```
#### Create a Controller
Create a new file named ```UserController.php``` in the ```src/Controllers/``` directory. This controller will handle user-related actions.
```
<?php

namespace MVC\Controllers;

use MVC\Controller;
use MVC\Models\User;

class UserController extends Controller {
    public function index() {
        $users = [
            new User('John Doe', 'john@example.com'),
            new User('Jane Doe', 'jane@example.com')
        ];

        $this->render('user/index', ['users' => $users]);
    }
}
    
```
#### Creating a View
Create a new file named ```index.php``` in the ```src/views/user/``` directory. This view will display a list of users.
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>User List</title>
</head>
<body>
    <h1>User List</h1>
    <ul>
        <?php foreach ($users as $user): ?>
            <li><?= $user->name ?> (<?= $user->email ?>)</li>
        <?php endforeach; ?>
    </ul>
</body>
</html>
```

#### Setting up Routes
Create a new file named ```routes.php``` in the ```src/``` directory. This file will define the routes for your application.

```
<?php

use MVC\Router;
use MVC\Controllers\UserController;

$router = new Router();

$router->addRoute('/', UserController::class, 'index');
    
```

#### Testing the Application
Create a new file named ```index.php``` in the root directory of your project. This file will serve as the entry point for your application.
```<?php

require 'vendor/autoload.php';

$uri = $_SERVER['REQUEST_URI'];

$router = require 'src/routes.php';
$router->dispatch($uri);
```
Run the built-in PHP web server to test your application:
```
php -S localhost:8000
```
Vist ```http://localhost:8000``` in your web browser to see the user list.

---
Credit: ```https://reintech.io/blog/building-php-mvc-framework-from-scratch```