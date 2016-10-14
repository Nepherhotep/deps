========================
DEP 2: Experimental APIs
========================

:DEP: 193
:Author: Alexey Zankevich
:Implementation Team: Alexey Zankevich
:Shepherd: Anssi Kääriäinen
:Status: Draft
:Type: Feature
:Created: 2016-10-14
:Last-Modified: 2016-10-14

.. contents:: Table of Contents
   :depth: 3
   :local:


Abstract
========

This DEP describes required changes in Django ORM functionality to allow
third party libraries to implement their own lookup syntax instead of default
keyword-based one.

Motivation
==========

Currently, Django ORM uses keyword syntax to perform lookups in database:

.. code:: Python

    # simple example
    User.objects.filter(username='Alexey')

    # more complicated
    User.objects.filter(username__icontains='alex')

    # example of multiple parameters to keyword
    Movie.objects.filter(created__range=(from_date, to_date))

    # example of the lookup, where parameters doesn't look consistent:
    # slice indexes encoded as a part of keyword
    # meanwhile additional parameter passed directly
    Movie.objects.filter(tags__0_1__contains=['thoughts'])


There are some issues, related to this approach:

1. Keyword strings is hard to reuse, thus DRY principle gets violated
2. Hard to pass multiple parameters
3. Keywords syntax doesn't look pythonic (especially, for indexes or slices)
4. Can't chain transforms with parameters (.e.g. COLLATE 'fi')

We can create a public API for third-party developers to implement their own
lookup syntax, which can look like in samples below:

.. code:: Python

    # comparison
    User.objects.filter(E.username == 'Alexey')

    # more complicated lookup
    User.objects.filter(E.username.icontains('alex'))

    # multiple parameters
    Movie.objects.filter(E.created.range(from_date, to_date))

    # slice
    Movie.objects.filter(E.tags[0:1].contains(['thoughts']))

    # chained transforms
    Movie.objects.filter(E.name.collate('fi').icontains('anssi'))


This DEP doesn't cover implementation of such libraries, just the API on Django
side allowing them to be implemented.

Implementation
==============

...

Rationale
=========

...

Copyright
=========

This document has been placed in the public domain per the Creative Commons
CC0 1.0 Universal license (http://creativecommons.org/publicdomain/zero/1.0/deed).
