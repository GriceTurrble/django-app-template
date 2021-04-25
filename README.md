# Custom Django app template

Custom app template to use in Django's `startapp` command.

## What is this?

By default, Django's [`startapp`][startapp_command] management command will create a new app by using their [built-in app template][django_default_template]. Files and sub-directories are copied, with files being passed through Django's template language system in order to generate app-specific details.

These templates are barebones and, for my tastes, need a fair amount of work to get to a starting point where they're actually usable.

Thankfully, the `startapp` command takes an optional `--template` argument, which can accept either a directory with template files or an archive containing those files. The contents of the `app_template/` directory you see in *this* repo are an example of those template files.

## What's included

### New modules

- `urls.py`: Though not required for an app to function, I tend to build apps into a site that include their own URL patterns, so this is a standard addition for me. Includes the basic components to start adding paths to the `urlpatterns` list, as well as [`app_name`][django_url_namespacing].
- `signals.py`: Often when folks get started with signals, they will tuck them into `models.py`, which is [discouraged][django_doc_signal_receivers]. I opt to standardize the module where these signals live.
  - As noted below, the app's `AppConfig` is also modified to include the import for this signals module by default, which makes wiring up the signal receivers easier.
  - If `signals.py` is removed from the app after the fact, its import should also be removed from the AppConfig's `ready` method.

### Changes

- All modules:
  - Modules include examples (commented out) for standard objects that might be included and links to interesting documentation.
  - Modules, classes, and functions include basic [PEP 257]-compliant docstrings
    - In some projects, I use a pre-commit hook that checks for this. The templated docstrings *should* make any new app pass such a test.
  - Files have been formatted with [Black] ahead of time: running Black on the newly-created files *should* yield no changes.
- `apps.py`:
  - Adds the `ready` method, which for now simply imports the `signals.py` module to allow its receiver functions to be registered on app startup.
- `models.py`:
  - A warning states [not to use Django's built-in `User` model directly][learndjango_user_model_ref], but to call `django.contrib.auth.get_user_model`, instead.
  - `get_user_model` is also called at the top level of the module to obtain the `User` class as an example.
- `views.py`:
  - *Coming soon* Some best-practice examples for using Class-Based Views will be embedded into this module template in a future release.

## Usage

See [Django docs for the `startapp` command][startapp_command] for details. You will need to use the `--template` argument to pass the new template in place of Django's default.

### Option 1: use the asset URL

Copy the below command to use the ZIP archive direct from the Releases on this repo:

```shell
$ python manage.py startapp --template https://github.com/GriceTurrble/django-app-template/releases/download/v0.2.0/app_template.zip myapp
```

### Option 2: download and use `app_template.zip` locally

1. [Download the latest `app_template.zip`][release_zip] and save it wherever you like.
1. Use the path to this archive in the `--template` argument for the `startapp` management command:

```shell
$ python manage.py startapp --template path/to/app_template.zip myapp
```

You can also un-ZIP the archive to a directory within your project, then point to that directory:

```shell
$ python manage.py startapp --template path/to/app_template_dir/
```

Doing so will also allow you to customize the template to your own project's needs, useful if you need to add lots of apps to the same site.

## Release contents

While the Django docs specifically mention using a GitHub archive ZIP as an option (choosing the "Download repo as ZIP" option), doing so will also copy this README, the LICENSE, and other files that you don't want mucking up your project space.

Instead, Releases on this repo include an `app_template.zip`, which contains only the file contents of the [`app_template/`](app_template/) directory. This provides a clean starting point for your new app.

[startapp_command]: https://docs.djangoproject.com/en/3.2/ref/django-admin/#startapp
[django_default_template]: https://github.com/django/django/tree/master/django/conf/app_template
[django_url_namespacing]: https://docs.djangoproject.com/en/3.2/topics/http/urls/#url-namespaces-and-included-urlconfs
[django_doc_signal_receivers]: https://docs.djangoproject.com/en/3.2/topics/signals/#connecting-receiver-functions
[PEP 257]: https://www.python.org/dev/peps/pep-0257/
[learndjango_user_model_ref]: https://learndjango.com/tutorials/django-best-practices-referencing-user-model
[Black]: https://black.readthedocs.io/en/stable/
[release_zip]: https://github.com/GriceTurrble/django-app-template/releases/download/v0.2.0/app_template.zip
