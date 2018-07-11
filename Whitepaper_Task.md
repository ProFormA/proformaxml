# An XML exchange format for (programming) tasks
THIS DOCUMENT IS CURRENTLY BEING EDITED AND NOT FINISHED

**Version 2.0**

contributors listed in alphabetical order:

Technische Universität Clausthal: *Oliver Müller*
Hochschule Hannover: *Robert Garmann, Paul Reiser, Peter Werner*
Ostfalia Hochschule für Angewandte Wissenschaften: *Karin Borm, Uta Priss, Oliver Rod*

Based on an earlier version with more contributors:
https://github.com/ProFormA/taskxml/blob/master/whitepaper.md

[TOC]

## Introduction

This document specifies syntax and semantics for the part about tasks in a standardized “ProFormA-XML
Format” - an exchange format for programming exercises/tasks. It
formalises the specification of programming exercises, for example, the
description, required programming language and suggested tests for
evaluating the student submitted code so that exercises written for one
tool can be exported and imported into another tool. 

## General Structure of the Task Part

The task part consists of two parts: The first section shows
the description/specification of a task, including supporting files; and
the second section demonstrates a specification of tests which are to be
included in the specification of a task under the \<tests\> tag. Each
task can have many tests.

### XML Specification

The general structure of the task part is given as follows (this is
meant to provide an overview and does not represent a minimal document):
```xml
    <tns:task>
        <tns:description></tns:description>
        <tns:proglang version=""></tns:proglang>
        <tns:submission-restrictions />
        <tns:files />
        <tns:external-resources />
        <tns:model-solutions />
        <tns:tests />
        <tns:grading-hints />
        <tns:meta-data />
    </tns:task>
```

### Task attributes

The task is identified by attribute <b>uuid</b>, an automatic generated UUID 
in Version 4 (see RFC 4122). There is no need for monitoring the uniqueness,
 the chance of generating two UUIDs having the same value is about 6 x 10^-11.

The optional attribute <b>parent-uuid</b> should be used whenever a task is changed. It is
a pointer to the original task-uuid. This is useful for version trees.

The task itself must have an attribute <b>lang</b> which specifies the natural language
used. The description, title etc should be written in this language. The content
of the “lang” attribute must comply with the IETF BCP 47, RFC 4647 and
ISO 639-1:2002 standards.

### The description part

The description element contains the task description as text. A
subset of HTML is allowed (see Appendix A).

### The proglang part

The progrlang element contains the programming/modelling/query
language to which this task applies. A valid list of values is specified
in Appendix B. The <b>version</b> attribute specifies which version of the
language was used in the creation of the task. (The task is guaranteed
to work with that version – any other requirements about version
compatibility must be checked externally.)  The “version” must be
entered as a “point” separated list of up to four unsigned integers.

### The submission-restrictions part

The submission-restrictions element can specify restrictions for the upload of
(submission) files - by default there are no restrictions. There is a choice
between three possible restrictions types. 
- [archive-restriction](#archive-restriction) 
- [file-restriction](#file-restriction) 
- [regexp-restriction](#regexp-restriction)

All restriction types have two optional attributes

-  <b>max-size</b> specifies the maximum size of a file in bytes which should be
   accepted. Systems which have a stronger limit of the file size should print a
   warning to the importing user. If this attribute is missing, a system default
   value will be used.
-  <b>mimetype-regexp</b> specifies the mimetype by regular expression of files the
   system should accept (specified regexp language in [regexp-language-restriction]
     (#regexp-language-specification))

#### Archive restriction

 - <b>unpack-files-from-archive</b> specifies if uploaded archives (zip/jar) should
   be unpacked automatically. If it is set to *false,* no extraction takes place
   and the archive is used as it is.
 - the filename of the uploaded archive must match "allowed-archive-filename"
 
There is a choice for handling the file restrictions.

1. by regexp: The archive may contain many files. The regular expression specifies, which files will be extracted from the archive

```xml
   <tns:submission-restrictions>
        <tns:archive-restrictions max-size="[size in bytes]" mime-type-regexp="[mimetype regexp]" unpack-files-from-archive="[true/false]">
              <tns:unpack-files-from-archive-regexp>regular expression</tns:unpack-files-from-archive-regexp>
        </tns:archive-restrictions>
    </tns:submission-restrictions>
``` 

 -   “unpack-files-from-archive-regexp” holds a regular expression that
     controls which files are automatically extracted. Only matching files (the
     whole path of the zip-items matches with “/” as path separator) are
     extracted from the archive (specified regexp language in [regexp-language-restriction]
     (#regexp-language-specification)).
 
2. by filenames: The archive must or may contain files as specified by the following file restrictions

```xml
   <tns:submission-restrictions>
        <tns:archive-restrictions max-size="[size in bytes]" mime-type-regexp="[mimetype regexp]" unpack-files-from-archive="[true/false]">
            <tns:file-restrictions>
                <tns:required path="[full path to file]" mime-type-regexp="[mimetype regexp]" />
                <tns:optional path="[full path to file]" mime-type-regexp="[mimetype regexp]"/>
            </tns:file-restrictions>
        </tns:archive-restrictions>
    </tns:submission-restrictions>
```

- <b>required</b> the archive must contain a file with specified "path" (rooted at the archive root) and optional "mime-type-regexp". Otherwise the submission should be rejected.
- <b>optional</b> the archive may contain a file with specified attributes

#### File restriction

A submission must or may consist of files as specified by the file restrictions. A submission should be rejected, if it does not match the restrictions.

```xml
   <tns:submission-restrictions>
            <tns:file-restrictions>
                <tns:required path="[full path to file]" mime-type-regexp="[mimetype regexp]" max-size="[size in bytes]"/>
                <tns:optional path="[full path to file]" mime-type-regexp="[mimetype regexp]" max-size="[size in bytes]"/>
            </tns:file-restrictions>
    </tns:submission-restrictions>
```

- <b>required</b> the submission must have a file with specified "path" (rooted at the archive root). Otherwise the submission should be rejected.
- <b>optional</b> the submission may have a file with specified attributes.

#### Regexp restriction

A submission must consist of one or several files, where all file names must adhere to the regular expression.

```xml
   <tns:submission-restrictions>
            <tns:regexp-restriction mime-type-regexp="[mimetype regexp]" max-size="[size in bytes]">regular expression</tns:regexp-restriction>
    </tns:submission-restrictions>
```

- <b>regexp-restriction</b> holds a regular expression of the filenames (only the filename, without path) which the system should accept. Regular expressions can contain less than/greater than signs. CDATA is allowed.

#### Regexp language specification

TODO!!!

### The files part

The files element contains 0 or more file elements. A file element is
used to attach files to a task. Files can be external or embedded into
the XML file.

### The file element

The file element includes or links a single file to a task. Each
instance/file must have a (task) unique string in its <b>id</b> attribute (in
order to reference this file within this task) and has to be classified
using the <b>class</b> attribute with one of the following values:

-   <b>template</b>: The file is a template for students to be used as a
    starting point for their solution.
-   <b>library</b>: The file is a library to be used by students (and might
    also be required for tests).
-   <b>inputdata</b>: The file contains data which the algorithm of the
    students should work with.
-   <b>instruction</b>: The file contains further instructions for handling
    the task, e.g. an UML activity diagram.
-   <b>internal-library</b>: This file is not visible for students and holds
    libraries which are required for processing the tests within the system.
-   <b>internal</b>: This file is not visible for students and holds files
    which are required for processing the task/tests within the system.

The information about how a file is used in a test is supplied by the
test-configuration which uses a file (via its ID). Further details can
be provided in the optional <b>comment</b> attribute. The file itself can be
embedded into the XML (recommended for shorter plain text files) or can
be included in the ZIP-archive (recommended for binary files). If a file
is embedded, the <b>type</b> attribute must be set to “embedded” and the text
content of the element is the file content. The <b>filename</b> attribute can
be set to define a filename (e.g., for Java classes where the class
name must be equal to the filename, it can also include a relative
path). This attribute defines the name the file will have when it is
executed. If the name is unimportant, a default name can be used. If a
file is not embedded, the <b>type</b> attribute must be set to “file” and the
text content of the element contain the filename within the task ZIP
archive (which can be different from the filename attribute).

### The external-resources part

The external-resources element contains 0 or more external-resource elements. An external-resource element is
used to refer to a resource that is neither embedded nor directly attached to the task.

### The external-resource element

Normally task files should be self-contained, but in rare cases the use of external resources is unavoidable for fulfilling or grading the task.  The external-resource element basically contains a reference to that kind of resources. In its simplest form, the resource is identified by an identifier contained in the <b>reference</b> attribute. More complicated references can be specified in child elements of any namespace.

The idea behind external resources is that sometimes large files needed by a grader, files that change frequently or web services cannot be bundled reasonably with the task itself. In these cases, the task part may reference the external resource by a unique name or any other identifier. Examples are the name or URL of a widely known database dump (e. g. ftp://ftp.fu-berlin.de/pub/misc/movies/database/) or the name and version number of a library (e. g. urn:mvn:groupId=org.mockito:artifactId=mockito-core:packaging=jar:version=1.9.5) or a web service URL. The semantics and the format of references is not defined by this exchange format. It could be a URL, URN or any other identifier. Also the exchange format does not define the distribution mechanism of resources (e. g. push from LMS to grader, active pull by the grader, etc.). Taken to the extreme, an external-resource element may mean, that the administrator has to install some software, service, or data in a grader specific location, before the task can be used to grade submissions. The identifier therefore does not need to be in a machine readable format. Further details about the semantics of the external-resource can be provided in the optional <b>description</b> element. The description may provide instructions in natural language how to install the required resource prior to grading so that the grader can interpret and resolve the reference attribute successfully when grading a submission.

Each external resource element can be identified by its mandatory <b>id</b> attribute and is referenced by the test-configuration (see below in the test section).

### The model-solutions part

The model solutions element is used to provide one or more solutions of
the task. For each model-solution a new <b>model-solution</b> element is added.

### The model-solution element

The model-solution element links one single model-solution to a task. Each
solution must have a (task) unique string in its <b>id</b> attribute.
The model-solution must refer to one or more files using the filerefs/fileref tag.
The optional attribute <b>comment</b> can be used for additional
information, for example if more than one model solution is provided it
can be explained why there are several solutions.

### The tests part

The tests element is used to provide automatic checks and tests for the
task. More specific information about the test XML is provided in the
[second section](#test-section) of this paper.

### The grading-hints element

TODO

### The meta-data element

The meta-data element holds a namespace for the meta-data of each
system. Because meta-data are already standardized in other systems, it
was decided not to formalize them as part of this specification. Only
meta-data relevant for the whole task should be entered here. Meta-data
that is specific to an individual test should be entered in the
test-meta-data element.

## Test section

### XML Specification

The general structure of the test part is given as follows:

```xml
    <tns:tests>
        <tns:test id="unique ID" validity="">
            <tns:title></tns:title>
            <tns:test-type></tns:test-type>
            <tns:test-configuration>
                <tns:filerefs>
                    <tns:fileref></tns:fileref>
                </tns:filerefs>
                <tns:externalresourcerefs>
                    <tns:externalresourceref></tns:externalresourceref>
                </tns:externalresourcerefs>
                <tns:test-meta-data />
            </tns:test-configuration>
        </tns:test>
    </tns:tests>
```
### The test element

The test element has a required attribute <b>id</b> and an optional attribute
<b>validity</b>. The optional attribute “validity” is used by some systems
(such as Vips) for tests which only partially verify the solution code.

### The title element

The title element is used to provide a short and clear name for the test
that can be displayed to students as part of their results. It should be
noted that the title does not have a language attribute because it is
assumed that the title is written in the same natural language as
specified for the task itself.

### The test-type element

Examples of values are: java-syntax, unittest. A list of allowed
entries is specified in Appendix C.

### The test-configuration part

The test-configuration contains all parameters which are needed for
configuring this specific test. Each test can
also have elements of its own namespace for test-type specific
configuration options.

### The filerefs part

Several filerefs can be specified via fileref elements.

### The fileref element

The fileref element links a single file to a test based on the ID of the
file which has to be defined in task/files. The ID has to be entered as
the refid attribute.

### The externalresourcerefs part

Several externalresourcerefs can be specified via externalresourceref elements.

### The externalresourceref element

The externalresourceref element links a single external-resource to a test based on the ID of the
external-resource which has to be defined in task/external-resources. The ID has to be entered as
the refid attribute.

### The test-meta-data element

The test-meta-data element holds a namespace for test-specific meta-data
of each system. This is particularly useful for attributes that are
required for ex- and import in one system but which are not relevant for
other systems.

## Appendix A: Subset of HTML

<\!\-\- … \-\-\>, a, b, blockquote, br, p, sup, sub, center, div, dl, dd,
dt, em, font, h1, h2, h3, h4, h5, h6, hr, img, li, ol, strong, pre,
span, table, tbody, td, tr, th, tt and ul.

However, for div, b, br, dd, dt, dl, blockquote, p, sup, sub, h1, ...h6,
li, ol, hr, strong, pre, span, table, tbody, td, tr, th, tt and ul there
are no attributes allowed in order to avoid problems with different
layouts.

## Appendix B: List of programming languages

-   java
-   SQL
-   prolog

## Appendix C: List of test types

-   java-compilation
-   java-checkstyle
-   java-code-coverage-emma
-   java-findbugs
-   java-pmd
-   unittest (urn:proforma:tests:unittest:v1)
-   dejagnu
-   anonymity (heuristics for checking that students have not included
    their names in the code)

