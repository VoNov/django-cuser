========================================================
django-cuser - Take care of current user in silent way.
========================================================

.. image:: https://travis-ci.org/Alir3z4/django-cuser.png
   :alt: travis-cli tests status for django-cuser
   :target: https://travis-ci.org/Alir3z4/django-cuser

.. contents:: Table of contents


Copyright (c) 
 * 2009-2011 Dennis Kaarsemaker <dennis@kaarsemaker.net>
 * 2011 Atamert Ölçgun <muhuk@muhuk.com>
 * 2012, 2013, 2014 Alireza Savand <alireza.savand@gmail.com>

Overview
--------

cuser will bring you Current user of your django application from anywehere in your code.
I know, sounds fantastic ;)

Installing
----------

django-cuser is also avilable at http://pypi.python.org/pypi/django-cuser
So it can be install it by pip or easy_install::

    $ python pip install django-cuser

Or you can grab the latest version tarball::

    $ python setup.py install

To enable django-cuser in your project

* Add ``cuser`` to ``INSTALLED_APPS`` in your ``settings.py``
* Add ``cuser.middleware.CuserMiddleware`` to ``MIDDLEWARE_CLASSES`` after the
  authentication and session middleware.

Who is the current user
-----------------------

To set/get the user info, there is the following API::

    from cuser.middleware import CuserMiddleware

Set the current user for this thread. Accepts user objects and login names::

    CuserMiddleware.set_user(some_user)

Get the current user or None::

    user = CuserMiddleware.get_user()

This will return some_user if there is no current user::

    user = CuserMiddleware.get_user(some_user)

Forget the current user. It is always safe to call this, even if there is no current user::

    CuserMiddleware.del_user()

The middleware automatically sets/deletes the current user for HTTP requests.
For other uses (management commands, scripts), you will need to do this
yourself.

CurrentUserField
-----------------

``cuser`` also provides a ``CurrentUserField``, which can be used for auditing
purposes. Use it as follows:

from cuser.fields import CurrentUserField
::

    class MyModel(models.Model):
        ....
        creator = CurrentUserField(add_only=True, related_name="created_mymodels")
        last_editor = CurrentUserField(related_name="last_edited_mymodels")
        ...

This field is a ``ForeignKey`` to the ``django.contrib.auth.models.User`` model and you
can treat it as such.
