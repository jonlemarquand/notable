---
title: SymfonyCasts PHP4 - PHP Namespaces
created: '2020-07-19T12:04:25.934Z'
modified: '2020-07-19T13:00:49.156Z'
---

# SymfonyCasts PHP4 - PHP Namespaces

```php

//Foo.php

<?php

namespace Acme\Tools

class Foo
{

  public function doAwesomeThings()
  {
    echo "Hi Foo!\n";
  }

}

//some-other-file.php

<?php

require 'Foo.php';

$foo = new \Acme\Tools\Foo();

$foo->doAwesomeThings();

```
* Usually the namespace of a class matches its directory, but doesn't have to.
* Putting a class in a namespace, it's like putting it in a directory.
  * To reference it you can use `$foo = new \Acme\Tools\Foo();`
* The use Statement
  * At the top of the file use `use Acme\Tools\Foo as SomeFooClass;`
  * A bit like React.
* Core PHP classes don't live in a namespace.
  * This means they live at the root namespace.
* Namespaces work like directories
  * If we use DateTime inside the Acme\Tools it will break.
  * You can use either `use DateTime;` or `\DateTime`.

