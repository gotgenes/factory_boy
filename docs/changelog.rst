ChangeLog
=========

2.0.0 (future)
--------------

.. note:: This section lists features planned for v2 of factory_boy. Changes announced here may not have been committed to the repository.

*New:*

    - Add support for Python3
    - Clean up documentation
    - Document extension points
    - Add support for ``get_or_create`` in :class:`~factory.DjangoModelFactory`

*Deprecation:*

    - Remove associated class discovery
    - Stop defaulting to Django's ``Foo.objects.create()`` when "creating" instances
    - Remove STRATEGY_*
    - Remove :meth:`~factory.Factory.set_building_function` / :meth:`~factory.Factory.set_creation_function`


1.3.0 (current)
---------------

.. warning:: This version deprecates many magic or unexplicit features that will be
             removed in v2.0.0.

             Please read the :ref:`changelog-1-3-0-upgrading` section, then run your
             tests with ``python -W default`` to see all remaining warnings.

New
"""

- **Global:**
    - Rewrite the whole documentation
    - Provide a dedicated :class:`~factory.MogoFactory` subclass of :class:`~factory.Factory`

- **The Factory class:**
    - Better creation/building customization hooks at :meth:`factory.Factory._build` and :meth:`factory.Factory.create`
    - Add support for passing non-kwarg parameters to a :class:`~factory.Factory`
      wrapped class through :attr:`~factory.Factory.FACTORY_ARG_PARAMETERS`.
    - Keep the :attr:`~factory.Factory.FACTORY_FOR` attribute in :class:`~factory.Factory` classes

- **Declarations:**
    - Allow :class:`~factory.SubFactory` to solve circular dependencies between factories
    - Enhance :class:`~factory.SelfAttribute` to handle "container" attribute fetching
    - Add a :attr:`~factory.Iterator.getter` to :class:`~factory.Iterator`
      declarations
    - A :class:`~factory.Iterator` may be prevented from cycling by setting
      its :attr:`~factory.Iterator.cycle` argument to ``False``
    - Allow overriding default arguments in a :class:`~factory.PostGenerationMethodCall`
      when generating an instance of the factory
    - An object created by a :class:`~factory.DjangoModelFactory` will be saved
      again after :class:`~factory.PostGeneration` hooks execution


Pending deprecation
"""""""""""""""""""

The following features have been deprecated and will be removed in an upcoming release.

- **Declarations:**
    - :class:`~factory.InfiniteIterator` is deprecated in favor of :class:`~factory.Iterator`
    - :class:`~factory.CircularSubFactory` is deprecated in favor of :class:`~factory.SubFactory`
    - The ``extract_prefix`` argument to :meth:`~factory.post_generation` is now deprecated

- **Factory:**
    - Usage of :meth:`~factory.Factory.set_creation_function` and :meth:`~factory.Factory.set_building_function`
      are now deprecated
    - Implicit associated class discovery is no longer supported, you must set the :attr:`~factory.Factory.FACTORY_FOR`
      attribute on all :class:`~factory.Factory` subclasses


.. _changelog-1-3-0-upgrading:

Upgrading
"""""""""

This version deprecates a few magic or undocumented features.
All warnings will turn into errors starting from v2.0.0.

In order to upgrade client code, apply the following rules:

- Add a ``FACTORY_FOR`` attribute pointing to the target class to each
  :class:`~factory.Factory`, instead of relying on automagic associated class
  discovery
- When using factory_boy for Django models, have each factory inherit from
  :class:`~factory.DjangoModelFactory`
- Replace ``factory.CircularSubFactory('some.module', 'Symbol')`` with
  ``factory.SubFactory('some.module.Symbol')``
- Replace ``factory.InfiniteIterator(iterable)`` with ``factory.Iterator(iterable)``
- Replace ``@factory.post_generation()`` with ``@factory.post_generation``
- Replace ``factory.set_building_function(SomeFactory, building_function)`` with
  an override of the :meth:`~factory.Factory._build` method of ``SomeFactory``
- Replace ``factory.set_creation_function(SomeFactory, creation_function)`` with
  an override of the :meth:`~factory.Factory._create` method of ``SomeFactory``



1.2.0 (09/08/2012)
------------------

*New:*

    - Add :class:`~factory.CircularSubFactory` to solve circular dependencies between factories

1.1.5 (09/07/2012)
------------------

*Bugfix:*

    - Fix :class:`~factory.PostGenerationDeclaration` and derived classes.

1.1.4 (19/06/2012)
------------------

*New:*

    - Add :meth:`~factory.use_strategy` decorator to override a
      :class:`~factory.Factory`'s default strategy
    - Improve test running (tox, python2.6/2.7)
    - Introduce :class:`~factory.PostGeneration` and
      :class:`~factory.RelatedFactory`

1.1.3 (9/03/2012)
-----------------

*Bugfix:*

  - Fix packaging rules

1.1.2 (25/02/2012)
------------------

*New:*

  - Add :class:`~factory.Iterator` and :class:`~factory.InfiniteIterator` for :class:`~factory.Factory` attribute declarations.
  - Provide :func:`~factory.Factory.generate` and :func:`~factory.Factory.simple_generate`, that allow specifying the instantiation strategy directly.
    Also provides :func:`~factory.Factory.generate_batch` and :func:`~factory.Factory.simple_generate_batch`.

1.1.1 (24/02/2012)
------------------

*New:*

  - Add :func:`~factory.Factory.build_batch`, :func:`~factory.Factory.create_batch` and :func:`~factory.Factory.stub_batch`, to instantiate factories in batch

1.1.0 (24/02/2012)
------------------

*New:*

  - Improve the :class:`~factory.SelfAttribute` syntax to fetch sub-attributes using the ``foo.bar`` syntax;
  - Add :class:`~factory.ContainerAttribute` to fetch attributes from the container of a :class:`~factory.SubFactory`.
  - Provide the :func:`~factory.make_factory` helper: ``MyClassFactory = make_factory(MyClass, x=3, y=4)``
  - Add :func:`~factory.build`, :func:`~factory.create`, :func:`~factory.stub` helpers

*Bugfix:*

  - Allow classmethod/staticmethod on factories

*Deprecation:*

  - Auto-discovery of :attr:`~factory.Factory.FACTORY_FOR` based on class name is now deprecated

1.0.4 (21/12/2011)
------------------

*New:*

  - Improve the algorithm for populating a :class:`~factory.Factory` attributes dict
  - Add ``python setup.py test`` command to run the test suite
  - Allow custom build functions
  - Introduce :data:`~factory.MOGO_BUILD` build function
  - Add support for inheriting from multiple :class:`~factory.Factory`
  - Base :class:`~factory.Factory` classes can now be declared :attr:`abstract <factory.Factory.ABSTRACT_FACTORY>`.
  - Provide :class:`~factory.DjangoModelFactory`, whose :class:`~factory.Sequence` counter starts at the next free database id
  - Introduce :class:`~factory.SelfAttribute`, a shortcut for ``factory.LazyAttribute(lambda o: o.foo.bar.baz``.

*Bugfix:*

  - Handle nested :class:`~factory.SubFactory`
  - Share sequence counter between parent and subclasses
  - Fix :class:`~factory.SubFactory` / :class:`~factory.Sequence` interferences

1.0.2 (16/05/2011)
------------------

*New:*

  - Introduce :class:`~factory.SubFactory`

1.0.1 (13/05/2011)
------------------

*New:*

  - Allow :class:`~factory.Factory` inheritance
  - Improve handling of custom build/create functions

*Bugfix:*

  - Fix concurrency between :class:`~factory.LazyAttribute` and :class:`~factory.Sequence`

1.0.0 (22/08/2010)
------------------

*New:*

  - First version of factory_boy


Credits
-------

* Initial version by Mark Sandstrom (2010)
* Developed by Raphaël Barrois since 2011


.. vim:et:ts=4:sw=4:tw=119:ft=rst:
