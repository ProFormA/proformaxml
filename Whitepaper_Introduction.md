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

(Because of the length of this whitepaper, it can be helpful to use a userscript that collapses markdown.)

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

## Elements and Types that are used in many parts of the XSD

### Files

All three main parts of the ProFormA format make use of files in various ways. The following file elements present different ways to attach binary and plaintext files to a task, a submission, and a response document.

#### The embedded-bin-file element

The embedded-bin-file element is used to embed a file containing binary content directly into XML. The file content must be encoded to Base64. embedded-bin-file requires a **filename**.

###### Code-Beispiel
```xml
<xs:complexType name="embedded-bin-file-type">
  <xs:simpleContent>
    <xs:extension base="xs:base64Binary">
      <xs:attribute name="filename" type="xs:string" use="required"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

#### The embedded-txt-file element

The embedded-txt-file element is used to embed a file containing plaintext content directly into XML. The file content must be encoded to UTF-8, which is the same encoding that the XML document is encoded in. embedded-txt-file requires a **filename**.

###### Code-Beispiel
```xml
<xs:complexType name="embedded-txt-file-type">
  <xs:simpleContent>
    <xs:extension base="xs:string">
      <xs:attribute name="filename" type="xs:string" use="required"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

#### The attached-bin-file element

The attached-bin-file element is used to attach files containing binary content to ZIP archives. This is especially useful when we are dealing with files that we do not want to embed in XML for various reasons (e. g. the files in question are particularly large in size).

The relative path to the binary file within the ZIP archive is specified in the element content (i. e. the element's text node).

###### Code-Beispiel
```xml
<xs:simpleType name="attached-bin-file-type">
  <xs:restriction base="xs:string"/>
</xs:simpleType>
```

#### The attached-text-file element

The attached-txt-file element is used to attach files containing plaintext content to ZIP archives. It comes with a few optional attributes that are particularly useful when dealing with plaintext.

- **encoding**

    The encoding of the text file, as an optional attribute.

- **natural-language** 

    The natural-language attribute specifies the natural language of the submitting student. Students tend to use all kinds of encodings in their text files. Most of the time, the encoding will be unknown at the time of submission. To address this problem, the natural-language attribute can be used to help the grader detect the encoding of a submitted plaintext file.
    
    It should be said that the natural-language attribute does not necessarily have to be the same as the one provided in the task's [lang](Whitepaper_Task.md#task-attributes) attribute. While the lang attribute indicates the language that the task has been written in, a student might use a different language when writing their text entirely.
    
    Providing a value for the natural-language attribute could be as simple as retrieving a preconfigured value, like the language the student configured in their user profile of the LMS.

    In case encoding and natural-language should happen to contradict each other, encoding is given precedence.

    The natural-language value should be formatted as ISO 639-1 for language codes, and ISO 3166-1 alpha-2 for country codes.

The relative path to the plaintext file within the ZIP archive is specified in the element content (i. e. the element's text node).

###### Code-Beispiel
```xml
<xs:complexType name="attached-txt-file-type">
  <xs:simpleContent>
    <xs:extension base="xs:string">
      <xs:attribute name="encoding" type="xs:string"/>
      <xs:attribute name="natural-language" type="xs:string"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

### The feedback-level

The feedback-level is used for submission and response documents. Usually, feedback can be grouped into different types of information.

- **debug**

    Debug feedback contains information normally only relevant to debugging a grading system.
        
- **info**

    Generally useful information to students and teachers, e. g. a specific test case passed successfully.
    
- **warn**

    The feedback contains a warning, e. g. the function being tested took up too much processing time. Or the student used a deprecated class method.
    
- **error**

    The feedback contains error information, e. g. the student's source code resulted in a compile-time error that caused the entire test case to fail. This error type should not be confused with the response's [is-internal-error](Whitepaper_Response.md#is-internal-error) flag, which indicates that an error occurred on the part of the grading system.

###### Code-Beispiel
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

## Grading Hints

A grading-hints section of a ProFormA task or a ProFormA submission defines, how a grader should calculate a total result from individual test results. Most ProFormA tasks define several tests. Every test is expected to generate a score from the interval [0,1]. The grading-hints element defines groups of tests and groups of groups in a tree like manner. This way the grading-hints element includes the complete hierarchical grading scheme with all tests references, weights, accumulating functions and nullify conditions. Hierarchy nodes and conditions can get a displaytitle and descriptions. All information below the grading-hints element except the root node is optional. 

###### Code-Beispiel
```xml
<xs:complexType name="grading-hints-type">
  <xs:sequence>
    <xs:element name="root" type="tns:grades-node-type"/>
    <xs:element name="combine" type="tns:grades-node-type" minOccurs="0" maxOccurs="unbounded"/>
    <xs:any namespace="##other" minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
  </xs:sequence>
</xs:complexType>
```

### The top elements in the grading hints section

First the grading-hints-type defines the root element. This is the root node of the grading scheme hierarchy. If the root does not specify any child nodes, the total grading score will be obtained by including all test results scores. A "function" attribute (see below) specifies the accumulator function.

combine elements are inner nodes of the grading scheme hierarchy - either as an immediate child of the root node or as a further descendant node. A combine node specifies how to condense several sub results. Sub results can be test results or again "combined" results.

Grader-specific hints from other XML namespaces can be included with the grading-hints element (xs:any). This could be any non-standard information that can be used by a grader or humans to calculate a total result from tests results.

### The elements root and combine

The above root and combine elements both are of the following grades-node-type:

###### Code-Beispiel
```xml
<xs:complexType name="grades-node-type">
  <xs:sequence>
    <xs:element name="displaytitle" type="xs:string" minOccurs="0"/>
    <xs:element name="description" type="tns:description-type" minOccurs="0"/>
    <xs:element name="internal-description" type="tns:description-type" minOccurs="0"/>
    <xs:choice minOccurs="0" maxOccurs="unbounded">
      <xs:element name="test-ref" type="tns:grades-test-ref-child-type" />
      <xs:element name="combine-ref" type="tns:grades-combine-ref-child-type" />
    </xs:choice>
  </xs:sequence>
  <xs:attribute name="id" type="xs:string" use="optional"/>
  <xs:attribute name="function" use="optional" default="min">
    <xs:simpleType>
      <xs:restriction base="xs:string">
        <xs:enumeration value="min"/>
        <xs:enumeration value="max"/>
        <xs:enumeration value="sum"/>
      </xs:restriction>
    </xs:simpleType>
  </xs:attribute>
</xs:complexType>
```

#### The grades-node-type

The grades-node-type represents an inner node of the grading scheme hierarchy. There are only two types of inner nodes: the "root" node and "combine" nodes. Both define a sequence of child references. All child references point to objects representing either a test result or a combined result. The combine node includes these referenced results in a bottom-up fashion to calculate an accumulated combined result. The function attribute defines the accumulator function. 

A test-ref element points to a test element in a ProFormA task. A combine-ref element points to a combine element in the grading scheme hierarchy. Both elements are described in detail below.

The function attribute defines the accumulator function that is used to condense several sub results to a single result. Currently there are three functions to choose from:
   
   * **sum**: Specifies the sum of several sub scores. This is used in a situation, where every child represents a problem aspect that could be solved more or less independently of the other aspects. Weights (see below) can be attached to child node references. Those child nodes representing easy problem aspects could get lower weights than other aspects. If all weights of all direct children of a node add up to 1, this would guarantee, that the parent node result is in [0,1] when all child nodes results are in [0,1].
   * **min**: Specifies the minimum of several sub scores. This can be used in an "all or nothing" situation, where a parent score should reflect the worst of the child results. Weights can be attached to child node references to express the valency of a child's result. The child node representing the easiest aspect among its siblings could get the weight 1. Child nodes for grading aspects connected with a higher effort represent scores that are more difficult to achieve. These child nodes could get weights larger than 1. This would guarantee, that when all child nodes results are in [0,1] also the parent node result is in [0,1].
   * **max**: Specifies the maximum of several sub scores. This is used in an "one success is enough" situation, where a parent score should reflect the best of the child results. An example is a task or a graded problem aspect that could be solved in different ways and for each way there is a separate test element in the task. A solution that succeeds any one of these tests is regarded successful. If one of the different ways of solving the task is more sophisticated than the others, the respective child test could get the highest weight 1. Easier, less valent solution paths get lower weights between 0 and 1. This would guarantee, that when all child nodes results are in [0,1] also the parent node result is in [0,1].

The ``id`` attribute is optional for the root element and required for combine elements.

A valid grading-hints element requires every combine element to be referenced by another combine or root element. All combine elements must have a parent node. Currently the format requires a combine node's parent to be unique. Especially, orphan combine elements are not allowed. The root element is the only element without a parent.

### The elements test-ref and combine-ref

The above combine-ref and test-ref elements both are derived from a common base type, namely grades-base-ref-child-type which is described below. To illustrate these elements, let's think about the following typical example of two combine elements and one root element:

###### Code-Beispiel
```xml
<tns:grading-hints>
  <tns:root function="sum">
    <tns:displaytitle>Total</tns:displaytitle>
    <tns:combine-ref weight="0.75" ref="basic"/>
    <tns:combine-ref weight="0.25" ref="advanced"/>
  </tns:root>
  <tns:combine id="basic" function="sum">
    <tns:displaytitle>Basic functionality</tns:displaytitle>
    <tns:test-ref weight="0.3" ref="test1"/>
    <tns:test-ref weight="0.7" ref="test2"/>
  </tns:combine>
  <tns:combine id="advanced" function="min">
    <tns:displaytitle>Advanced style aspects</tns:displaytitle>
    <tns:test-ref ref="test3"/>
    <tns:test-ref ref="test4"/>
  </tns:combine>
</tns:grading-hints>
```

The example demonstrates that test-ref and combine-ref elements define an optional weight and a referenced identifier. Both elements are specified as follows: 

###### Code-Beispiel
```xml
<xs:complexType name="grades-base-ref-child-type">
  <xs:sequence>
    <xs:choice minOccurs="0">
      <xs:element name="nullify-conditions" type="tns:grades-nullify-conditions-type" />
      <xs:element name="nullify-condition" type="tns:grades-nullify-condition-type" />
    </xs:choice>
  </xs:sequence>
  <xs:attribute name="weight" type="xs:double" use="optional" />
</xs:complexType>
<xs:complexType name="grades-combine-ref-child-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-base-ref-child-type">
      <xs:attribute name="ref" type="xs:string" use="required"/>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="grades-test-ref-child-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-base-ref-child-type">
      <xs:sequence>
        <xs:element name="displaytitle" type="xs:string" minOccurs="0"/>
        <xs:element name="description" type="tns:description-type" minOccurs="0"/>
        <xs:element name="internal-description" type="tns:description-type" minOccurs="0"/>
      </xs:sequence>
      <xs:attribute name="ref" type="xs:string" use="required"/>
      <xs:attribute name="sub-ref" type="xs:string" use="optional"/>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
```
#### Explanations

A combine-ref element's ``ref`` attribute points to the ``id`` attribute of the referenced combine element. The same way a test-ref element's ``ref`` attribute points to the ``id`` attribute of the referenced test element. When the pointed at test exhibits sub test results, the sub-ref attribute points to one of the sub results. Examples are individual test cases in a unit test specification, individual violation rules in a static code sytle analyzer, individual error classes in a compilation step, etc. Since the sub-ref format or content is test-tool-specific, it is not normed in the ProFormA format.

Both, combine-ref and test-ref elements, specifiy a weight that is multiplied to the sub result value of the pointed-at node when flowing into the accumulator function (sum, min, or max). When calculating the accumulated result for the pointing-from node, the score of the pointed-at node is multiplied by the weight. If no weight is present, the score of the pointed-to node is multiplied by 1.

A special feature for the test-ref element are the displaytitle, description and internal-description elements. These override the title or descriptions of the pointed-at test element.  This can be used especially when pointing to sub test results.
   
A last important element of a child reference (test-ref or combine-ref) are so-called nullify conditions, that are described in the following section.

### Nullify conditions

Sometimes a teacher or a task author wants to nullify scores for advanced style aspects if the basic functionality aspects do not exceed a certain threshold. This seems reasonable when we take a closer look at static code analysis tools ("style checker") that often count rule violations. In the above example a student can easily achieve high scores in test3 and test4 when submitting a minimal program that has near to zero functionality. For this, the task author includes a nullify condition at the child reference to the advanced child:

###### Code-Beispiel
```xml
<tns:grading-hints>
  <tns:root function="sum">
    <tns:displaytitle>Total</tns:displaytitle>
    <tns:combine-ref weight="0.75" ref="basic"/>
    <tns:combine-ref weight="0.25" ref="advanced">
      <tns:nullify-condition compare-op="lt">
        <tns:nullify-combine-ref ref="basic"/>
        <tns:nullify-literal value="0.5"/>
      </tns:nullify-condition>
    </tns:combine-ref>
  </tns:root>
  <tns:combine id="basic" function="sum">   <!-- unchanged (as above) -->
    <tns:displaytitle>Basic functionality</tns:displaytitle>
    <tns:test-ref weight="0.3" ref="test1"/>
    <tns:test-ref weight="0.7" ref="test2"/>
  </tns:combine>
  <tns:combine id="advanced" function="min">    <!-- unchanged (as above) -->
    <tns:displaytitle>Advanced style aspects</tns:displaytitle>
    <tns:test-ref ref="test3"/>
    <tns:test-ref ref="test4"/>
  </tns:combine>
</tns:grading-hints>
```
#### Explanations

Nullify conditions specify conditions when the sub result of a pointed-at node should get nullified. It is possible to express simple comparison conditions like above ("basic functionality is less than 50%"). Also composite boolean expressions with boolean and- and or-operators can be formulated.

The above example nullify condition extends the tree topology by an arrow from the edge at the upper right to ``basic`` as shown in the following graphic:

```
                         +--------+
           +-------------+  root  +------------+
           |             +--------+            |
           |                                   |
           |     nullify if basic<0.5 +-------(|
           |                          |        |
     +-----+----+                     |   +----+-----+
     |  basic   +<--------------------+   | advanced |
     ++-------+-+                         +-+-------++
      |       |                             |       |
+-----+-+   +-+-----+                 +-----+-+   +-+-----+
| test1 |   | test2 |                 | test3 |   | test4 |
+-------+   +-------+                 +-------+   +-------+
```

A simple comparison condition is represented by the nullify-condition element. Composite boolean expressions are represented by the nullify-conditions element (note the trailing *s*). Both types of nullify conditions are evaluated when obtaining the score of a pointed-at node. If the condition is true, the score that flows into the accumulator function of the pointing-from-node (here: ``root``), is assumed to be emitted as 0 from the pointed-at node (here: ``advanced``). Stated differently, the score of the pointed-at node `` advanced`` is _not_ accumulated into the condensed result of the pointing-from-node ``root``, when the nullify condition evaluates true.

#### The nullify-conditions element for composite boolean expressions

A nullify-conditions element is attributed with one of the boolean operators { and, or }. Further it contains operands that usually are of the nullify-condition type, which represents a simple comparison. Nevertheless a composite condition can have nested composite conditions as operands as well.

The nullify-conditions element's base type (grades-nullify-base-type) defines titles and descriptions that could be displayed by a grader when explaining a score nullification to students or teachers. 

###### Code-Beispiel
```xml
<xs:complexType name="grades-nullify-base-type">
  <xs:sequence>
    <xs:element name="displaytitle" type="xs:string" minOccurs="0" />
    <xs:element name="description" type="tns:description-type" minOccurs="0" />
    <xs:element name="internal-description" type="tns:description-type" minOccurs="0" />
  </xs:sequence>
</xs:complexType>
<xs:complexType name="grades-nullify-conditions-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-nullify-base-type">
      <xs:sequence>
        <xs:choice minOccurs="2" maxOccurs="unbounded">
          <xs:element name="nullify-conditions" type="tns:grades-nullify-conditions-type" />
          <xs:element name="nullify-condition" type="tns:grades-nullify-condition-type" />
        </xs:choice>
      </xs:sequence>
      <xs:attribute name="compose-op" use="required">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="and" />
            <xs:enumeration value="or" />
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
```

#### The nullify-condition element for simple comparison expressions

A simple comparison condition is represented by the following nullify-condition element.

###### Code-Beispiel
```xml
<xs:complexType name="grades-nullify-condition-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-nullify-base-type">
      <xs:sequence>
        <xs:choice minOccurs="2" maxOccurs="2">
          <xs:element name="nullify-combine-ref" type="tns:grades-nullify-combine-ref-type" />
          <xs:element name="nullify-test-ref" type="tns:grades-nullify-test-ref-type" />
          <xs:element name="nullify-literal" type="tns:grades-nullify-literal-type" />
        </xs:choice>
      </xs:sequence>
      <xs:attribute name="compare-op" use="required">
        <xs:simpleType>
          <xs:restriction base="xs:string">
            <xs:enumeration value="eq" />
            <xs:enumeration value="ne" />
            <xs:enumeration value="gt" />
            <xs:enumeration value="ge" />
            <xs:enumeration value="lt" />
            <xs:enumeration value="le" />
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="grades-nullify-comparison-operand-type">
</xs:complexType>
<xs:complexType name="grades-nullify-combine-ref-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-nullify-comparison-operand-type">
      <xs:attribute name="ref" type="xs:string" use="required" />
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="grades-nullify-test-ref-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-nullify-comparison-operand-type">
      <xs:attribute name="ref" type="xs:string" use="required" />
      <xs:attribute name="sub-ref" type="xs:string" use="optional" />
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="grades-nullify-literal-type">
  <xs:complexContent>
    <xs:extension base="tns:grades-nullify-comparison-operand-type">
      <xs:attribute name="value" type="xs:decimal" use="required" />
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
```

##### Explanations

The simple comparison condition is attributed with one of the following six common comparison operators:

   * **eq**: means "equals"
   * **ne**: means "not equals"
   * **gt**: means "greater than"
   * **ge**: means "greater than or equals"
   * **lt**: means "less than"
   * **le**: means "less than or equals"

Further it contains operands that refer to tests (grades-nullify-test-ref-type) or combine nodes (grades-nullify-combine-ref-type) or that specify a numerical constant (grades-nullify-literal-type), which a result should be compared to:

   * **nullify-combine-ref**: When evaluating the condition, the numerical score calculated for the referenced combine node will be used as an operand in comparison.
   * **nullify-test-ref**: When evaluating the condition, the numerical score delivered by the referenced test will be used as an operand in comparison.
   * **nullify-literal**: A numerical constant serving as an operand of the comparison expression. The constant is specified as a "value" attribute in the nullify-literal element.
   
#### Cycles

A grading-hints element is valid only, if there are no cyclic dependencies in nullify conditions.

## General Structure of the Task Part

The task part consists of two parts: The first section shows
the description/specification of a task, including supporting files; and
the second section demonstrates a specification of tests which are to be
included in the specification of a task under the \<tests\> tag. Each
task can have many tests.

### XML Specification

The general structure of the task part is given as follows (this is
meant to provide an overview and does not represent a minimal document):

###### Code-Beispiel

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
in Version 3, 4 (see RFC 4122) or 5. There is no need for monitoring the uniqueness,
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

##### 1. By regexp

The archive may contain many files. The regular expression specifies, which files will be extracted from the archive.

- “unpack-files-from-archive-regexp” holds a regular expression that controls which files are automatically extracted. Only matching files (the whole path of the zip-items matches with “/” as path separator) are extracted from the archive (specified regexp language in [regexp-language-restriction] (#regexp-language-specification)).

###### Code-Beispiel

```xml
   <tns:submission-restrictions>
        <tns:archive-restrictions max-size="[size in bytes]" mime-type-regexp="[mimetype regexp]" unpack-files-from-archive="[true/false]">
        <tns:unpack-files-from-archive-regexp>regular expression</tns:unpack-files-from-archive-regexp>
        </tns:archive-restrictions>
    </tns:submission-restrictions>
```  
 
##### 2. By filenames

The archive must or may contain files as specified by the following file restrictions.

- <b>required</b> the archive must contain a file with specified "path" (rooted at the archive root) and optional "mime-type-regexp". Otherwise the submission should be rejected.
- <b>optional</b> the archive may contain a file with specified attributes

###### Code-Beispiel

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

#### File restriction

A submission must or may consist of files as specified by the file restrictions. A submission should be rejected, if it does not match the restrictions.

- <b>required</b> the submission must have a file with specified "path" (rooted at the archive root). Otherwise the submission should be rejected.
- <b>optional</b> the submission may have a file with specified attributes.

###### Code-Beispiel

```xml
   <tns:submission-restrictions>
            <tns:file-restrictions>
                <tns:required path="[full path to file]" mime-type-regexp="[mimetype regexp]" max-size="[size in bytes]"/>
                <tns:optional path="[full path to file]" mime-type-regexp="[mimetype regexp]" max-size="[size in bytes]"/>
            </tns:file-restrictions>
    </tns:submission-restrictions>
```

#### Regexp restriction

A submission must consist of one or several files, where all file names must adhere to the regular expression.

- <b>regexp-restriction</b> holds a regular expression of the filenames (only the filename, without path) which the system should accept. Regular expressions can contain less than/greater than signs. CDATA is allowed.

###### Code-Beispiel

```xml
   <tns:submission-restrictions>
            <tns:regexp-restriction mime-type-regexp="[mimetype regexp]" max-size="[size in bytes]">regular expression</tns:regexp-restriction>
    </tns:submission-restrictions>
```

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
    starting point for their solution. If a textfield is supplied for students to upload
    their submission, then this textfield should be pre-filled with the template.
    (Visible to students/irrelevant for grader)
-   <b>instruction</b>: The file contains further instructions for handling
    the task, e.g. an UML activity diagram. (Visible to students/irrelevant for grader)
-   <b>library</b>: The file is a library to be used by students. (Visible to students/used 
    by grader for compiling or linking)
-   <b>internal-library</b>: Like library, but not made available to students.
    (Not visible to students/used by grader for compiling or linking)
-   <b>source-code</b>: Similar to library, but pre-compiled, for example header libraries
    for C. (Visible to students/used by grader) 
-   <b>inputdata</b>: The file contains data which the algorithm of the
    students should work with. (Visible to students/used by grader at run-time)
-   <b>internal</b>: This holds files which may be required for processing the 
    task/tests within the system. Examples are JUnit tests and model solutions. How the
    file is used by the grader is specified by the test configurations.
    (Not visible to students/used by grader) 

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

The [grading-hints](Whitepaper_Introduction.md#grading-hints) element specifies, how a grader should calculate a total result from individual test results. Most ProFormA tasks define several tests. Every test is expected to generate a score from the interval [0,1]. The grading-hints element defines groups of tests and groups of groups in a tree like manner. The grading-hints element is specified as part of a task which can be overriden by a grading-hints element specified in a submission.

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

###### Code-Beispiel

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

