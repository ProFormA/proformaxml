# An XML exchange format for (programming) tasks
THIS DOCUMENT IS CURRENTLY BEING EDITED AND NOT FINISHED

**Version 2.0**

contributors listed in alphabetical order:

Technische Universität Clausthal: *Oliver Müller*;
Hochschule Hannover: *Robert Garmann, Paul Reiser, Peter Werner*;
Ostfalia Hochschule für Angewandte Wissenschaften: *Karin Borm, Uta Priss, Oliver Rod*

[TOC]

## Introduction

## Internalization

ISO 639-1 and 3166-1 ALPHA2 are used for indicating regions. If a ProFormA task contains only one natural
language, this is indicated using the <b>lang</b> attribute of the <b>task</b> element. In case 
multiple languages, the <b>lang</b> attribute of the <b>task</b> element contains the main
language. Where appropriate translations can be supplied (eg. in descriptions and titles) using
markup with <b>@@@</b> and a file with translations. For example
```xml
   <displaytitle>@@@title2@@@</displaytitle>
```
Mixing markup and non-markup is not allowed:
```xml
   <displaytitle>@@@title3@@@ is not allowed!!</displaytitle>
```
