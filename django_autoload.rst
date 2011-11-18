Django autoload
===============

django-autoload is a reusable django app which allows for correct initialization of your django project by ensuring the loading of signal handlers or indexes before any request is being processed.

Installation
------------

1. Add django-autoload/autoload to settings.INSTALLED_APPS::

        INSTALLED_APPS = (
            'autoload',
        )

2. Add the autoload.middleware.AutoloadMiddleware before any other middleware::

        MIDDLEWARE_CLASSES = ('autoload.middleware.AutoloadMiddleware', ) + \
                              MIDDLEWARE_CLASSES

How does django-autoload ensure correct initialization of my project?
----------------------------------------------------------------------

django-autoload provides two mechanisms to allow for correct initialization:

1. The autoload.middleware.AutoloadMiddleware middleware will load all models.py from settings.INSTALLED_APPS as soon as the first request is being processed. This ensures the registration of signal handlers for example.
2. django-autoload provides a site configuration module loading mechanism. Therefore, you have to create a site configuration module. The module name has to be specified in your settings::

    # settings.py:
    AUTOLOAD_SITECONF = 'indexes'

Now, django-autoload will load the module indexes.py before any request is being processed. An example module could look like this::

    # indexes.py:
    from dbindexer import autodiscover
    autodiscover()

This will ensure the registration of all database indexes specified in your project.

autoload.autodiscover(module_name)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some apps like django-dbindexer or nonrel-search provide an autodiscover-mechanism which tries to import index definitions from all apps in settings.INSTALLED_APPS. Since the autodiscover-mechanism is so common django-autoload provides an autodiscover function for these apps.

autodiscover will search for [module_name].py in all settings.INSTALLED_APPS and load them. django-dbindexer's autodiscover function just looks like this for example::

    def autodiscover():
        from autoload import autodiscover as auto_discover
        auto_discover('dbindexes')

