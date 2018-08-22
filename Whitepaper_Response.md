# An XML exchange format for (programming) tasks

**Version 2.0**

## Response

```xml
<xs:complexType name="response-type">
  <xs:sequence>
    <xs:choice>
     <xs:element name="merged-test-feedback" type="tns:merged-test-feedback-type"/>
     <xs:element name="separate-test-feedback" type="tns:separate-test-feedback-type"/>
    </xs:choice>
   <xs:element name="files" type="tns:response-files-type"/>
   <xs:element name="response-meta-data" type="tns:response-meta-data-type"/>
  </xs:sequence>
  <xs:attribute name="lang" type="xs:string"/>
</xs:complexType>
```

The response contains the results of a graded submission.

### The merged-test-feedback element

```xml
<xs:complexType name="merged-test-feedback-type">
  <xs:sequence>
    <xs:element name="overall-result" type="tns:result-type"/>
    <xs:element name="student-feedback" type="tns:merged-feedback-type" minOccurs="0"/>
    <xs:element name="teacher-feedback" type="tns:merged-feedback-type" minOccurs="0"/>
  </xs:sequence>
</xs:complexType>
```

The merged-test-feedback element holds two feedback elements that act as a single HTML "blob". This blob can be embedded and displayed accordingly in LMS that are less capable in terms of handling complex feedback structures. Additionally, merged-test-feedback provides a precalculated [overall-result](#the-result-element) element, making the total score of a task easily accessible.

#### The merged-feedback element

```xml
<xs:simpleType name="merged-feedback-type">
  <xs:restriction base="xs:string"/>
</xs:simpleType>
```

The merged-feedback element contains the entire submission feedback formatted as a single HTML fragment. How the information is structured is up to the grader. For the sake of completeness, the feedback should include the feedback text, score, as well as any files relevant to the tests and sub-tests listed in the [grading-hints](Whitepaper_Introduction.md) element. Files should be included "in-line" in the HTML using the [Data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme) or JavaScript.

#### The result element

```xml
  <xs:complexType name="result-type">
    <xs:sequence>
      <xs:element name="score" type="tns:score-type"/>
      <xs:element name="validity" minOccurs="0" type="tns:validity-type"/>
    </xs:sequence>
    <xs:attribute name="is-internal-error" type="xs:boolean" default="false"/>
  </xs:complexType>
```

The result element holds the score, as well as the score's validity that a student achieved for a submission or a particular test (or sub-test). The optional element "validity" is used to partially verify the score of a result.

- <a name="is-internal-error"/> **is-internal-error**

    is-internal-error should be used to indicate an error that is not attributed to a student submission.

    For instance, if a grading system used a faulty JUnit test to assess the correctness of a student's Java source code, and that test case would always result in a failure regardless of the correctness of the solution (due to the grader not being able to compile the JUnit test, for example), the result would qualify as an internal error. Since the fault was not with the student's solution but with the grading system itself, the submission should be invalidated so the student would not lose their submission attempt (if such restrictions were in place). In order for this to work, the grader would need to return a response document with the **is-internal-error** attribute set to true in a [test-result](#the-test-result-element) and a [feedback](#the-feedback-element) entry detailing the error.

    Errors that are not directly related to a test should be indicated by other means rather than the is-internal-error flag. For example, if a grader received a poorly formatted submission document, it would have no choice but to reject the submission. Instead of using the is-internal-error attribute, it would be more appropriate to use the underlying communication protocol's error handling mechanism, such as using a HTTP status code (e. g. "400 Bad Request") to indicate a client error.

### The separate-test-feedback element

```xml
  <xs:complexType name="separate-test-feedback-type">
    <xs:sequence>
      <xs:element name="submission-feedback-list" type="tns:feedback-list-type"/>
      <xs:element name="tests-response" type="tns:tests-response-type"/>
    </xs:sequence>
  </xs:complexType>
```

The separate-test-feedback element contains the general feedback for the entire submission and test-specific feedback for individual tests. Feedback may either be represented by plaintext or HTML. Test-specific feedback entries are connected to their corresponding tests via a test id or sub-test id as specified in the grading-hints.

Calculating a submission's total score based on partial results of separate tests, as well as presenting these results along with the feedback to the student (or teacher) must be handled by the LMS.

#### The feedback-list element

```xml
<xs:complexType name="feedback-list-type">
  <xs:sequence>
    <xs:element name="student-feedback" minOccurs="0" maxOccurs="unbounded"
                type="tns:feedback-type"/>
    <xs:element name="teacher-feedback" minOccurs="0" maxOccurs="unbounded"
                type="tns:feedback-type"/>
  </xs:sequence>
</xs:complexType>
```

The feedback-list contains zero or more [feedback](#the-feedback-element) elements for both the student and teacher. While a response document structured in the form of [separate-test-feedback](#the-separate-test-feedback-element) must contain a [result](#the-result-element) element for each [test-response](#the-test-response-element), the test-responses are not required to have any feedback, depending on the settings used in the result specification (see [student-feedback-level and teacher-feedback-level](Whitepaper_Submission.md#the-student-feedback-level-and-teacher-feedback-level-elements)).

#### The feedback element

```xml
<xs:complexType name="feedback-type">
  <xs:sequence>
    <xs:element name="title" minOccurs="0" type="xs:string"/>
    <xs:element name="content" minOccurs="0">
      <xs:complexType>
        <xs:simpleContent>
          <xs:extension base="xs:string">
            <xs:attribute name="format" use="required">
              <xs:simpleType>
                <xs:restriction base="xs:string">
                  <xs:enumeration value="html"/>
                  <xs:enumeration value="plaintext"/>
                </xs:restriction>
              </xs:simpleType>
            </xs:attribute>
          </xs:extension>
        </xs:simpleContent>
      </xs:complexType>
    </xs:element>
    <xs:element name="filerefs" type="tns:filerefs-type" minOccurs="0"/>
  </xs:sequence>
  <xs:attribute name="level" type="tns:feedback-level-type"/>
</xs:complexType>
```

The feedback element is the immediate feedback for a specific test, a sub-test, or a submission as a whole.

The feedback element consists of four parts:

- **title**

    The title is meant to provide a quick and minimal summary of the feedback's content and should be formatted as plaintext. Multiple consecutive whitespace characters might be collapsed to a single whitespace character by the LMS.

- <a name="feedback-type-content"/> **content**

     The content element contains the actual feedback text and has a **format** attribute indicating whether the text is formatted as plaintext or HTML.

    * **plaintext**

        The feedback text is formatted as plaintext and should be displayed as [preformatted text](https://www.w3.org/MarkUp/html3/literal.html) in the LMS.

    * **html**

        The feedback text is formatted as a HTML fragment, wrapped inside a CDATA or PCDATA section. In case of PCDATA, markup-specific entities such as `<` and `>` must be escaped as `&lt;` and `&gt;`, respectively.

        HTML may include files "in-line" using the [Data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme) or JavaScript.

- <a name="feedback-type-filerefs"/> **filerefs**

    The filerefs element contains a list of file references to [files](#the-response-file-element) that are relevant to a particular feedback element. They are presented as downloadable links to the student and teacher.

    Since a file can be referred to by multiple feedback elements, response documents use filerefs rather than duplicating [response-file](#the-response-file-element) elements.

- **level**

    The [level](Whitepaper_Introduction.md#the-feedback-level) of a feedback entry. Specifying a level is optional.

#### The test-response element

```xml
<xs:complexType name="test-response-type">
  <xs:choice>
    <xs:element name="test-result" type="tns:test-result-type"/>
    <xs:element name="subtests-response" type="tns:subtests-response-type"/>
  </xs:choice>
  <xs:attribute name="id" use="required" type="xs:string"/>
</xs:complexType>
```

The test-response element represents the result for a single test. It may consist of a single [test-result](#the-test-result-element) element or a list of [subtest-responses](#the-subtest-response-element). Which one to choose depends on the situation. For instance, if a test is partitioned into multiple sub-tests in the grading-hints, it would make sense for the test-response to contain a list of subtest-responses, each one referring to the corresponding sub-test in the grading-hints. It would also allow for a finer breakdown of the test score and feedback. However, if the entire test case were to fail, it would probably not make a lot of sense to have all subtest-responses contain the same error message. Using a single test-result would be more appropriate in this case, as the error message would appear in the LMS only once.

- **id**

    The id attribute is used to refer to the corresponding test element in the grading-hints.

#### The test-result element

```xml
<xs:complexType name="test-result-type">
  <xs:sequence>
    <xs:element name="result" type="tns:result-type"/>
    <xs:element name="feedback-list" type="tns:feedback-list-type"/>
  </xs:sequence>
</xs:complexType>
```

The test-result element holds the result and the [feedback](#the-feedback-list-element) for a submission or an individual test.

#### The subtest-response element

```xml
<xs:complexType name="subtest-response-type">
  <xs:sequence>
    <xs:element name="test-result" type="tns:test-result-type"/>
  </xs:sequence>
  <xs:attribute name="id" use="required" type="xs:string"/>
</xs:complexType>
```

The subtest-response element consists of a [test-result](#the-test-result-element) element and an **id** attribute. The test-result element contains the results for this particular sub-test.

Using the id attribute, a subtest-response is connected to the corresponding sub-test (and thus, the parent test) that is listed in the grading-hints.

### The response-file element

```xml
<xs:complexType name="response-file-type">
  <xs:complexContent>
    <xs:extension base="tns:file-type">
      <xs:choice>
        <xs:element name="embedded-file" type="tns:embedded-file-type"/>
        <xs:element name="attached-file" type="tns:attached-file-type"/>
        <xs:element name="attached-text-file" type="tns:attached-text-file-type"/>
      </xs:choice>
      <xs:attribute name="id" type="xs:string" use="required"/>
      <xs:attribute name="title" type="xs:string" use="required"/>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="file-type" abstract="true">
  <xs:attribute name="mimetype" type="xs:string" use="optional"/>
</xs:complexType>
```

The response-file element may consist of an [embedded-file](Whitepaper_Introduction.md#the-embedded-file-element), an [attached-file](Whitepaper_Introduction.md#the-attached-file-element), or an [attached-text-file](Whitepaper_Introduction.md#the-attached-text-file-element).

It has the following attributes:

- **mimetype**

    An optional mimetype that can be useful to the LMS when handling all kinds of response file types.

- **id**

    The ID attribute is used to refer to files using [filerefs](#feedback-type-filerefs).

    Note that within a response document, filerefs must exclusively reference response-file elements to ensure referential integrity.

- **title**

    The title serves as a short description of a file. It may be used as the link text for downloadable file links in the [filerefs](#feedback-type-filerefs) section. This is useful if responses tend to contain files with cryptic filenames.

Note that text, such as source code, written inside an online editor of an LMS can also be represented by a submission-file (specifically the attached-text-file element).

### The response-meta-data element

```xml
<xs:complexType name="response-meta-data-type">
  <xs:sequence>
    <xs:element name="grader-engine" type="tns:grader-engine-type"/>
    <xs:any namespace="##other" minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
  </xs:sequence>
</xs:complexType>
```

The response-meta-data element contains information about the grading system used to grade the submission, like the grader's name and version, as well as an any namespace for any additional meta data relevant to the submission response.
