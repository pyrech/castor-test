# Task arguments

When creating a function that will be used as a task, all the parameters of
the function will be used as arguments or options:

```php
use Castor\Attribute\AsTask;

use function Castor\run;

#[AsTask()]
function task(
    string $firstArg,
    string $secondArg
) {
    run(['echo', $firstArg, $secondArg]);
}
```

Which can be called like that:

```bash
$ castor task foo bar
foo bar
```

## Optional arguments

You can make an argument optional by giving it a default value:

```php
use Castor\Attribute\AsTask;

use function Castor\run;

#[AsTask()]
function task(
    string $firstArg,
    string $default = 'default'
) {
    run(['echo', $firstArg, $secondArg]);
}
```

```bash
$ castor task foo
foo default
$ castor task --default=bar foo
foo bar
```

> [!TIP]
> Related example: [args.php](https://github.com/jolicode/castor/blob/main/examples/args.php)

## Overriding the argument name and description

You can override the name and description of an argument by using
the `Castor\Attribute\AsArgument` attribute:

```php
use Castor\Attribute\AsArgument;
use Castor\Attribute\AsTask;

use function Castor\run;

#[AsTask()]
function command(
    #[AsArgument(name: 'foo', description: 'This is the foo argument')]
    string $arg = 'bar',
) {
    run(['echo', $arg]);
}
```

```bash
$ castor command foo
foo
```

> [!TIP]
> Related example: [args.php](https://github.com/jolicode/castor/blob/main/examples/args.php)

## Overriding the option name and description

If you prefer, you can force an argument to be an option by using the
`Castor\Attribute\AsOption` attribute:

```php
use Castor\Attribute\AsOption;
use Castor\Attribute\AsTask;

use function Castor\run;

#[AsTask()]
function command(
    #[AsOption(name: 'foo', description: 'This is the foo option')]
    string $arg = 'bar',
) {
    run(['echo', $arg]);
}
```

```bash
$ castor command --foo=foo
foo
```

You can also configure the `mode` of the option. The `mode` determines how the
option must be configured:

```php
use Castor\Attribute\AsOption;
use Castor\Attribute\AsTask;

use function Castor\run;

#[AsTask()]
function command(
    #[AsOption(description: 'This is the foo option', mode: InputOption::VALUE_NONE)]
    bool $force,
) {
    if ($force) {
        echo "command has been forced\n";
    }
}
```

```bash
$ castor command --force
command has been forced
```

> [!TIP]
> Related example: [args.php](https://github.com/jolicode/castor/blob/main/examples/args.php)

---

Please refer to the [Symfony
documentation](https://symfony.com/doc/current/console/input.html#using-command-options)
for more information.
