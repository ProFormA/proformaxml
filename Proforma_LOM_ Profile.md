<table>
	<tr>
		<th>Element</th>
		<th>Content (in context ProFormA)</th>
		<th>Referenced Element/Attribute in Task XML</th>
		<th>Value space</th>
		<th>Data type</th>
		<th>Example</th>
		<th>Usage</th>
		<th>Usage (LOM-DE)</th>
	</tr>
	<tr>
		<td><b>General</b></td>
		<td><b>General information about the task</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>mandatory</b></td>
	</tr>
	<tr>
		<td><sub><b>identifier</b></sub></td>
		<td><sub>globaly unique identifier for the task</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>catalog</sub></td>
		<td><sub>Name/type of identifier used</sub></td>
		<td></td>
		<td><sub>"UUID"</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>entry</sub></td>
		<td><sub>uuid of the task</sub></td>
		<td><sub>task attribute <b>uuid</b></sub></td>
		<td><sub>any uuid</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>title</b></sub></td>
		<td><sub>Title of the task</sub></td>
		<td><sub>task element <b>title</b></sub></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td><sub></sub></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>language</b></sub></td>
		<td><sub>Natural main language used for the task in order to communicate to a user</sub></td>
		<td><sub>task attribute <b>lang<b></sub></td>
		<td><sub>2 Letter Code from ISO-639-1 (3 Letter Code from ISO 639-2 only if there isn’t a 2 letter code available) + ISO Country code [ISO3166] when necessary, separated by a dash</sub></td>
		<td><sub>CharacterString</sub></td>
		<td><sub>"de", "de-CH"</sub></td>		
		<td><sub>recommended</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>description</b></sub></td>
		<td><sub>The description of the task</sub></td>
		<td><sub>task element <b>description</b></sub></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>keywords</b></sub></td>
		<td><sub>Keywords describing the topic of the task (for search), should contain at least programming language and programming language version</sub></td>
		<td><sub><b>keyword programming language:</b> task element proglang<br><br><b>keyword programming language version:</b> task element proglang attribute version</sub></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>structure</b></sub></td>
		<td><sub>Underlying organizational structure of the learning object</sub></td>
		<td></td>
		<td><sub>"atomic" (task.xml only), "collection" (task.xml in zip file)</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>mandatory? (Value automatisch vom System festlegen lassen)</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>coverage</b></sub></td>
		<td><sub>Time, culture or region to which the learning object applies</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td><sub>"2019", "Niedersachsen"</sub></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>aggregation level</b></sub></td>
		<td><sub>Functional granularity of the learning object (number of times the learning object can be decomposed into smaller ones)</sub></td>
		<td></td>
		<td><sub>"1" (the smallest level of aggregation, e.g., raw media data or fragments), "2" (: a collection of level 1 learning objects, e.g., a lesson), "3" (a collection of level 2 learning objects, e.g., a course), "4" (the largest level of granularity, e.g., a set of courses that lead to a certificate)</sub></td>
		<td><sub>VocabularyTerm (enumerated) (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>n/a oder optional?</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><b>Life Cycle</b></td>
		<td><b>Information about tasks release state, version and change history</td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>recommended</b></td>
	</tr>
	<tr>
		<td><sub><b>version</b></sub></td>
		<td><sub>Versionnumber of the task</sub></td>
		<td></td>
		<td><sub>0 iff parent-uuid is empty?</sub></td>
		<td></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>status</b></sub></td>
		<td><sub>Release status</sub></td>
		<td></td>
		<td><sub>"draft", "final", "revised", "unavailable"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>contribute</b></sub></td>
		<td><sub>Information about entities that contributed to the state of the task</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>recommended oder mandatory?</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>role</sub></td>
		<td><sub>Role of contributor</sub></td>
		<td></td>
		<td><sub>"author", "publisher", "unknown", "initiator", "terminator", "validator", "editor", "graphical designer", "technical implementer", "content provider", "technical validator", "educational validator", "script writer", "instructional designer", "subject matter expert"</td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>entity</sub></td>
		<td><sub>Name of contributor (person/organisation)</sub></td>
		<td></td>
		<td><sub>vCard, as defined by IMC vCard 3.0 (RFC 2425, RFC 2426)</sub></td>
		<td><sub>CharacterString</sub></td>
		<td><sub>BEGIN:VCARD
			VERSION:3.0
			N:Smith;Mary;
			FN:Mary Smith
			ORG:CAREO
			END:VCARD</sub></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>date</sub></td>
		<td><sub>Timestamp/date of contribution</sub></td>
		<td></td>
		<td></td>
		<td><sub>DateTime</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><b>Meta-Metadata</b></td>
		<td><b>Metadata to describe the task specific Metadata record</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>mandatory</b></td>
	</tr>
	<tr>
		<td><sub><b>identifier</b></sub></td>
		<td><sub>globaly unique identifier for the metadata record</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>catalog</sub></td>
		<td><sub>Name/type of identifier used</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>entry</sub></td>
		<td><sub>The identifier value</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>contribute</b></sub></td>
		<td><sub>entities who affected the state of the metadata record</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>role</sub></td>
		<td><sub>Role of contributor (i.e. creator)</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>entity</sub></td>
		<td><sub>Name of contributor</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>date</sub></td>
		<td><sub>Timestamp/date of contribution</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>metadataschema</b></sub></td>
		<td><sub>Name and version of the Metadata specification used to create metadata record (IMS MD 1.3.2, ProFormA MD 1.0) [system-generated or user-selectable]</sub></td>
		<td></td>
		<td><sub>"IMS MD 1.3.2", "ProFormA MD 1.0" (erforderlich)</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>language</b></sub></td>
		<td><sub>default natural language used for metadata entries in metadata instance (default language for all LangString values)</sub></td>
		<td></td>
		<td><sub>see general -> language</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><b>Technical</b></td>
		<td><b>Technical requirements and other technical aspects regarding the task</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>mandatory</b></td>
	</tr>
	<tr>
		<td><sub><b>size</b></sub></td>
		<td><sub>Filesize of learning object in bytes</sub></td>
		<td></td>
		<td><sub>sequence of digits between 0 and 9</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>requirement</b></sub></td>
		<td><sub>Technical requirements in order to use the task (only operating system and browser)</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>orComposite</sub></td>
		<td><sub></sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub></sub></td>
	</tr>
	<tr>
		<td><sub><sub>type</sub></sub></td>
		<td><sub></sub></td>
		<td></td>
		<td><sub>"operating system", "browser"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub></sub></td>
	</tr>
	<tr>
		<td><sub><sub>name</sub></sub></td>
		<td><sub></sub></td>
		<td></td>
		<td><sub><b>operating system:</b> "pc-dos", "ms-windows", "macos", "unix", "multi-os", "none"; <b>browser:</b> "any", "netscape communicator", "ms-internet explorer", "opera", "amaya"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub></sub></td>
	</tr>
	<tr>
		<td><sub><sub>minimumVersion</sub></sub></td>
		<td><sub></sub></td>
		<td></td>
		<td></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub></sub></td>
	</tr>
	<tr>
		<td><sub><sub>maximumVersion</sub></sub></td>
		<td><sub></sub></td>
		<td></td>
		<td></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub></sub></td>
	</tr>
	<tr>
		<td><sub><b>installationRemarks</b></sub></td>
		<td><sub>Installation instructions</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>location</b></sub></td>
		<td><sub>URL to access/download the learning object (where is the learning object physically located?)</sub></td>
		<td></td>
		<td></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>format</b></sub></td>
		<td><sub>Datatype(s) of the learning object</sub></td>
		<td></td>
		<td><sub>MIME types based on IANA registration (see RFC2048:1996)</sub></td>
		<td><sub>CharacterString</sub></td>
		<td><sub>"text/xml"</sub></td>
		<td><sub>optional</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>otherPlattformRequirements</b></sub></td>
		<td><sub>Additional technical plattform requirements (i.e. technical requirements for the automatic grading process)</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>duration</b></sub></td>
		<td><sub>duration of the learning object (i.e. for video files)</sub></td>
		<td></td>
		<td></td>
		<td><sub>Duration</sub></td>
		<td><sub>see Educational -> typicalLearningTime</sub></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><b>Educational</b></td>
		<td><b>Educational/Teaching related aspects of the tasks</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>recommended</b></td>
		<td><b>mandatory</b></td>
	</tr>
	<tr>
		<td><sub><b>learningResource</b></sub></td>
		<td><sub>Type of the learning object/task</sub></td>
		<td></td>
		<td><sub></sub></td>
		<td><sub></sub></td>
		<td></td>
		<td><sub>recommended</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>intendedEndUserRole</b></sub></td>
		<td><sub>Target group for the task</sub></td>
		<td></td>
		<td><sub>"teacher", "author", "learner", "manager"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>optional?</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>context</b></sub></td>
		<td><sub>Learning context/-environment/-situation for the task</sub></td>
		<td></td>
		<td><sub>"school", "higher education", "training", "other"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>recommended<sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>difficulty</b></sub></td>
		<td><sub>Difficulty level of task</sub></td>
		<td></td>
		<td><sub>"very easy", "easy", "medium", "difficult", "very difficult"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>typicalLearningTime</b></sub></td>
		<td><sub>period of time in which the task has to be solved</sub></td>
		<td></td>
		<td><sub>Value in ISO 8601:2000 standard</sub></td>
		<td><sub>Duration</sub></td>
		<td><sub>PT1W (1 Week)</sub></td>
		<td><sub>optional<sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>description</b></sub></td>
		<td><sub>Internal description i.e. with hints for manual grading or instructions how to use the task for teaching purposes</sub></td>
		<td><sub>task element <b>internal-description</b></sub></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>interactivity</b></sub></td>
		<td><sub>User input needed?, only for information?</sub></td>
		<td></td>
		<td><sub>"active" (Assessment, Drill and practice), "expositive" (Guide, Glossary, Information resource), "mixed" (LO with active and expositive components)</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>interactivityLevel</b></sub></td>
		<td><sub>Level/Degree of interactivity necessary to work on the task</sub></td>
		<td></td>
		<td><sub>"very low", "low", "medium", "high", "very high"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>semanticDensity</b></sub></td>
		<td><sub>Degree of conciseness of the learning object (i.e. relation between amount of information privided and size, span or duration of learning object)</sub></td>
		<td></td>
		<td><sub>"very low", "low", "medium", "high", "very high"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>typicalAgeRange</b></sub></td>
		<td><sub>Recommended age of target group</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>language</b></sub></td>
		<td><sub>Natural language used by the typical intended user of the task</sub></td>
		<td></td>
		<td><sub>see general -> language</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><b>Rights</b></td>
		<td><b>Rightsmanagement for the task (intellectual propertiy rights, conditions of use)</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>recommended</b></td>
	</tr>
	<tr>
		<td><sub><b>copyrightAndOtherRestrictions</b></sub></td>
		<td><sub>Copyright information available?</sub></td>
		<td></td>
		<td><sub>"yes", "no"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>description</b></sub></td>
		<td><sub>Specification of copyright information (i.e. applicable licenses)</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>mandatory, if coyrightAndOtherRestrictions = yes, n/a otherwise</sub></td>
		<td><sub>mandatory, if coyrightAndOtherRestrictions = yes</sub></td>
	</tr>
	<tr>
		<td><sub><b>cost</b></sub></td>
		<td><sub>Payment required in order to be allowed to use the learning object?</sub></td>
		<td></td>
		<td><sub>"no"</sub></td>
		<td><sub>VocabularyTerm (LOMv1.0)</sub></td>
		<td></td>
		<td><sub>mandatory (set to "no" automatically)</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><b>Relation</b></td>
		<td><b>Relation of the task to other ones</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>optional</b></td>
	</tr>
	<tr>
		<td><sub><b>kind</b></sub></td>
		<td><sub>Kind of relation between the tasks</sub></td>
		<td></td>
		<td><sub>"ispartof", "haspart", "isversionof", "hasversion", "isformatof", "hasformat", "references", "isreferencedby", "isbasedon", "isbasisfor", "requires", "isrequiredby", "haspreview", "ispreviewof", "istranslationof", "hastranslation", "hasmetadata"</sub></td>
		<td><sub>VocabularyTerm (LREv3.0) (based on Dublin Core)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>resource</b><sub></td>
		<td><sub>Information about the referenced task</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>identifier</sub></td>
		<td><sub>globaly unique identifier of the referenced task</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub><sub>catalog<sub></sub></td>
		<td><sub>Name/type of identifier used</sub></td>
		<td></td>
		<td><sub>"UUID"</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub><sub>entry</sub></sub></td>
		<td><sub>UUID of referenced Task</sub></td>
		<td><sub>task attribute <b>parent-uuid</b></sub></td>
		<td><sub>any uuid</sub></td>
		<td><sub>CharacterString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Short description of parent task</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><b>Annotation</b></td>
		<td><b>Feedback for the task/task rating</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>optional</b></td>
	</tr>
	<tr>
		<td><sub><b>entity</b></sub></td>
		<td><sub>Creator of task feedback/rating </sub></td>
		<td></td>
		<td><sub>vCard, as defined by IMC vCard 3.0 (RFC 2425, RFC 2426)</sub></td>
		<td><sub>CharacterString</sub></td>
		<td><sub>see Life Cycle -> contribute -> entity</sub></td>
		<td><sub>optional (anonymous feedback/rating allowed)</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub><b>date</b></sub></td>
		<td><sub>Creating date of feedback</sub></td>
		<td></td>
		<td></td>
		<td><sub>DateTime</sub></td>
		<td><sub>"2003-13-03"</sub></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub><b>description</b></sub></td>
		<td><sub>Feedback content (text or rating)</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><b>Classification</b></td>
		<td><b>Information to classify the task</b></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>mandatory</b></td>
	</tr>
	<tr>
		<td><sub><b>purpose</b></sub></td>
		<td><sub>Kind of classification</sub></td>
		<td></td>
		<td><sub>"educational objective", "programming language", "programming language version", "covered programming principles/concepts", "statistics.average_working_time", "statistics.users", "statistics.full_score_users", "statistics.normalized_average_score"</sub></td>
		<td><sub>VocabularyTerm (ProformA MD 1.0)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub><b>description</b></sub></td>
		<td><sub>Specification/description of the classification</sub></td>
		<td><sub>if purpose = programming language: task element <b>proglang</b><br><br> if purpose = programming language version: task element proglang attribute <b>version</b><br><br> if purpose = "statistics.*" = statistical information formatted as Integer (<b>"statistics.users"</b> and <b>"statistics.full_score_users"</b>), Float (<b>"statistics.normalized_average_score"</b>) or Duration (<b>"statistics.average_working_time"</b>)<sub></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>taxonPath</b></sub></td>
		<td><sub>Categories and sub categories to which the task belongs to (i.e. printed materials -> books -> picture books)</sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub><b>keyword</b></sub></td>
		<td><sub>keyword description of learning objective relative to its stated purpose</sub></td>
		<td></td>
		<td></td>
		<td><sub>LangString</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>n/a</sub></td>
	</tr>
</table>
