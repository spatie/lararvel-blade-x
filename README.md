**THIS PACKAGE IS IN DEVELOPMENT, DO NOT USE IN PRODUCTION YET**

# Use custom html components in your Blade views

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/laravel-blade-x.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-blade-x)
[![Build Status](https://img.shields.io/travis/spatie/laravel-blade-x/master.svg?style=flat-square)](https://travis-ci.org/spatie/laravel-blade-x)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/laravel-blade-x.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/laravel-blade-x)
[![StyleCI](https://github.styleci.io/repos/150733020/shield?branch=master)](https://github.styleci.io/repos/150733020)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-blade-x.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-blade-x)

This package provides an easy way to render custom html components in your Blade views.

Here's an example. Instead of this:

```blade
<h1>My view</h1>

@include('myAlert', ['type' => 'error', 'message' => $message])
```

you can write this

```blade
<h1>My view</h1>

<my-alert type="error" message="{{ $message }}" />
```

You can place the content of that alert in a simple blade view that needs to be [registered](https://github.com/spatie/laravel-blade-x#usage) before using the `my-alert` component.

```blade
{{-- resources/views/components/myAlert.blade.php --}}

<div class="{{ $type }}">
   {{ $message }}
</div>
```

## Installation

You can install the package via composer:

```bash
composer require spatie/laravel-blade-x
```

The package will automatically register itself.

## Usage

The contents of a component can be stored in a simple Blade view.

```blade
{{-- resources/views/components/myAlert.blade.php --}}

<div class="{{ $type }}>
   {{ $message }}
</div>
```

Before using that component you must first register it. Typically you would do this in the `AppServiceProvider` or a service provider of your own

```php
BladeX::component('my-alert', 'components.myAlert')
```

You can also register an entire directory like this.

```php
// This will register all Blade views that are stored in `resources/views/components`

BladeX::components('components')
```

In your Blade view you can now use the component using the kebab-cased name:

```blade
<h1>My view</h1>

<my-alert type="error" message="{{ $message }}" />
```

### Using slots

BladeX support slots too. This layout component

```blade
{{-- resources/views/components/layout.blade.php --}}

<div>
    <h1>{{ $title }}</h1>
    <div class="flex">
        <div class="w-1/3">
            {{ $sidebar }}
        </div>
        <div class="w-2/3">
            {{ $slot }}
        </div>
    </div>
    <footer>
        {{ $footer }}
    </footer>
</div>
```

can be used in your views like this:

```blade
<layout title="Zed's chopper">
    <slot name="sidebar">
        <ul>
            <li>Home</li>
            <li>Contact</li>
        </ul>
    </slot>

    <main class="content">Whose motorcycle is this?</main>

    <slot name="footer">It's not a motorcycle honey, it's a chopper.</slot>
</layout>
```

### Prefixing components

If you're using Vue components in combination with BladeX components, it might be worth prefixing your BladeX components to make them easily distinguishable from the rest.
 
Setting a global prefix can easily be done before or after registering components:

```php
BladeX::component('my-alert', 'components.myAlert');

BladeX::prefix('x');
```

All your registered components can now be used like this:

```blade
<x-my-alert message="Notice the prefix!" />
``` 

## Under the hood

When you register a component 

```php
BladeX::component('my-alert', 'components.myAlert')
```
with this html

```blade
{{-- resources/views/components/myAlert.blade.php --}}
<div class="{{ $type }}">
   {{ $message }}
</div>
```

and use it in your Blade view like this,

```blade
<my-alert type="error" message="{{ $message }}" />
```

our package will replace that html in your view with this:

```blade
@component('components/myAlert', ['type' => 'error','message' => $message,])@endcomponent
```

After that conversion Blade will compile (and possibly cache) that view.

Because all this happens before any html is sent to the browser, client side frameworks like Vue.js will never see the original html you wrote (with the custom tags).


## Testing

``` bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Postcardware

You're free to use this package, but if it makes it to your production environment we highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Samberstraat 69D, 2060 Antwerp, Belgium.

We publish all received postcards [on our company website](https://spatie.be/en/opensource/postcards).

## Credits

- [Sebastian De Deyne](https://github.com/sebdedeyne)
- [Brent Roose](https://github.com/brendt)
- [Alex Vanderbist](https://github.com/alexvanderbist)
- [Ruben Van Assche](https://github.com/rubenvanassche)
- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

## Support us

Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

Does your business depend on our contributions? Reach out and support us on [Patreon](https://www.patreon.com/spatie). 
All pledges will be dedicated to allocating workforce on maintenance and new awesome stuff.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
