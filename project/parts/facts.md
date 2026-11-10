Drupal 11 site for <what it is and who uses it, 2-3 lines>. Custom modules in `web/modules/custom/` · theme in
`web/themes/custom/<theme_name>/` · config sync in `config/sync/` · DDEV local.

## Critical Conventions
- Custom modules use the `actg_` prefix (e.g. `actg_certificates`). Never create a custom module without it.
- Never edit `web/core`, `web/modules/contrib`, `web/themes/contrib` or `vendor/`. Contrib fixes go through composer patches.
- Configuration is exported with `ddev drush cex` and imported with `ddev drush cim`. Never hand-edit `config/sync/` except for trivial, reviewed changes.
- Business logic lives in services (`src/Service/`) with dependency injection. Hooks and controllers only delegate. Avoid static `\Drupal::` calls inside classes.
- Schema or data changes need an update hook (`hook_update_N`) in the module's `.install` file; never rely on re-installing.
- Every user-facing string is English in code and wrapped in `t()` / `$this->t()`; translations live in `.po` files.
- Access control: use permissions and access handlers, never hardcode role names or user IDs.
- Code must pass `phpcs` (Drupal + DrupalPractice) and `phpstan` before committing (`/gate`).
- Dependencies are managed with composer only (`ddev composer require`); commit `composer.lock`.
