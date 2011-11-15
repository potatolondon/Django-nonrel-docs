Django toolbox
==============

Django toolbox is a collection of tools which make dealing with
non-relational databases easier.

Extra fields
------------

Django toolbox adds the following field types: ``ListField``,
``SetField``, ``DictField``, ``BlobField`` and ``EmbeddedModelField``.

App Engine notes
----------------

App Engine has `native support for lists`_. To query for a specific
value in a ListField or SetField, you can simply use an equality filter::

    team = Team(players=['Bob', 'Harry'])
    team.save()

    Team.objects.get(players='Bob')

DictField is implemented as a key to a blob, so you cannot query on a
DictField.

.. _native support for lists: http://code.google.com/appengine/docs/python/datastore/typesandpropertyclasses.html#ListProperty
