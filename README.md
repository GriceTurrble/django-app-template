# Custom Django app template

Custom app template to use in Django's `startapp` command.

## What is this?

By default, Django's [`startapp`][1] management command will create a new app by using their [built-in app template][2]. Files and sub-directories are copied, with files being passed through Django's template language system in order to generate app-specific details.

These templates are barebones and, for my tastes, need a fair amount of work to get to a starting point where they're actually usable.

Thankfully, the `startapp` command takes an optional `--template` argument, which can accept either a directory with template files or an archive containing those files. The contents of the `app_template/` directory you see in *this* repo are an example of those template files.

## What's included

### New modules

- `urls.py`: Though not required for an app to function, I tend to build apps into a site that include their own URL patterns, so this is a standard addition for me. Includes the basic components to start adding paths to the `urlpatterns` list, as well as [`app_name`][3].
- `signals.py`: Often when folks get started with signals, they will tuck them into `models.py`, which is [discouraged][4]. I opt to standardize the module where these signals live.
  - As noted below, the app's `AppConfig` is also modified to include the import for this signals module by default, which makes wiring up the signal receivers easier.
  - If `signals.py` is removed from the app after the fact, its import should also be removed from the AppConfig's `ready` method.

### Changes

- All modules:
  - Modules include examples (commented out) for standard objects that might be included and links to interesting documentation.
  - Modules, classes, and functions include basic [PEP 257][5]-compliant docstrings
    - In some projects, I use a pre-commit hook that checks for this. The templated docstrings *should* make any new app pass such a test.
- `apps.py`:
  - includes an example of the string that should be used in the `INSTALLED_APPS` setting to add the new app.
  - adds the `ready` method, which for now simply imports the `signals.py` module to allow its receiver functions to be register on app startup.
- `models.py`:
  - A warning states [not to use Django's built-in `User` model directly][6], but to call `django.contrib.auth.get_user_model`, instead.
  - `get_user_model` is also called at the top level of the module to obtain the `User` class as an example.
- `views.py`:
  - *Coming soon* Some best-practice examples for using Class-Based Views will be embedded into this module template in a future release.

## Usage

1. Navigate to this repo's **Releases** section.
1. Find the latest release (currently `v0.0.1`).
1. Copy the download link for the `app_template.zip` asset attached to that release.
1. Use the URL for this asset when running the `startapp` command, by including a `--template` flag like so:

```shell
$ python manage.py startapp --template https://link/to/app_template.zip myapp
```

See [Django docs specific to the `--template` flag][1] for details on how this works.

## New releases

While the Django docs specifically mention using a GitHub archive ZIP as an option (choosing the "Download repo as ZIP" option), doing so will also copy this README, the LICENSE, and other files that you don't want mucking up your project space.

Instead, this repo includes some GitHub Actions that will automatically create a new Release when tags are added starting with `v` to indicate a new release version. The contents of `app_template/` are then ZIPped and added to that Release, providing a clean artifact to use as a template.

Feel free to download the `app_template.zip` asset to save locally to your project, which will allow you to more easily start new apps without needing to copy the URL for it each time.

[1]: https://docs.djangoproject.com/en/3.0/ref/django-admin/#startapp
[2]: https://github.com/django/django/tree/master/django/conf/app_template
[3]: https://docs.djangoproject.com/en/3.0/topics/http/urls/#url-namespaces-and-included-urlconfs "Django docs: URL namespaces and included URLconfs"
[4]: https://docs.djangoproject.com/en/3.0/topics/signals/#connecting-receiver-functions "Django docs: Connecting receiver functions"
[5]: https://www.python.org/dev/peps/pep-0257/ "PEP 257 -- Docstring conventions"
[6]: https://learndjango.com/tutorials/django-best-practices-referencing-user-model "blog: Django Best Practices: Referencing the User Model"
