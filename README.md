Status
======

This project is no longer maintained. Feel free to clone it and fix for new postgresql versions. You can let me know (email address below) if you have it working, I'll put link here to your repository.

www_fdw
=======

This library contains a PostgreSQL extension,
a Foreign Data Wrapper (FDW) handler of PostgreSQL
which provides easy way for interacting with different web-services.

Installation
============

    $ export USE_PGXS=1
    $ make && make install
    $ psql -c "CREATE EXTENSION www_fdw" db

After that you need to create server for extension.
The simpliest example here is:

    $ cat test/default-json.sql
    DROP EXTENSION IF EXISTS www_fdw CASCADE;
    CREATE EXTENSION www_fdw;
    CREATE SERVER www_fdw_server_test FOREIGN DATA WRAPPER www_fdw OPTIONS (uri 'http://localhost:7777');
    CREATE USER MAPPING FOR current_user SERVER www_fdw_server_test;
    CREATE FOREIGN TABLE www_fdw_test (
        title text,
        link text,
        snippet text
    ) SERVER www_fdw_server_test;


For more examples check [github wiki](https://github.com/cyga/www_fdw/wiki/Examples).

postgresql versions
-------------------

* 9.5 - has to be supported by the master branch (not tested);
* 9.2 - supported by the master branch;
* 9.1 - use git tag "9.1" to retrieve working version. For proper usage of deserialize callbacks you need to use the patch [patches/foreign-table-as-argument-to-plpgsql_parse_cwordtype.patch](https://github.com/cyga/www_fdw/tree/master/patches/foreign-table-as-argument-to-plpgsql_parse_cwordtype.patch).

XML
---

For XML support you need to install postgresql server with --with-libxml configure option and (check libxml dependency below).

JSON
----

For json support in your callbacks you need to install [json type for postgresql](http://git.postgresql.org/gitweb/?p=json-datatype.git;a=summary).

This type is used only for custom callback functions only. Even though, you can parse text representations of json in this case.

Current implementation of postgresql json native type doesn't allow to retrieve fields, thus can't be used in current state.

Documentation
=============

Up-to-date documentation can be found at [github](https://github.com/cyga/www_fdw/wiki/Documentation).

PostgreSQL server installation
==============================

If your response isn't of xml type or you don't plan to use xml parsing in `response_deserialize_callback` - don't bother about it.
Otherwise, in order to work with xml type (used in `response_deserialize_callback`) your installation has to support xml type. Usually it means building PostgreSQL with `--with-libxml` option.
If you plan to use `response_deserialize_callback` for xml but with own parsing mechanism - your callback will be passed with text parameter.

Depencencies
============

This module depends on

  * [libcurl](http://curl.haxx.se/libcurl/)
  * [libjson](http://projects.snarc.org/libjson/)
  * [libxml](http://en.wikipedia.org/wiki/Libxml2)

The source of libjson is included in this module package and linked as a
static library, whereas libcurl is assumed to be installed in the system.
You may need additional development packages, as libcurl-dev.
Consult your system and repository owner for more details.

Development
===========

* [source](http://github.com/cyga/www_fdw);
* [documentation](https://github.com/cyga/www_fdw/wiki/Documentation);
* [test/README.md](https://github.com/cyga/www_fdw/blob/master/test/README.md) - documentation on test system.

Author(s)
=========

Alex Sudakov <cygakob@gmail.com>

Copyright and License
=====================

This module is free software; you can redistribute it and/or modify it under
the [PostgreSQL License](http://www.opensource.org/licenses/postgresql).

Permission to use, copy, modify, and distribute this software and its
documentation for any purpose, without fee, and without a written agreement is
hereby granted, provided that the above copyright notice and this paragraph
and the following two paragraphs appear in all copies.

In no event shall author(s) be liable to any party for direct,
indirect, special, incidental, or consequential damages, including
lost profits, arising out of the use of this software and its documentation,
even if author(s) has been advised of the possibility of such damage.

Author(s) specifically disclaims any warranties,
including, but not limited to, the implied warranties of merchantability and
fitness for a particular purpose. The software provided hereunder is on an "as is" basis, and author(s) has no obligations to provide maintenance,
support, updates, enhancements, or modifications.
