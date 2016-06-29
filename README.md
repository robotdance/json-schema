# JSON Schema for PHP

[![Code Climate](https://codeclimate.com/github/robotdance/php-json-schema/badges/gpa.svg)](https://codeclimate.com/github/robotdance/php-json-schema)
[![Test Coverage](https://codeclimate.com/github/robotdance/php-json-schema/badges/coverage.svg)](https://codeclimate.com/github/robotdance/php-json-schema/coverage)
[![Issue Count](https://codeclimate.com/github/robotdance/php-json-schema/badges/issue_count.svg)](https://codeclimate.com/github/robotdance/php-json-schema)
[![Build Status](https://travis-ci.org/robotdance/php-json-schema.svg?branch=master)](https://travis-ci.org/robotdance/php-json-schema)

A PHP [json-schema](http://json-schema.org/) implementation for validating JSON data agains a JSON Schema definition.

Originally forked from [justinrainbow/json-schema](https://github.com/justinrainbow/json-schema).In addition to the original, I18n was added for the validation messages, with initial support for en-US and pt-BR.
Currently the branch "add-i18n" will be kept in sync with the forked repository, and the master branch will follow is own way.

## Installation

PHP-Json-Schema is available as a [`Composer`](https://github.com/composer/composer) package, and use it also as its dependency manager.
So you need a composer.json in your project root, requiring php-json-schema:

```json
  ...
  "require": {
    "robotdance/php-json-schema": "latest stable version"
  }
  ...
```

So after updating your composer file, simply update composer.

    $ php composer.phar update
    $ composer update

The way you update with composer will depend on how you installed composer.

## Usage

```php
<?php

// Get the schema and data as objects
// If you use $ref or if you are unsure, resolve those references here
// This modifies the $schema object
$refResolver = new JsonSchema\RefResolver(new JsonSchema\Uri\UriRetriever(), new JsonSchema\Uri\UriResolver());
$schema = $refResolver->resolve('file://' . realpath('schema.json'));

$data = json_decode(file_get_contents('data.json'));

// Validate
$validator = new JsonSchema\Validator();
$validator->check($data, $schema);

if ($validator->isValid()) {
    echo "The supplied JSON validates against the schema.\n";
} else {
    echo "JSON does not validate. Violations:\n";
    foreach ($validator->getErrors() as $error) {
        echo sprintf("[%s] %s\n", $error['property'], $error['message']);
    }
}
```

## Running the tests

    $ vendor/bin/phpunit
