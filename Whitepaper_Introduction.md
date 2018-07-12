# An XML exchange format for (programming) tasks
THIS DOCUMENT IS CURRENTLY BEING EDITED AND NOT FINISHED

**Version 2.0**

contributors listed in alphabetical order:

Technische Universität Clausthal: *Oliver Müller*;
Hochschule Hannover: *Robert Garmann, Paul Reiser, Peter Werner*;
Ostfalia Hochschule für Angewandte Wissenschaften: *Karin Borm, Uta Priss, Oliver Rod*

Based on an earlier version with more contributors:
https://github.com/ProFormA/taskxml/blob/master/whitepaper.md


## Introduction

This document specifies syntax and semantics for a standardized “ProFormA-XML
Format” - an exchange format for programming exercises/tasks. It
formalises the specification of programming exercises, for example, the
description, required programming language and suggested tests for
evaluating the student submitted code so that exercises written for one
tool can be exported and imported into another tool.

The format contains three main parts: <b>task</b>, <b>submission</b> and <b>response</b> 
which are each described in a separate document.

## Internalization

ISO 639-1 and 3166-1 ALPHA2 are used for indicating regions. If a ProFormA task contains only one natural
language, this is indicated using the <b>lang</b> attribute of the <b>task</b> element. In case 
multiple languages, the <b>lang</b> attribute of the <b>task</b> element contains the main
language. Where appropriate translations can be supplied using markup with <b>@@@</b> and a file with translations. For example
```xml
   <displaytitle>@@@title2@@@</displaytitle>
```
Mixing markup and non-markup is not allowed:
```xml
   <displaytitle>@@@title3@@@ is not allowed!!</displaytitle>
```
This is relevant for any elements with these names: <b>description</b>, <b>comment</b>, <b>title</b>, <b>content</b> and <b>displaytitle</b>. In the case of files, copies in different languages can be supplied. Such files are not embedded in the XML, but their
filename ist marked up:
```xml
   <file id="f1" class="internal" type="file">@@@pathtofile1@@@</file>
```
