# SpamGuard

Guarding form requests against bots.


## Install

```bash
composer require fungku/spamguard
```

Add the service provider to `config/app.php` in the `providers` array:

```php
Fungku\SpamGuard\Providers\SpamGuardServiceProvider::class,
```

Add the alias to `config/app.php` in the `aliases` array:

```php
'SpamGuard' => Fungku\SpamGuard\Facades\SpamGuard::class,
```

## Config (optional)

If you'd like to change the default package config values, then publish the config and change the defaults...

```bash
php artisan vendor:publish --provider="Fungku\SpamGuard\Providers\SpamGuardConfigServiceProvider" --tag="config"
```

## Usage

To use the spamguard, there are two things you need to do.

### 1. Add the *SpamGuard* form elements in your form

Somewhere inside your form, just use `SpamGuard::html()`.

Using all spam guard elements:

```html
<form action="/some-route/action">

    {!! SpamGuard::html() !!}
    
    <!-- other form elements -->
    
</form>
```

Specifying specific elements:

```html
<form action="/some-route/action">

    {!! SpamGuard::html(['spam_honeypot', 'spam_timer']) !!}
    
    <!-- other form elements -->
    
</form>
```

### 2. Add the *SpamGuard* middleware to your route or controller

Using the helper function to assign all spam middleware to all actions:

```php
class MyController extends Controller
{
    public function __construct()
    {
        \SpamGuard::middleware($this);
    }
}
```

Using the helper function to assign all spamguard middleware to only the `update` and `store` actions:

```php
\SpamGuard::middleware($this, ['only' => ['update', 'store']]);
```

Using the helper function to assign specific middleware to only the `update` and `store` actions:

```php
\SpamGuard::middleware(
    $this,
    ['only' => ['update', 'store']],
    ['spam_timer', 'spam_honeypot']
);
```

Or you can just use controller middleware normally.

Specifying specific middlewares:

```php
class MyController extends Controller
{
    public function __construct()
    {
        $this->middleware('spam_honeypot');
        
        $this->middleware('spam_timer');
    }
}
```

Using all spam middleware:

```php
$this->middleware('spamguard');
```

Using the `spam_timer` middleware in a controller normally and overriding the `min_time` and `max_time` for a specific action:

```php
$this->middleware('spam_timer:10,300', ['only' => 'postComment']);
```

## Options

Currently there are two spam middleware: `spam_honeypot` and `spam_timer`.

## Notes

If you are selectively using elements and middleware, please note that the elements you use must match up with the
middleware you assign to the routes.

## Plans

I will look into adding some more baked-in goodness, like Akismet or something else in the future if anyone other than
myself ends up using this package (or maybe I will do it anyway).
