Exposing a problem in Composer single-package update in case a conflicting dependency is found.

## Steps to reproduce

All the `--dry-run` flags are intentional, showing behavior without modifying the filesystem.

Initialize dependencies with

```
composer update
```

Modify composer.json substituting `8.4.6` with `^8.4.6`, following new minor versions of that package.

```
composer update --dry-run drupal/core
```

won't update the package to `8.5.*`.

```
composer update --dry-run drupal/core --with-dependencies
```

also won't update the package.

```
composer update --dry-run drupal/core symfony/config --with-dependencies
```

A blanket

```
composer update --dry-run
```

will update the package, but on a real project it may have a lot of side-effects in updating other unrelated packages.

## Potential reason

`symfony/config` is not a dependency of `drupal/core`.

However, there are conflicts between the new versions of `symfony/dependency-injection` required by `drupal/core@8.5` and the currently installed `symfony/config@v3.2.14`.

Therefore, `symfony/config` has to be whitelisted for the upgrade to happen.
