proformaxml (Beta - not yet completely finished)
=======
This project is about creating an XML exchange format for programming exercises. The project is maintained by members of the ProFormA group within the eCULT project (www.ecult-niedersachsen.de).

ProFormA stands for FORMatives Assessment of PROgramming exercises. Such exercises are for example Java programming exercises which are automatically assessed by JUnit, compilation and checkstyle tests. Because it is time consuming to create such exercises, it is helpful for lecturers if they can exchange exercises with other lecturers. Because different universities employ different tools for implementing such exercises, an export-import format is needed to facilitate exchanging exercises. That is the purpose of this ProFormA-XML format. 

The ProFormA-XML format is supplied as an XSD that describes the XML. The XML contains up to 3 top elements (for the task, the submission and the response).

Older stable versions are kept here: https://github.com/ProFormA/taskxml
Up to version 1.1 only the task details were specified by the format. In 2018 it was decided to create a new version (2.0) which also specifies the details for the submission (what information has to be transmitted in addition to the file(s) provided by the student) and the response (what the grader sends back to the LMS). 


History: even older versions are at http://teach.ecult.me/proforma/
