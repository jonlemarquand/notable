---
title: CWD9 - MySQL
created: '2020-02-24T07:49:03.000Z'
modified: '2020-02-24T09:34:20.334Z'
---

# CWD9 - MySQL

## Session Variables

* How we keep users signed in to the website.
* To use session variables we have to start our session with:

```php

session_start();
$_SESSION['username'] = "robpercival";

echo $_SESSION['username'];

```

* This will see if there is currently a session with that username.
* Don't put HTML code before `session_start();`
* To see this in a form:

```php

if (mysqli_query($link, $query)) {

    $_SESSION('email') = $_POST['email'];

    header("Location: session.php");

} else {
    
    echo "<p>There was a problem signing you up - please try again later.</p>";
}

```


## Cookies

* If you want to keep users logged in for longer (after they've closed the window), you could use cookies.
* To see:

```php

<? php

    setcookie("customerId", "1234", time() + 60 * 60 * 24)

    echo $_COOKIE["cusomterId"];
?>

```

* This would set the cookie to expire after a day.
* To delete a cookie:

```php

    setcookie("customerId", "", time() - 60 * 60);

```

* However, the cookie doesn't expire until the page is reloaded.

## Storing Passwords Securely

* By default the passwords was just stored as plaintext, which means if someone accessed the database they could see usernames/passwords.
* To encrypt the passwords we use hashing.
    * PHP uses md5 - One way encryption.
        * Because this is a unique one-way hash it is easy to find online and crack it.
* $salt - a long string of numbers and letters that is inserted in front of the password.

```php

$salt = "AFASDAFA324234aSda34a"

echo md5($salt."password");

```
* One thing to note with this output is that if you have a lot of usernames/passwords the hackers could eventually be able to work out the salt.
* The next level is to use a unique salt with each password.

```php

$row['id'] = 73;

echo md5(md5($row['id'])."password");

```
