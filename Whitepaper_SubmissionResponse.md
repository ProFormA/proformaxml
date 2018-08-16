# An XML exchange format for (programming) tasks

**Version 2.0**

**THIS DOCUMENT IS IN AN ALPHA STAGE**

## Submission

Students provide submissions to tasks, usually by uploading them to an LMS. A submission consists of five parts that contain everything needed to grade a submission for a given task. 

```xml
<xs:complexType name="submission-type">
  <xs:sequence>
    <xs:choice>
      <xs:element name="external-task" type="tns:external-task-type"/>
      <xs:element name="task" type="tns:task-type"/>
      <xs:element name="inline-task-zip" type="tns:inline-task-zip-type"/>
    </xs:choice>
    <xs:element name="grading-hints" type="tns:grading-hints-type" minOccurs="0"/>
    <xs:choice>
      <xs:element name="external-submission" type="tns:external-submission-type"/>
      <xs:element name="files" type="tns:submission-files-type"/>
    </xs:choice>
    <xs:element name="lms" type="tns:lms-type" minOccurs="0"/>
    <xs:element name="result-spec" type="tns:result-spec-type"/>
  </xs:sequence>
</xs:complexType>
```

The task that the submission is for is part of the submission. The grading-hints may also be included, which, if specified, override the task's default version of the grading-hints. This is followed by the actual submission files, along with the submission context, such as LMS and submitting student details. At the end is the result specification as requested by the LMS.

### The task part

There are three different ways to include a task into a submission, either as an [XML element](#task-element), as an [inline-task-zip](#inline-task-zip), or as an [external-task](#external-task). Tasks are likely to be cached intensively by grading or middleware systems, which is why each option provides an easily available task-uuid attribute for quick access.

#### The task element

A task may be included as a regular XML element in the submission.

#### The inline-task-zip element

```xml
<xs:complexType name="inline-task-zip-type">
  <xs:simpleContent>
    <xs:extension base="xs:string">
      <xs:attribute name="uuid" type="xs:string" use="optional"/>
      <xs:attribute name="mimetype" type="xs:string" use="optional"/>
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

Using the inline-task-zip element, a task may be included as a ZIP file in the submission. The content of the inline-task-zip element must be encoded to Base64.

The inline-task-zip has the following attributes:

- **uuid**

    The uuid of the task. Inline tasks are likely to be cached, so the uuid serves as a convenience attribute to avoid repeated unpacking of (large) archive files.

- **mimetype**

    By default, the inline-task-zip should be formatted as ZIP. The mimetype may be specified in case another archive file format should be used.

#### The external-task element

TODO

### The grading-hints part

Teachers may prefer their own (modified) version of the [grading-hints](#) to the default ones that ship as part of a task. The grading-hints element is an optional part, which, if included, overrides the default grading-hints in the [task part](#the-task-part) of the submission. 

### The submission files part

Students provide solutions to programming tasks by submitting source code files (among other files). There are two ways to submit such files, either by including them into the submission document as [submission-files](#the-submission-file), or by using an already existing [external-submission](#the-external-submission) file.

#### The submission-file

```xml
<xs:complexType name="submission-file-type">
  <xs:complexContent>
    <xs:extension base="tns:file-type">
      <xs:choice>
        <xs:element name="embedded-file" type="tns:embedded-file-type"/>
        <xs:element name="attached-file" type="tns:attached-file-type"/>
        <xs:element name="attached-text-file" type="tns:attached-text-file-type"/>
      </xs:choice>
      <xs:attribute name="id" type="xs:string" use="optional"/>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="file-type" abstract="true">
  <xs:attribute name="mimetype" type="xs:string" use="optional"/>
</xs:complexType>
```

The submission-file may consist of an [embedded-file](#), an [attached-file](#), or an [attached-text-file](#).

The submission-file has the following attributes:

- **mimetype**

    An optional mimetype that can be useful to grading systems when handling all kinds of submission file types.

- **id**

    An optional ID attribute in case submission files need to be referred to by a response document within the [content element](#) of feedback entries. It should be noted that it is not possible to cross-reference the ID of a submission-files using a [fileref](#) element from within a response document. This is because the filerefs element is closely tied to the ID attribute of the [response-file element](#).

Note that source code (or any kind of text, for that matter) written inside an online text editor of an LMS can also be represented by submission-files (specifically the embedded-file and attached-text-file elements).

#### The external-submission element

Another way to specify a student submission is to provide a reference to an already existing submission by means of the external-submission element. 

TODO

### The LMS part

```xml
<xs:complexType name="lms-type">
  <xs:sequence>
    <xs:element name="submission-datetime" type="xs:dateTime"/>
    <xs:element name="user-id" type="xs:string" minOccurs="0" maxOccurs="unbounded"/>
    <xs:element name="course-id" type="xs:string" minOccurs="0"/>
    <xs:any namespace="##other" minOccurs="0" maxOccurs="unbounded"
            processContents="lax"/>
  </xs:sequence>
  <xs:attribute name="url" type="xs:string" use="optional"/>
</xs:complexType>
```

The LMS part contains general parameters within the context of the LMS.

- **url** <a name="lms-url"/>

    The base URL of the LMS.

- **submission-datetime**

    The date and time of the submission.

- **user-id**

    The user ID of the submitter within the LMS context, as an optional attribute. Multiple user IDs can be specified in case the submission is submitted by a group of students.

- **course-id** <a name="course-id"/>

    The id of the course within the LMS context. For instance, it could be an identity string as part of the URL of an LMS course.

If necessary, additional information can be provided in the any namespace element.

Using the [user-id](#) element along with the [task uuid](#) attribute, graders are able to put submissions into context, allowing for submission penalties. For example, if the teacher put submission attempt restrictions in place, such as a submission deadline, late submissions might result in a reduction of the total score.

### The result-spec part

```xml
<xs:complexType name="result-spec-type">
  <xs:sequence>
    <xs:element name="student-feedback-level" type="tns:feedback-level-type" minOccurs="0"/>
    <xs:element name="teacher-feedback-level" type="tns:feedback-level-type" minOccurs="0"/>
  </xs:sequence>
  <xs:attribute name="format" use="required">
    <xs:simpleType>
      <xs:restriction base="xs:string">
        <xs:enumeration value="xml"/>
        <xs:enumeration value="zip"/>
      </xs:restriction>
    </xs:simpleType>
  </xs:attribute>
  <xs:attribute name="structure" use="required">
    <xs:simpleType>
      <xs:restriction base="xs:string">
        <xs:enumeration value="merged-test-feedback"/>
        <xs:enumeration value="separate-test-feedback"/>
      </xs:restriction>
    </xs:simpleType>
  </xs:attribute>
  <xs:attribute name="lang" type="xs:string"/>
</xs:complexType>
```

The result-spec element is a result specification that the LMS may request from the grader. The element specifies the requested structure and format of response documents, the type of feedback, as well as the natural language preferred by the student. A grader should comply with the request (or part of it) whenever possible.

The result-spec element has the following attributes:

- **format**

    The format element specifies the content type of the response, which may come in two different types. For compatibility reasons, an LMS can prefer one format over the other. The grader should comply with the requested format. When the LMS submits a HTTP request (such as a GET request) to the grader, the grader should return the response in the requested format (xml or zip) and indicate the type used in the `Content-Type` field of the HTTP header to let the LMS know how to interpret the body of the response resource.

    * **xml**
    
        The response should be in the form of an XML document.
        
        Note that if the requested format is XML, the grader must include all files in the response document using the [embedded-file](#) element. This is because using the [attached-file](#) and [attached-text-file](#) elements would inevitably require the response document to be in the ZIP format.
        
    * **zip**

        The response should be in the ZIP file format.

- **structure**

    The structure attribute indicates how feedback should be structured in a response document.

    * **merged-test-feedback**
    
        This option specifies that the response document should contain a single feedback item for each audience (student and teacher). The feedback's [content](#) element is formatted as an HTML fragment containing all test scores and feedback entries in accordance with the specified [feedback-level](#the-student-feedback-level-and-teacher-feedback-level-elements).
    
        See the [merged-test-feedback](#) element for more details.

    * **separate-test-feedback**
    
        This option specifies that the submission response should contain comprehensive feedback, with a [feedback element](#) for every test and sub-test as listed in the [grading-hints](#the-grading-hints-part).

        See the [separate-test-feedback](#) element for more details.
       
- **lang**

    This is the student's preferred natural language that the feedback should be presented in.
    
    TODO: format

#### The student-feedback-level and teacher-feedback-level elements

These elements set the [minimum](#) [feedback-level](#) for student and teacher feedback that should be included in a response document, as a sort of filtering mechanism. Since a feedback-level also serves as a type of severity level, each feedback-level automatically includes all higher (more severe) levels. The LMS may use this to request for response documents to contain feedback entries with a "minimum" level. For example, if the LMS sets the student-feedback-level to "info", the document should also include all feedback entries with the "warn" and "error" level, but exclude any "debug" feedback in the student view.

The following table illustrates which feedback-levels will be visible as part of other levels, based on their severity.

<table >
  <tr>
    <td colspan="5" align="center"></td>
  </tr>
  <tr>
  	<td></td>
    <td>error</td>
  	<td>warn</td>
  	<td>info</td>
  	<td>debug</td>
  </tr>
  <tr>
  	<td>error</td>
  	<td align="center">X</td>
  	<td> </td>
  	<td> </td>
  	<td> </td>
  </tr>
  <tr>
  	<td>warn</td>
  	<td align="center">X</td>
  	<td align="center">X</td>
  	<td></td>
  	<td></td>
  </tr>
  <tr>
  	<td>info</td>
  	<td align="center">X</td>
  	<td align="center">X</td>
  	<td align="center">X</td>
  	<td></td>
    </tr>
  <tr>
  	<td>debug</td>
  	<td align="center">X</td>
  	<td align="center">X</td>
  	<td align="center">X</td>
  	<td align="center">X</td>
    </tr>
</table>

While the student-feedback-level and teacher-feedback-level are technically set apart, it is sometimes necessary for the teacher to see both their own and the student feedback, but not the other way around. It is for this very reason that the teacher-feedback-level element may be omitted entirely to avoid any potential feedback redundancies. However, it is important to note that compared to the student feedback, teacher feedback *may* contain more detailed information content. For instance, a teacher feedback might provide more details about the nature of an [error](#) than the corresponding student feedback. The actual extent of the information differences is up to the grader.

If neither student-feedback-level nor teacher-feedback-level are specified, no [feedback content](#content-element) will be included in the response document in case of [separate-test-feedback](#), and no [merged-feedback](#) in case of [merged-test-feedback](#).

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

The merged-test-feedback element holds two feedback elements that act as a single HTML "blob". This blob can be embedded and displayed accordingly in LMS that are less capable in terms of handling complex feedback structures. Additionally, merged-test-feedback provides a precalculated [overall-result](#overall-result) element, making the total score of a task easily accessible.

#### The merged-feedback element

```xml
<xs:simpleType name="merged-feedback-type">
  <xs:restriction base="xs:string"/>
</xs:simpleType>
```

The merged-feedback element contains the entire submission feedback formatted as a single HTML fragment. How the information is structured is up to the grader. For the sake of completeness, the feedback should include the feedback text, score, as well as any files relevant to the tests and sub-tests listed in the [grading-hints](#) element. Files should be included "in-line" in the HTML using the [Data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme) or JavaScript.

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

The result element holds the score, as well as the [score's validity](#) that a student achieved for a submission or a particular test (or sub-test).

- **is-internal-error**

    is-internal-error should be used to indicate an error that is not attributed to a student submission.
    
    For instance, if a grading system used a faulty JUnit test case to assess the correctness of a student's Java function, and that test case would result in a failure regardless of the correctness of the solution, the result would qualify as an internal error. Since the fault was not with the student's solution but with the grading system itself, the submission should be invalidated so the student would not lose their submission attempt (if such restrictions were in place). In order for this to work, the grader would need to return a response document with the **is-internal-error** attribute set to true in a [test-result](#) and a [feedback](#the-feedback-element) entry detailing the error. 
    TODO: On the other hand, how would the grader even know that the JUnit test is to blame and not the student's code? A failed JUnit test is just a failed test, after all. Probably need a better use case.

    Errors that are not directly related to a test should be indicated by other means rather than the is-internal-error flag. For example, if a grader received a poorly formatted submission document, it would have no choice but to reject the submission. Instead of using the is-internal-error attribute, it would be more reasonable to use a HTTP status code, such as "400 Bad Request" to indicate a client error.
    
### The separate-test-feedback element

```xml
  <xs:complexType name="separate-test-feedback-type">
    <xs:sequence>
      <xs:element name="submission-feedback-list" type="tns:feedback-list-type"/>
      <xs:element name="tests-response" type="tns:tests-response-type"/>
    </xs:sequence>
  </xs:complexType>
```

The separate-test-feedback element contains the general feedback for the entire submission and test-specific feedback for individual tests. Feedback may either be represented by plaintext or HTML. Test-specific feedback entries are connected to their corresponding tests via a test-id or sub-test-id as specified in the grading-hints. 

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

The feedback-list contains zero or more [feedback](#the-feedback-element) elements for both the student and teacher. While a response document structured in the form of [separate-test-feedback](#) must contain a [result](#) element for each [test-response](), the test-responses are not required to have any feedback, depending on the settings used in the result specification (see [student-feedback-level and teacher-feedback-level](#)).

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

- **content**

     The content element contains the actual feedback text and has a **format** attribute indicating whether the text is formatted as plaintext or HTML.

    * **plaintext**
    
        The feedback text is formatted as plaintext and should be displayed as [preformatted text](https://www.w3.org/MarkUp/html3/literal.html) in the LMS.

    * **html**
    
        The feedback text is formatted as a HTML fragment, wrapped inside a CDATA or PCDATA section. In case of PCDATA, markup-specific entities such as `<` and `>` must be escaped as `&lt;` and `&gt;`, respectively.
        
        HTML may include files "in-line" using the [Data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme) or JavaScript.

- **filerefs** <a name="feedback-type-filerefs"/>

    The filerefs element contains a list of file references to [files](#) that are relevant to a particular feedback element. They are presented as downloadable links to the student and teacher. 
    
    Since a file can be referred to by multiple feedback elements, response documents use filerefs rather than duplicating [response-file](#) elements.

- **level**

    The [level](#) of a feedback entry. Specifying a level is optional.

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

The test-response element represents the result for a single test. It may consist of a single [test-result]() element or a list of [subtest-responses](). Which one to choose depends on the situation. For instance, if a test is partitioned into multiple sub-tests in the grading-hints, it would make sense for the test-response to contain a list of subtest-responses, each one referring to the corresponding sub-test in the grading-hints. It would also allow for a finer breakdown of the test score and feedback. However, if the entire test case were to fail, it would probably not make a lot of sense to have all subtest-responses contain the same error message. Using a single test-result would be more appropriate in this case, as the error message would appear in the LMS only once.

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

The test-result element holds the result and the [feedback](#feedback-list) for a submission or an individual test.

#### The subtest-response element

```xml
<xs:complexType name="subtest-response-type">
  <xs:sequence>
    <xs:element name="test-result" type="tns:test-result-type"/>
  </xs:sequence>
  <xs:attribute name="id" use="required" type="xs:string"/>
</xs:complexType>
```

The subtest-response element consists of a [test-result](#) element and an **id** attribute. The test-result element contains the results for this particular sub-test.

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

The response-file element may consist of an [embedded-file](#), an [attached-file](#), or an [attached-text-file](#).

It has the following attributes:

- **mimetype**

    An optional mimetype that can be useful to the LMS when handling all kinds of response file types.

- **id**

    The ID attribute is used to refer to files using [filerefs](#).
    
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

## General element types

#### The embedded-file element

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

The embedded-file element is used to embed a file directly into the submission's XML. Files that contain binary data must be encoded to Base64 and the **is-base64-encoded** attribute set to true. Plaintext file content must be encoded to UTF-8. Embedded files also require a **filename**.

##### The attached-file element

```xml
<xs:simpleType name="attached-file-type">
  <xs:restriction base="xs:string"/>
</xs:simpleType>
```

The attached-file element is used to attach arbitrary files to a [submission ZIP archive](#). This is especially useful when we are dealing with files that we do not want to embed in XML for various reasons (e. g. the files in question are particularly large in size). The element content is the relative path to the file within the ZIP file.

Note that while it is possible to use the attached-file element for any kind of file, the [attached-text-file](#) element should be the preferred way to attach plaintext files to a submission.

#### The attached-text-file element

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

The attached-text-file element is used to exclusively attach plaintext files to a [submission ZIP archive](#). It comes with a few optional attributes that are particularly useful when dealing with plaintext.

- **encoding** 

    The encoding of the text file, as an optional attribute.

- **natural-language** 

    The natural-language attribute specifies the natural language of the submitting student. Students tend to use all kinds of encodings in their text files. Most of the time, the encoding will be unknown at the time of submission. To address this problem, the natural-language attribute can be used to help the grader detect the encoding of a submitted plaintext file file.
    
    It should be said that the natural-language attribute does not necessarily have to be the same as the one provided in the task's [lang](#) attribute. While the lang attribute indicates the language that the task has been written in, a student might use a different language when writing their text entirely.
    
    Providing a value for the natural-language attribute could be as simple as retrieving a preconfigured value, like the language the student configured in their user profile of the LMS.

    In the case that encoding and natural-language should happen to contradict each other, encoding is given precedence.

    TODO: natural-language format

### The feedback-level element

Usually, feedback can be grouped into different types of information. 

- **debug**

    Debug feedback contains information normally only relevant to debugging a grading system.
        
- **info**

    Generally useful information to students and teachers, e. g. a specific test case passed successfully.
    
- **warn**

    The feedback contains a warning, e. g. the function being tested took up too much processing time. Or the student used a deprecated class method.
    
- **error**

    The feedback contains error information, e. g. the student's source code resulted in a compile-time error that caused the entire test case to fail. This error type should not be confused with the [is-internal-error](#is-internal-error) flag of a test result, which indicates that an error occurred on the part of the grading system.
