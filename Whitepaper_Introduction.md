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
language, this is indicated using the <b>lang</b> attribute of the <b>task</b> element. In case of
multiple languages, the <b>lang</b> attribute of the <b>task</b> element contains the main
language and translations can be indicated by using markup with <b>@@@</b> and a file <b>strings.txt</b> with translations that is stored in "lang/en/strings.txt", "lang/de/strings.txt", "lang/en_us/strings.txt", etc. 
For example, multiple languages for a title are indicated by
```xml
   <title>@@@title2@@@</title>
```
with a corresponding line in strings.txt. Mixing markup and non-markup is not allowed:
```xml
   <title>@@@title3@@@ is not allowed!!</title>
```
This is relevant for any elements with these names: <b>description</b>, <b>comment</b>, <b>title</b>, <b>content</b> and <b>displaytitle</b>. In the case of files, copies in different languages can be supplied. Such files cannot be embedded in the XML, but their
filename is marked up:
```xml
   <file id="f1" class="internal" type="file">@@@pathtofile1@@@</file>
```
The submission part does not contain any markup. The attribute <b>lang</b> in the <b>result-spec</b> of the submission part indicates a preferred language for the response.
