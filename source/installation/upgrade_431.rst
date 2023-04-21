##############################
Upgrading from 4.3.0 to 4.3.1
##############################

설치 방법에 해당하는 업그레이드 지침을 참조하십시오.

- :ref:`Composer 설치 - App Starter 업그레이드 <app-starter-upgrading>`
- :ref:`Composer 설치 - 기존 프로젝트의 CodeIgniter4 업그레이드 <adding-codeigniter4-upgrading>`
- :ref:`수동 설치 업그레이드 <installing-manual-upgrading>`

.. contents::
    :local:
    :depth: 2

Composer Version
****************

.. important:: If you use Composer, CodeIgniter v4.3 requires
    Composer 2.0.14 or later.

If you are using older version of Composer, upgrade your ``composer`` tool,
and delete the **vendor/** directory, and run ``composer update`` again.

The procedure, for example, is as follows::

    > composer self-update
    > rm -rf vendor/
    > composer update

Mandatory File Changes
**********************

Config Files
============

app/Config/Email.php
--------------------

- If you updated **app/Config/Email.php** when upgrading to v4.3.0, you must
  set the default values to ``$fromEmail``, ``$fromName``,  ``$recipients``,
  ``$SMTPHost``, ``$SMTPUser`` and ``$SMTPPass`` to apply environment variable
  (**.env**) values.
- If no default values are set, setting environment variables for them will not
  be reflected in the Config object.

app/Config/Exceptions.php
-------------------------

- If you are using PHP 8.2, you need to add new properties ``$logDeprecations`` and ``$deprecationLogLevel``.

Project Files
*************

Some files in the **project space** (root, app, public, writable) received updates. Due to
these files being outside of the **system** scope they will not be changed without your intervention.

There are some third-party CodeIgniter modules available to assist with merging changes to
the project space: `Explore on Packagist <https://packagist.org/explore/?query=codeigniter4%20updates>`_.

Content Changes
===============

The following files received significant changes (including deprecations or visual adjustments)
and it is recommended that you merge the updated versions with your application:

Config
------

- app/Config/Email.php
    - Set the default value ``''`` to ``$fromEmail``, ``$fromName``,
      ``$recipients``, ``$SMTPHost``, ``$SMTPUser`` and ``$SMTPPass``
      to apply environment variable (**.env**) values.

All Changes
===========

This is a list of all files in the **project space** that received changes;
many will be simple comments or formatting that have no effect on the runtime:

-   app/Config/Email.php
-   composer.json
