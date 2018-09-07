# An XML exchange format for (programming) tasks
THIS DOCUMENT IS CURRENTLY BEING EDITED AND NOT FINISHED

**Version 2.0**

contributors listed in alphabetical order:

Technische Universität Clausthal: *Oliver Müller*;
Hochschule Hannover: *Robert Garmann, Peter Fricke, Paul Reiser*;
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

## Internationalization

If a ProFormA task contains only one natural
language, this is indicated by using the <b>lang</b> attribute of the <b>task</b> element. In case of
multiple languages, the <b>lang</b> attribute of the <b>task</b> element contains the main
language and translations can be indicated by using markup with <b>@@@</b> and a file <b>strings.txt</b> with translations that is stored in "lang/en/strings.txt", "lang/de/strings.txt", "lang/en_us/strings.txt", etc. ISO 639-1 and 3166-1 ALPHA2 are used for indicating languages and regions.
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

TODO: strings.txt files:

- what's in it? key/value pairs separated by equals sign? Spaces around the equals sign are neither part of the key nor the value?
- What about multiline values?
- UTF8 encoded?

## Files

### The embedded-file element

```xml
<xs:complexType name="embedded-file-type">
  <xs:simpleContent>
    <xs:extension base="xs:string">
      <xs:attribute name="filename" type="xs:string"
                    use="required"/>
      <xs:attribute name="is-base64-encoded" type="xs:boolean"
                    default="false"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

The embedded-file element is used to embed a file directly into XML. Files that contain binary data must be encoded to Base64 and the **is-base64-encoded** attribute set to true. Plaintext file content must be encoded to UTF-8. Embedded files also require a **filename**.

### The attached-file element

```xml
<xs:simpleType name="attached-file-type">
  <xs:restriction base="xs:string"/>
</xs:simpleType>
```

The attached-file element is used to attach arbitrary files to ZIP archives. This is especially useful when we are dealing with files that we do not want to embed in XML for various reasons (e. g. the files in question are particularly large in size). The element content is the relative path to the file within the ZIP file.

Note that while it is possible to use the attached-file element for any kind of file, the [attached-text-file](#the-attached-text-file-element) element should be the preferred way to attach plaintext files to ZIP archives.

### The attached-text-file element

```xml
<xs:complexType name="attached-text-file-type">
  <xs:simpleContent>
    <xs:extension base="tns:attached-file-type">
      <xs:attribute name="encoding" type="xs:string"/>
      <xs:attribute name="natural-language" type="xs:string"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

The attached-text-file element is used to exclusively attach plaintext files to a ZIP archive. It comes with a few optional attributes that are particularly useful when dealing with plaintext.

- **encoding** 

    The encoding of the text file, as an optional attribute.

- **natural-language** 

    The natural-language attribute specifies the natural language of the submitting student. Students tend to use all kinds of encodings in their text files. Most of the time, the encoding will be unknown at the time of submission. To address this problem, the natural-language attribute can be used to help the grader detect the encoding of a submitted plaintext file.
    
    It should be said that the natural-language attribute does not necessarily have to be the same as the one provided in the task's [lang](Whitepaper_Task.md#task-attributes) attribute. While the lang attribute indicates the language that the task has been written in, a student might use a different language when writing their text entirely.
    
    Providing a value for the natural-language attribute could be as simple as retrieving a preconfigured value, like the language the student configured in their user profile of the LMS.

    In the case that encoding and natural-language should happen to contradict each other, encoding is given precedence.

    The natural-language value should be formatted as ISO 639-1 for language codes, and ISO 3166-1 alpha-2 for country codes.

### The feedback-level

```xml
<xs:simpleType name="feedback-level-type">
  <xs:restriction base="xs:string">
    <xs:enumeration value="debug"/>
    <xs:enumeration value="info"/>
    <xs:enumeration value="warn"/>
    <xs:enumeration value="error"/>
  </xs:restriction>
</xs:simpleType>
```

The feedback-level is used for submission and response documents. Usually, feedback can be grouped into different types of information.

- **debug**

    Debug feedback contains information normally only relevant to debugging a grading system.
        
- **info**

    Generally useful information to students and teachers, e. g. a specific test case passed successfully.
    
- **warn**

    The feedback contains a warning, e. g. the function being tested took up too much processing time. Or the student used a deprecated class method.
    
- **error**

    The feedback contains error information, e. g. the student's source code resulted in a compile-time error that caused the entire test case to fail. This error type should not be confused with the response's [is-internal-error](Whitepaper_Response.md#is-internal-error) flag, which indicates that an error occurred on the part of the grading system.

## Grading Hints

TODO: should be explained here because it relates to several parts
