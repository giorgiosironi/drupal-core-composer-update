Exposing a problem in Composer single-package update

## Steps to reproduce

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

## Potential reason

`symfony/config` is not a dependency of `drupal/core`.

However, there are conflicts between the new versions of `symfony/dependency-injection` required by `drupal/core@8.5` and the currently installed `symfony/config@v3.2.14`.

Therefore, `symfony/config` has to be whitelisted for the upgrade to happen.
