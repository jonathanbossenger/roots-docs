---
date_modified: 2023-02-08 09:46
date_published: 2021-10-21 13:21
description: Acorn automatically handles logging and rendering thrown exceptions in dev. Logs default to the the `storage/logs`
title: Error Handling
authors:
  - ben
  - jure
  - Log1x
---

# Error Handling

[[toc]]

## Introduction

When working in a development environment, Acorn automatically registers an exception handler configured to handle logging as well as the rendering of thrown exceptions. This can be especially useful when diagnosing errors thrown by Blade views.

## Configuration

The `debug` option in your `config/app.php` is solely responsible for whether or not an exception is handled by Acorn. [By default](https://github.com/roots/acorn/blob/ad4f632dca909e09ef2783b8a2b8e3ce40334bcd/config/app.php#L46), this option is set to be enabled when `WP_DEBUG && WP_DEBUG_DISPLAY` are enabled in your WordPress config.

During local development, it is highly advised to ensure that `WP_DEBUG` is enabled to properly render exceptions thrown by Blade views and other errors further down the stack.

## The exception handler

By default, Acorn utilizes the Symfony exception handler. This provides an easier to read stack trace on the errors thrown in your application. While this generic error screen is useful, we recommend using [Ignition](https://github.com/spatie/laravel-ignition) during development.

### Ignition

If you're familiar with Laravel, you may have heard of Ignition. Ignition provides a beautiful and helpful error page for your application.

Not only does it provide useful hints pertaining to your error, it also allows you to publicly share your error using [Flare](https://flareapp.io/). This can be very useful when it comes to submitting bug reports or requesting help on Discourse.

To use Ignition, simply require it alongside Acorn as a dev-dependency:

```shell
$ composer require spatie/laravel-ignition --dev
```

::: warning
PHP >= 8.1 is a requirement to use Ignition with Acorn v3
:::


### Reporting exceptions

Exception reporting can be used to log exceptions to storage or send them to an external service such as Sentry. By default, exceptions will be logged to disk located in the `storage/logs` folder.

Check out the documentation on [logging](logging.md) to learn more about log implementation.

### Disabling the exception handler

Acorn's `acorn/throw_error_exception` filter can be used to disable the exception handler:

```php
add_filter('acorn/throw_error_exception', '__return_false');
```