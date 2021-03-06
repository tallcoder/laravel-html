# Painless html generation

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/laravel-html.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-html)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/spatie/laravel-html/master.svg?style=flat-square)](https://travis-ci.org/spatie/laravel-html)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/laravel-html.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/laravel-html)
[![StyleCI](https://styleci.io/repos/78114062/shield?branch=master)](https://styleci.io/repos/78114062)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-html.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-html)

This package helps you generate HTML using a clean, simple and easy to read API. All elements can be dynamically generated and put together. The HTML builder helps you generate dynamically assigned form elements based on your selected model, the session or a default value.

### Documentation

You'll find full documentation [here](https://docs.spatie.be/laravel-html).

### Generating elements

For example creating a new `span` element with a class is super easy with the [fluent methods for elements](/laravel-html/v1/general-usage/element-methods):

 ```php
html()->span()->text('Hello world!')->class('fa fa-eye');
```

### Building forms

Here's a quick example that builds a basic form with an e-mail input:

```php
{{ html()->form('PUT', '/post')->open() }}

{{ html()->email('email')->placeholder('Your e-mail address') }}

{{ html()->form()->close() }}
```

The generated HTML will look like this:

```html
<form method="POST" action="/post">
    <input type="hidden" name="_method" id="_method" value="PUT">
    <input type="hidden" name="_token" id="_token" value="csrf_token_will_be_here">
    <input type="email" name="email" id="email" placeholder="Your e-mail address">
</form>
```

Notice how the hidden `_method` and `_token` fields were automatically added and filled? You'll never forget to add `csrf_field()` again because now you simply wont have to anymore!

Another common use case might be to fill an input element based on the value that was previously submitted (using `$request->old()`). Worry no more, this has been taken care of as well. The above code will automatically fill in the `email` field if `$session->old('email')` exists. Amazing.

### Models in the HTML builder

The HTML builder can also generate elements based on a model:

```php
{{ html()->model($user) }}

{{ html()->input('name') }}
```

The value of the `name` field will automatically be filled with the model's `name` property if available:

```html
<input type="text" name="name" id="name" value="John">
```

A "model" can be any object that implements `ArrayAccess` — anything from a complex Eloquent model to a plain array.

## Postcardware

You're free to use this package (it's [MIT-licensed](LICENSE.md)), but if it makes it to your production environment we highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Samberstraat 69D, 2060 Antwerp, Belgium.

All postcards are published [on our website](https://spatie.be/en/opensource/postcards).

## Installation

You can install the package via composer:

``` bash
composer require spatie/laravel-html
```

## Usage

### Concepts

Elements—classes under the `Spatie\Html\Elements` namespace—are generally created via a `Spatie\Html\Html` builder instance.

```php
$html = app(Html::class);

$html->span()->text('Hello world!');
```

Element attributes and contents are modified via with fluent methods which return a new instance. This means element instances are immutable.

```php
$icon = $html->span()->class('fa');

$icon->class('fa-eye'); // '<span class="fa fa-eye"></span>'
$icon->class('fa-eye-slash'); // '<span class="fa fa-eye-slash"></span>'
```

Element classes don't have any knowledge of the outside world. Any coupling to other concepts, like requests and sessions, should happen in the builder class, not on the element classes.

By convention, we assume that builder methods will modify values to our advantage (like pulling old values from the session on a failed form request), and element methods will be deterministic.

```php
// This will try to resolve an initial value, and fall back to 'hello@example.com'
$email = $html->email('email', 'hello@example.com');

// This will always have 'hello@example.com' as it's value
$email = $html->email('email')->value('hello@example.com');
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Credits

- [Sebastian De Deyne](https://github.com/sebastiandedeyne)
- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

## About Spatie
Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
