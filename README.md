# Scout Lumen APM Agent

Monitor the performance of PHP Lumen applications with Scout's PHP APM Agent.
Detailed performance metrics and transaction traces are collected once the scout-apm package is installed and configured.

## Requirements

* PHP Version: PHP 7.1+
* Lumen Version: 5.5+

## Quick Start

A Scout account is required. [Signup for Scout](https://scoutapm.com/users/sign_up).

```bash
composer require scoutapp/scout-apm-lumen
```

Add the `ScoutApmServiceProvider` to your `bootstrap/app.php`, for example:

```php
// $app->register(App\Providers\AppServiceProvider::class);
// $app->register(App\Providers\AuthServiceProvider::class);
// $app->register(App\Providers\EventServiceProvider::class);
$app->register(\Scoutapm\Laravel\Providers\ScoutApmServiceProvider::class);
```

Add the four middleware to your `bootstrap/app.php`, for example:

```php
$app->middleware([
    // These three should be as early as possible in the pipeline:
    \Scoutapm\Laravel\Middleware\SendRequestToScout::class,
    \Scoutapm\Laravel\Middleware\IgnoredEndpoints::class,
    \Scoutapm\Laravel\Middleware\MiddlewareInstrument::class,
    
    // The rest of your middleware should go here
    // ...
    
    // This middleware should come last
    \Scoutapm\Laravel\Middleware\ActionInstrument::class,
]);
```

If you wish to use the `\Scoutapm\Laravel\Facades\ScoutApm` facade, you should also enable facades if you did not
already by uncommenting the `withFacades()` call in `bootstrap/app.php`:

```php
$app->withFacades();
```

### Configuration

In your `.env` file, make sure you set a few configuration variables:

```
SCOUT_KEY=ABC0ZABCDEFGHIJKLMNOP
SCOUT_NAME="My Laravel App"
SCOUT_MONITOR=true
```

Your key can be found in the [Scout organization settings page](https://scoutapm.com/settings).

#### Logging Verbosity

Once you have set up Scout and are happy everything is working, you can reduce the verbosity of the library's logging
system. The library is intentionally *very* noisy by default, which gives us more information to support our customers
if something is broken. However, if everything is working as expected, these logs can be reduced by setting the
`log_level` configuration key to a higher `Psr\Log\LogLevel`. For example, if you are using `.env` configuration:

```
SCOUT_LOG_LEVEL=error
```

Any of the constants defined in `\Psr\Log\LogLevel` are acceptable values for this configuration option.

## Documentation

For full installation and troubleshooting documentation, visit our [help site](https://docs.scoutapm.com/#lumen).

## Support

Please contact us at support@scoutapm.com or create an issue in this repo.
