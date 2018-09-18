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

All three main parts of the ProFormA format make use of files in various ways. The following file elements present different ways to attach files of any kind to a task, a submission, and a response document.

### The embedded-bin-file element

##### Code-Beispiel
```xml
<xs:complexType name="embedded-bin-file-type">
  <xs:simpleContent>
    <xs:extension base="xs:base64Binary">
      <xs:attribute name="filename" type="xs:string" use="required"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

The embedded-bin-file element is used to embed a file containing binary content directly into XML. The file content must be encoded to Base64. embedded-bin-file requires a **filename**.

### The embedded-txt-file element

##### Code-Beispiel
```xml
<xs:complexType name="embedded-txt-file-type">
  <xs:simpleContent>
    <xs:extension base="xs:string">
      <xs:attribute name="filename" type="xs:string" use="required"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

The embedded-txt-file element is used to embed a file containing plaintext content directly into XML. The file content must be encoded to UTF-8, which is the same encoding that the XML document is encoded in. embedded-txt-file requires a **filename**.

### The attached-bin-file element

##### Code-Beispiel
```xml
<xs:simpleType name="attached-bin-file-type">
  <xs:restriction base="xs:string"/>
</xs:simpleType>
```

The attached-bin-file element is used to attach files containing binary content to ZIP archives. This is especially useful when we are dealing with files that we do not want to embed in XML for various reasons (e. g. the files in question are particularly large in size).

The relative path to the binary file within the ZIP archive is specified in the element content (i. e. the element's text node).

### The attached-text-file element

##### Code-Beispiel
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

## The feedback-level

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

A grading-hints section of a ProFormA task or a ProFormA submission defines, how a grader should calculate a total result from individual test results. Most ProFormA tasks define several tests. Every test is expected to generate a score from the interval [0,1]. The grading-hints element defines groups of tests and groups of groups in a tree like manner. This way the grading-hints element includes the complete hierarchical grading scheme with all tests references, weights, accumulating functions and nullify conditions. Hierarchy nodes and conditions can get a displaytitle and descriptions. All information below the grading-hints element except the root node is optional. 

```xml
<xs:complexType name="grading-hints-type">
  <xs:sequence>
    <xs:element name="root" type="tns:grades-node-type"/>
    <xs:element name="combine" type="tns:grades-node-type" minOccurs="0" maxOccurs="unbounded"/>
    <xs:any namespace="##other" minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
  </xs:sequence>
</xs:complexType>
```

First the grading-hints-type defines the root element. This is the root node of the grading scheme hierarchy. If the root does not specify any child nodes, the total grading score will be obtained by including all test results scores. A "function" attribute (see below) specifies the accumulator function.

combine elements are inner nodes of the grading scheme hierarchy - either as an immediate child of the root node or as a further descendant node. A combine node specifies how to condense several sub results. Sub results can be test results or again "combined" results.

Grader-specific hints from other XML namespaces can be included with the grading-hints element (xs:any). This could be any non-standard information that can be used by a grader or humans to calculate a total result from tests results.

### root and combine elements

The above root and combine elements both are of the following grades-node-type:

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

The grades-node-type represents an inner node of the grading scheme hierarchy. There are only two types of inner nodes: the "root" node and "combine" nodes. Both define a sequence of child references. All child references point to objects representing either a test result or a combined result. The combine node includes these referenced results in a bottom-up fashion to calculate an accumulated combined result. The function attribute defines the accumulator function. 

A test-ref element points to a test element in a ProFormA task. A combine-ref element points to a combine element in the grading scheme hierarchy. Both elements are described in detail below.

The function attribute defines the accumulator function that is used to condense several sub results to a single result. Currently there are three functions to choose from:
   
   * **sum**: Specifies the sum of several sub scores. This is used in a situation, where every child represents a problem aspect that could be solved more or less independently of the other aspects. Weights (see below) can be attached to child node references. Those child nodes representing easy problem aspects could get lower weights than other aspects. If all weights of all direct children of a node add up to 1, this would guarantee, that the parent node result is in [0,1] when all child nodes results are in [0,1].
   * **min**: Specifies the minimum of several sub scores. This can be used in an "all or nothing" situation, where a parent score should reflect the worst of the child results. Weights can be attached to child node references to express the valency of a child's result. The child node representing the easiest aspect among its siblings could get the weight 1. Child nodes for grading aspects connected with a higher effort represent scores that are more difficult to achieve. These child nodes could get weights larger than 1. This would guarantee, that when all child nodes results are in [0,1] also the parent node result is in [0,1].
   * **max**: Specifies the maximum of several sub scores. This is used in an "one success is enough" situation, where a parent score should reflect the best of the child results. An example is a task or a graded problem aspect that could be solved in different ways and for each way there is a separate test element in the task. A solution that succeeds any one of these tests is regarded successful. If one of the different ways of solving the task is more sophisticated than the others, the respective child test could get the highest weight 1. Easier, less valent solution paths get lower weights between 0 and 1. This would guarantee, that when all child nodes results are in [0,1] also the parent node result is in [0,1].

The ``id`` attribute is optional for the root element and required for combine elements.

A valid grading-hints element requires every combine element to be referenced by another combine or root element. All combine elements must have a parent node. Currently the format requires a combine node's parent to be unique. Especially, orphan combine elements are not allowed. The root element is the only element without a parent.

### test-ref and combine-ref

The above combine-ref and test-ref elements both are derived from a common base type, namely grades-base-ref-child-type which is described below. To illustrate these elements, let's think about the following typical example of two combine elements and one root element:

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

A combine-ref element's ``ref`` attribute points to the ``id`` attribute of the referenced combine element. The same way a test-ref element's ``ref`` attribute points to the ``id`` attribute of the referenced test element. When the pointed at test exhibits sub test results, the sub-ref attribute points to one of the sub results. Examples are individual test cases in a unit test specification, individual violation rules in a static code sytle analyzer, individual error classes in a compilation step, etc. Since the sub-ref format or content is test-tool-specific, it is not normed in the ProFormA format.

Both, combine-ref and test-ref elements, specifiy a weight that is multiplied to the sub result value of the pointed-at node when flowing into the accumulator function (sum, min, or max). When calculating the accumulated result for the pointing-from node, the score of the pointed-at node is multiplied by the weight. If no weight is present, the score of the pointed-to node is multiplied by 1.

A special feature for the test-ref element are the displaytitle, description and internal-description elements. These override the title or descriptions of the pointed-at test element.  This can be used especially when pointing to sub test results.
   
A last important element of a child reference (test-ref or combine-ref) are so-called nullify conditions, that are described in the following section.

### Nullify conditions

Sometimes a teacher or a task author wants to nullify scores for advanced style aspects if the basic functionality aspects do not exceed a certain threshold. This seems reasonable when we take a closer look at static code analysis tools ("style checker") that often count rule violations. In the above example a student can easily achieve high scores in test3 and test4 when submitting a minimal program that has near to zero functionality. For this, the task author includes a nullify condition at the child reference to the advanced child:

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


A nullify-conditions element is attributed with one of the boolean operators { and, or }. Further it contains operands that usually are of the nullify-condition type, which represents a simple comparison. Nevertheless a composite condition can have nested composite conditions as operands as well.

The nullify-conditions element's base type (grades-nullify-base-type) defines titles and descriptions that could be displayed by a grader when explaining a score nullification to students or teachers. 

#### The nullify-condition element for simple comparison expressions

A simple comparison condition is represented by the following nullify-condition element.

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
