# Filesystem

## The `fs()` function

The `fs()` function returns an object that provides OS-independent utilities for
filesystem operations and for file/directory paths manipulation. It returns an
instance of `Symfony\Component\Filesystem\Filesystem`.

You can also use static methods of the class
`Symfony\Component\Filesystem\Path`:

```php
use Castor\Attribute\AsTask;
use Symfony\Component\Filesystem\Path;

use function Castor\fs;

#[AsTask()]
function foo()
{
    $dir = '/tmp/foo';

    echo $dir, ' directory exist: ', fs()->exists($dir) ? 'yes' : 'no', \PHP_EOL;

    fs()->mkdir($dir);
    fs()->touch($dir . '/bar.md');

    echo $dir, ' is an absolute path: ', Path::isAbsolute($dir) ? 'yes' : 'no', \PHP_EOL;
    echo '../ is an absolute path: ', Path::isAbsolute('../') ? 'yes' : 'no', \PHP_EOL;

    fs()->remove($dir);

    echo 'Absolute path: ', Path::makeAbsolute('../', $dir), \PHP_EOL;
}
```

> [!NOTE]
> You can check the
> [Symfony documentation](https://symfony.com/doc/current/components/filesystem.html)
> for more information about this component and how to use it.

## The `finder()` function

The `finder()` function returns an object that finds files and directories based
on different criteria (name, file size, modification time, etc.) via an
intuitive fluent interface. It returns an instance of
`Symfony\Component\Finder\Finder`:

```php
use Castor\Attribute\AsTask;

use function Castor\finder;

#[AsTask()]
function foo()
{
    echo 'Number of PHP files: ', finder()->name('*.php')->in(__DIR__)->count(), \PHP_EOL;
}
```

> [!NOTE]
> You can check the
> [Symfony documentation](https://symfony.com/doc/current/components/finder.html)
> for more information about this class and how to use it.
