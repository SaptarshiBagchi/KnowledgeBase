# Things I learned in PHP while learning Laravel

1. [Know which classes implement a particular interface](#getting-to-know-which-classes-implement-a-particular-instance)
2. [Dynamically instantiating a class](#dynamically-instantiating-a-class)
3. [Laravel .htaccess](#laravel-.htaccess)

## Getting to know which classes implement a particular interface

```php
foreach (get_declared_classes() as $className) {
    if (in_array('Iterator', class_implements($className))) {
        echo $className, PHP_EOL;
    }
}
```

## Dynamically instantiating a class

1. Normal loading as we all know

```php
class BicycleModel{
  //@ToDo: implement BicycleModel
}

class CarModel{
  //@ToDo: implement CarModel
}

// Normal Class Loading
$myTodayChoice = new BicycleModel();

// Dynamic Class Loading
$vehicleName = 'Bicycle';
// You should first concatenate $vehicleName & Model, then use resulting variable for creating class instance.
// Keep in mind that you're not allowed to merge class instantiation & concatenation.
// $myTodayChoice - new $vehicleName.'Model'(); IS NOT VALID
$className = $vehicleName . 'Model';
$myTodayChoice = new $className();
```

2. Declaring the proper name spaces

```php
/* File Component/Package/Vehicle/BicycleModel.php */

namespace Component\Package\Vehicle;

class BicycleModel{
  //@ToDo: implement BicycleModel
}
------------------------------------------------------

/* File Component/Package/Vehicle/CarModel.php */

namespace Component\Package\Vehicle;

class CarModel{
  //@ToDo: implement CarModel
}
------------------------------------------------------

/* File Component/Package/Vehicle/VehicleFactory.php */

namespace Component\Package\Vehicle;

class VehicleFactory
{
  private $vehicleName;

  public function __construct($vehicle = 'Bicycle'){
    $this->$vehicleName = $vehicle;
  }

  public function getInstance(){
  $className = 'Component\\Package\\Vehicle\\' . $this->$vehicleName . 'Model';
  return new $className();
  }
}
```

## Laravel .htaccess

```
<IfModule mod_rewrite.c>
<IfModule mod_negotiation.c>
    Options -MultiViews
</IfModule>

RewriteEngine On
# force https
   
RewriteCond %{HTTPS} !on
RewriteRule ^.*$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]


RewriteCond %{REQUEST_FILENAME} -d [OR]
RewriteCond %{REQUEST_FILENAME} -f
RewriteRule ^ ^$1 [N]

RewriteCond %{REQUEST_URI} (\.\w+$) [NC]
RewriteRule ^(.*)$ public/$1

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ server.php
</IfModule>
```

## Laravel Super charged queries

1. Model::query()
2. Relationship conditioning
3. WhereHas
4. Dynamically generated attributes
5. WhereMonth
https://www.youtube.com/watch?v=TuPdEbEBvo0
