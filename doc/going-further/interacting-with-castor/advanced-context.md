# Advanced Context usage

## Disabled tasks according to the context

You can disable a task according to the context by using the
`AsTask::enabled` argument:

```php
use Castor\Attribute\AsTask;

#[AsTask(description: 'Say hello, but only in production', enabled: "var('production') == true")]
function hello(): void
{
    echo "Hello world!\n";
}
```

The value can be one of:

* `true`: always enabled (default value)
* `false`: always disabled
* a string: it represents an expression that will be evaluated in the context of
  the task and **must return a bool**. The task will be enabled if the
  expression returns `true` and disabled otherwise. The expression can use the
  `var()` function to get the value of a variable. Internally, it uses the
  [symfony/expression-language](https://symfony.com/doc/current/components/expression_language.html)
  component.

## Getting a specific context

You can get a specific context by its name using the `context()` function:

```php
use Castor\Attribute\AsContext;
use Castor\Context;

use function Castor\run;

#[AsContext(name: 'my_context')]
function create_my_context(): Context
{
    return new Context(['foo' => 'bar'], currentDirectory: '/tmp');
}

#[AsTask()]
function foo(): void
{
    $context = context('my_context');

    run(['echo', $context['foo']]); // will print bar even if you do not use the --context option
    run('pwd', context: $context); // will print /tmp
}
```

## The `with()` function

You may want to run a bunch of commands inside a specific directory or with a
specific context. Instead of passing those parameters to each run, you can use
the `with()` function:

```php
use Castor\Attribute\AsContext;
use Castor\Context;

use function Castor\run;
use function Castor\with;

#[AsContext(name: 'my_context')]
function create_my_context(): Context
{
    return new Context(['foo' => 'bar'], currentDirectory: '/tmp');
}

#[AsTask()]
function foo(): void
{
    with(function (Context $context) {
        run(['echo', $context['foo']]); // will print bar even if you do not use the --context option
        run('pwd'); // will print /tmp
    }, context: 'my_context');
}
```

## The `AsContextGenerator()` attribute

In some case, you may want to programmatically define contexts. You can use the
`AsContextGenerator()` attribute:

```php
use Castor\Attribute\AsContextGenerator;
use Castor\Context;

/**
 * @return iterable<string, \Closure(): Context>
 */
#[AsContextGenerator()]
function context_generator(): iterable
{
    yield 'dynamic' => fn () => new Context([
        'name' => 'dynamic',
        'production' => false,
        'foo' => 'baz',
    ]);
}
```
