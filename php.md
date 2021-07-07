# Things I learned in PHP while learning Laravel

## Getting to know which classes implement a particular interface

```php
foreach (get_declared_classes() as $className) {
    if (in_array('Iterator', class_implements($className))) {
        echo $className, PHP_EOL;
    }
}
```

## Dynamically instantiating a class (namespacing properly)

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
