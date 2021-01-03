---
title: SymfonyCasts PHP5 - Dependency Injection
created: '2020-07-19T12:11:40.196Z'
modified: '2020-07-19T19:33:08.072Z'
---

# SymfonyCasts PHP5 - Dependency Injection

## Services and Dependency Injection

* A service is any PHP class that performs an action.

```php

class FriendHarvester
{
  private $pdo;

  public function __construct($pdo)
  {
    $this->pdo = $pdo;
  }
}

```
* If a class needs an object or configuration, we force that information to be passed into the class.

## Injecting Config & Services and using Interfaces

* You can add more constructor arguments such as `($pdo, array $smtpConfig)`
* Type-hinting:
  * `public function __construct(\PDO $pdo, SmtpMailer $mailer)`
  * If you pass something else in, you'll get a much clearer error message. 
  * It documents the class even further. A developer now knows exactly what methods she can call on these objects. 
  * Third, if you use an IDE, this gives you auto-completion!

### Interfaces

```php

//MailerInterface.php

namespace DiDemo\Mailer;
interface MailerInterface
{
    public function sendMessage($recipientEmail, $subject, $message, $from);
}

// Update SmtpMailer to implement the interface and change the type-hint in FriendHarvester as well:

class SmtpMailer implements MailerInterface
{
}

use DiDemo\Mailer\MailerInterface;
class FriendHarvester
{
    public function __construct(\PDO $pdo, MailerInterface $mailer)
    {
    }
}

```

## Dependency Injection Container

```php

$container = new Pimple();

$container['mailer'] = function() {
  return new SmtpMailer(
    'smtp.SendMoneytoStrangers.com',
    'smtpuser',
    'smtppass',
    465
  );
};

$friendHarvester = new FriendHarvester($pdo, $container['mailer']);

```

Benefits:

  * Wrapping the instantiation of the mailer service in an anonymous function makes its creation lazy.
    * This means that the object isn't created until much later when we reference the mailer service and ask the container to give it to us. 
    * And if we never reference mailer, it's never created at all - saving us time and memory.
  * The container will only create this once and return the original object.
    * If we send many emails, for example, we would only have the one mailer and call it several times.
