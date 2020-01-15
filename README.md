---
section: 32
x-masysma-name: masysmaci/build
title: Ant Build Template
date: 2019/12/06 13:16:32
lang: en-US
author: ["Linux-Fan, Ma_Sys.ma (Ma_Sys.ma@web.de)"]
keywords: ["ant", "make", "build", "masysmaci", "template", "ant_build_template", "ant-build-template.xml"]
x-masysma-version: 1.0.0
x-masysma-repository: https://www.github.com/m7a/co-build
x-masysma-owned: 1
x-masysma-copyright: |
  Copyright (c) 2019 Ma_Sys.ma.
  For further info send an e-mail to Ma_Sys.ma@web.de.
---
WARNING: EXPERIMENTAL CODE
==========================

**THIS IS HIGHLY EXPERIMENTAL AND UNDER DEVELOPMENT**

Overview
========

The Ma_Sys.ma _Ant Build Template_ is an attempt to provide a single unifying
`build.xml` for the Ant build system which allows compiling most Ma_Sys.ma
Programs with only minor modifications and which provides extended functionality
to generate Debian Packages. This replaces large parts of the formerly used
MDPC scripts and the previously used Makefile templates.

In addition to being a template, this is designed to facilitate _code reuse_ by
not being copied upon use, but rather _imported_. E.g. this XML can be loaded
directly from its repository or alternatively from the file system. See section
_Using the Template_ for details.

The template is designed to allow building programs in the following programming
languages:

 * Ada
 * C
 * Java

Additionally, it can compile _LaTeX_ source code to PDF and act as a top-level
template for recursive invocation.

How to compile Projects using this Template
===========================================

Similar to Makefiles, `ant` provides a set of targets.
The following targets/functionalities are generally available.

`build`
:   This target compiles the project.
`jar`
:   If it is a Java project, `jar` builds a Jarfile if possible.
`clean`
:   Deletes object files i.e. all generated except for the linked build result.
`dist-clean`
:   Deletes all generated files including the linked build result.

If a project is intended to be built in form of a Debian package, these
additional target can be invoked:

`package`
:   Build a Debian package for this project.

For development purposes, the following targets allow managing the state of
the package.

`init`
:   Initializes a changelog.
`incver`
:   Interactively invoke the VIM editor to add a changelog entry
    (for an incremented version).

Using the Template
==================

Two ways of including this template are recommended.

The include is a little obscure because it implements a routine for searching
the template's XML file and retrieving it from a local filesystem if possible.
The logic tries two distinct approaches:

 1. _Attempt to load from a local file_. This searches for a copy of the
    repository by the name `co-build` in the directory given by
    environment variable `MDVL_CI_PHOENIX_ROOT` or `..` (i.e. one level up the
    present working directory) if `MDVL_CI_PHOENIX_ROOT` is not set.
 2. If the file was not found this way, _attempt to load from Internet_. To
    do this, it tries to download the XML from GitHub, i.e. URL
    <https://raw.githubusercontent.com/m7a/co-build/master/ant-build-template.xml>.

## Variant A: Block at the end of a file

This is the preferred default variant which consists of a block entitled
`CONSTANT TPL IMPORT` to indicate that what follows is not intended to
normally be modified.

~~~{.xml}
<?xml version="1.0" encoding="UTF-8"?>
<project default="build">

<!-- APPLICATION METADATA -->
<property name="masysma.target"  value="hello"/> 
<!-- ... -->

<!-- CI INTEGRATION -->
<!-- ... -->

<!-- CONSTANT TPL IMPORT -->
<property environment="env"/>
<condition property="masysma.internal.includepath.rel" value="${env.MDVL_CI_PHOENIX_ROOT}" else=".."><isset property="env.MDVL_CI_PHOENIX_ROOT"/></condition>
<property name="masysma.internal.includepath" location="${masysma.internal.includepath.rel}"/>
<property name="masysma.internal.loadpath" value="${masysma.internal.includepath}/co-build/ant-build-template.xml"/>
<condition property="masysma.internal.load" value="file://${masysma.internal.loadpath}" else="https://raw.githubusercontent.com/m7a/co-build/master/ant-build-template.xml"><resourceexists><file file="${masysma.internal.loadpath}"/></resourceexists></condition>
<import><url url="${masysma.internal.load}"/></import>

</project>
~~~

## Variant B: Block split into two parts

This variant is useful if access to `MDVL_CI_PHOENIX_ROOT` is also needed by
parts of the build targets. They can read its absolute location from property
`masysma.internal.includepath`.

~~~{.xml}
<?xml version="1.0" encoding="UTF-8"?>
<project default="build">

<!-- (metadata etc) -->

<!-- PART OF TPL HERE FOR USE IN BUILD -->
<property environment="env"/>
<condition property="masysma.internal.includepath.rel" value="${env.MDVL_CI_PHOENIX_ROOT}" else=".."><isset property="env.MDVL_CI_PHOENIX_ROOT"/></condition>
<property name="masysma.internal.includepath" location="${masysma.internal.includepath.rel}"/>

<!-- (customized build instructions -->

<!-- TPL IMPORT (PARTIALLY GIVEN ABOVE) -->
<property name="masysma.internal.loadpath" value="${masysma.internal.includepath}/co-build/ant-build-template.xml"/>
<condition property="masysma.internal.load" value="file://${masysma.internal.loadpath}" else="https://raw.githubusercontent.com/m7a/co-build/master/ant-build-template.xml"><resourceexists><file file="${masysma.internal.loadpath}"/></resourceexists></condition>
<import><url url="${masysma.internal.load}"/></import>

</project>
~~~

Use in Projects with a single Programming Language
==================================================

_TODO NO CONTENT ON THIS YET_

## C

	masysma.c.standard
	masysma.c.link.1

Use in Projects with multiple Programming Languages
===================================================

_TODO NO CONTENT ON THIS YET_

MDPC 2.0
========

_TODO NO CONTENT ON THIS YET_
