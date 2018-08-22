# An XML exchange format for (programming) tasks

**Version 2.0**

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

There are three different ways to include a task into a submission, either as an [XML element](#the-task-element), as an [inline-task-zip](#the-inline-task-zip-element), or as an [external-task](#the-external-task-element). Tasks are likely to be cached intensively by grading or middleware systems, which is why each option provides an easily available task-uuid attribute for quick access.

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

Teachers may prefer their own (modified) version of the [grading-hints](Whitepaper_Introduction.md#grading-hints) to the default ones that ship as part of a task. The grading-hints element is an optional part, which, if included, overrides the default grading-hints in the task part of the submission. 

### The submission files part

Students provide solutions to programming tasks by submitting source code files (among other files). There are two ways to submit such files, either by including them into the submission document as [submission-files](#the-submission-files-part), or by using an already existing [external-submission](#the-external-submission-element) file.

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

The submission-file may consist of an [embedded-file](Whitepaper_Introduction.md#the-embedded-file-element), an [attached-file](Whitepaper_Introduction.md#the-attached-file-element), or an [attached-text-file](Whitepaper_Introduction.md#the-attached-text-file-element).

The submission-file has the following attributes:

- **mimetype**

    An optional mimetype that can be useful to grading systems when handling all kinds of submission file types.

- **id**

    An optional ID attribute in case submission files need to be referred to by a response document within the [content element](Whitepaper_Response.md#feedback-type-content) of feedback entries. It should be noted that it is not possible to cross-reference the ID of a submission-files using a [fileref](Whitepaper_Response.md#feedback-type-filerefs) element from within a response document. This is because the filerefs element is closely tied to the ID attribute of the [response-file element](Whitepaper_Response.md#the-response-file-element).

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

- <a name="user-id"/> **user-id**

    The user ID of the submitter within the LMS context, as an optional attribute. Multiple user IDs can be specified in case the submission is submitted by a group of students.

- <a name="course-id"/> **course-id**

    The id of the course within the LMS context. For instance, it could be an identity string as part of the URL of an LMS course.

If necessary, additional information can be provided in the any namespace element.

Using the [user-id](#user-id) element along with the [task uuid](Whitepaper_Task.md#task-attributes) attribute, graders are able to put submissions into context, allowing for submission penalties. For example, if the teacher put submission attempt restrictions in place, such as a submission deadline, late submissions might result in a reduction of the total score.

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

    The format element specifies the content type of the response, which may come in two different types. For compatibility reasons, an LMS can prefer one format over the other. The grader should comply with the requested format. When the LMS submits a request (such as a HTTP GET request) for a response document, the grader should return the response in the requested format (xml or zip) and indicate the type used (as in the `Content-Type` field of the HTTP header to let the LMS know how to interpret the body of the response resource).

    * **xml**
    
        The response should be in the form of an XML document.
        
        Note that if the requested format is XML, the grader must include all files in the response document using the [embedded-file](Whitepaper_Introduction.md#the-embedded-file-element) element. This is because using the [attached-file](Whitepaper_Introduction.md#the-attached-file-element) and [attached-text-file](Whitepaper_Introduction.md#the-attached-text-file-element) elements would inevitably require the response document to be in the ZIP format.
        
    * **zip**

        The response should be in the ZIP file format.

- **structure**

    The structure attribute indicates how feedback should be structured in a response document.

    * <a name="merged-test-feedback"/> **merged-test-feedback**
    
        This option specifies that the response document should contain a single feedback item for each audience (student and teacher). The feedback's [content](Whitepaper_Response.md#feedback-type-content) element is formatted as an HTML fragment containing all test scores and feedback entries in accordance with the specified [feedback-level](#the-student-feedback-level-and-teacher-feedback-level-elements).
    
        See the [merged-test-feedback](Whitepaper_Response.md#the-merged-test-feedback-element) element for more details.

    * <a name="separate-test-feedback"/> **separate-test-feedback**
    
        This option specifies that the submission response should contain comprehensive feedback, with a [feedback element](Whitepaper_Response.md#the-feedback-element) for every test and sub-test as listed in the [grading-hints](Whitepaper_Introduction.md#grading-hints).

        See the [separate-test-feedback](Whitepaper_Response.md#the-separate-test-feedback-element) element for more details.
       
- **lang**

    This is the student's preferred natural language that the feedback should be presented in.
    
    Its value should be formatted as ISO 639-1 for language codes, and as ISO 3166-1 alpha-2 for country codes.

#### The student-feedback-level and teacher-feedback-level elements

These elements set the minimum [feedback-level](Whitepaper_Introduction.md#the-feedback-level) for student and teacher feedback that should be included in a response document, as a sort of filtering mechanism. Since a feedback-level also serves as a type of severity level, each feedback-level automatically includes all higher (more severe) levels. The LMS may use this to request for response documents to contain feedback entries with a "minimum" level. For example, if the LMS sets the student-feedback-level to "info", the document should also include all feedback entries with the "warn" and "error" level, but exclude any "debug" feedback in the student view.

The following table illustrates which feedback-levels will be visible as part of other levels, based on their severity.

<table >
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

While the student-feedback-level and teacher-feedback-level are technically set apart, it is sometimes necessary for the teacher to see both their own and the student feedback, but not the other way around. It is for this very reason that the teacher-feedback-level element may be omitted entirely to avoid any potential feedback redundancies. However, it is important to note that compared to the student feedback, teacher feedback *may* contain more detailed information content. For instance, a teacher feedback might provide more details about the nature of an error than the corresponding student feedback. The actual extent of the information differences is up to the grader.

If neither student-feedback-level nor teacher-feedback-level are specified, no [feedback content](Whitepaper_Response.md#feedback-type-content) will be included in the response document in case of [separate-test-feedback](#separate-test-feedback), and no [merged-feedback](Whitepaper_Response.md#the-merged-feedback-element) in case of [merged-test-feedback](#merged-test-feedback).
