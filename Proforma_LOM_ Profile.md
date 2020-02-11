<table>
	<tr>
		<th>Element</th>
    	<th>Content (in context ProFormA)</th>
		<th>Referenced Element/Attribute in Task XML</th>
    	<th>Usage</th>
		<th>Usage (LOM-DE)</th>
	</tr>
	<tr>
        <td><b>General</b></td>
		<td><b>General information about the task</b></td>
		<td></td>		
		<td><b>mandatory</b></td>
		<td><b>mandatory</b></td>
    </tr>
	<tr>
		<td><sub>identifier</sub></td>
		<td><sub>UUID of the task</sub></td>
		<td><sub>task attribute <b>uuid</b></sub></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>title</sub></td>
		<td><sub>Title of the task</sub></td>
		<td><sub>task element <b>title</b></sub></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>language</sub></td>
		<td><sub>Natural main language used for task</sub></td>
		<td><sub>task attribute <b>lang<b></sub></td>		
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Short description/teaser of the Task</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>keywords</sub></td>
		<td><sub>Keywords describing the topic of the task (for search)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>coverage</sub></td>
		<td><sub>Time, culture or region to which the learning object applies</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>structure</sub></td>
		<td><sub>Underlying organizational structure of the learning object (i.e. atomic, collection [i.e. zip])</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>aggregation level</sub></td>
		<td><sub>Functional granularity of the learning object</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
        <td><b>Life Cycle</b></td>
		<td><b>Information about tasks release state, version and change history</td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>recommended</b></td>
    </tr>
	<tr>
		<td><sub>version</sub></td>
		<td><sub>Versionnumber of the task</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>status</sub></td>
		<td><sub>Release (i.e. draft, final)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>contribute</b></sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>role</sub></td>
		<td><sub>Role of contributor (i.e. author of the task)</sub></td>
	    <td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>entity</sub></td>
		<td><sub>Name of contributors (person/organisation)</sub></td>
	    <td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>date</sub></td>
		<td><sub>Timestamp of contribution</sub></td>
	    <td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	    <td><b>Meta-Metadata</b></td>
		<td><b>Metadata to describe the task specific Metadata record</b></td>
		<td></td>
		<td><b>n/a</b></td>
		<td><b>mandatory</b></td>
	<tr>
        <td><b>Technical</b></td>
		<td><b>Technical requirements and other technical aspects regarding the task</b></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>mandatory</b></td>
    </tr>
	<tr>
		<td><sub>size</sub></td>
		<td><sub>Filesize of learning object in bytes</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>requirement</sub></td>
		<td><sub>Technical requirements in order to use the task (i.e. technical requirements for the automatic grading process)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>installationRemarks</sub></td>
		<td><sub>Installation instructions</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>location</sub></td>
		<td><sub>URL to access/download the learning object</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>format</sub></td>
		<td><sub>Datatype(s) of the learning object</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>otherPlattformRequirements</sub></td>
		<td><sub>Additional technical plattform requirements</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>duration</sub></td>
		<td><sub>duration of the learning object (i.e. for video files)</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
        <td><b>Educational</b></td>
		<td><b>Educational/Teaching related aspects of the tasks</b></td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>mandatory</b></td>
    </tr>
	<tr>
		<td><sub>learningResource</sub></td>
		<td><sub>Type of the learning object/task (i.e. Assessment, exercise)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>intendedEndUserRole</sub></td>
		<td><sub>Target group for the task</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>context</sub></td>
		<td><sub>Learning context/-environment/-situation for the task</sub></td>
		<td></td>
		<td><sub>optional<sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>difficulty</sub></td>
		<td><sub>Difficulty level of task</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>typicalLearningTime</sub></td>
		<td><sub>Average time needed to solve the task</sub></td>
		<td></td>
		<td><sub>optional<sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Internal description i.e. with hints for manual grading or instructions how to use the task for teaching purposes</sub></td>
		<td><sub>task element <b>internal-description</sub></b></td>
		<td><sub>optional</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>interactivity</sub></td>
		<td><sub>User input needed?, only for information?</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>interactivityLevel</sub></td>
		<td><sub>Level of interactivity necessary to work on the task</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>semanticDensity</sub></td>
		<td><sub>Degree of conciseness of the learning object (i.e. relation between amount of information privided and size, span or duration of learning object)</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>typicalAgeRange</sub></td>
		<td><sub>Recommended age of target group</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
        <td><b>Rights</b></td>
		<td><b>Rightsmanagement for the task</b></td>
		<td></td>
		<td><b>mandatory</b></td>
		<td><b>recommended</b></td>
    </tr>
	<tr>
		<td><sub>copyrightAndOtherRestrictions</sub></td>
		<td><sub>Copyright information available (yes/no)?</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Specification of copyright information (i.e. applicable licenses)</sub></td>
		<td></td>
		<td><sub>mandatory, if coyrightAndOtherRestrictions = yes, n/a otherwise</sub></td>
		<td><sub>mandatory, if coyrightAndOtherRestrictions = yes</sub></td>
	</tr>
	<tr>
		<td><sub>cost</sub></td>
		<td><sub>Payment required in order to be allowed to use the learning object?</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
        <td><b>Relation</b></td>
		<td><b>Relation of the task to other ones</b></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>optional</b></td>
    </tr>
	<tr>
		<td><sub>kind</sub></td>
		<td><sub>Kind of relation between the tasks</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub><b>resource</b><sub></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>identifier</sub></td>
		<td><sub>Parent UUID</sub></td>
		<td><sub>task attribute <b>parent-uuid</b></sub></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Short description of parent task</sub></td>
		<td></td>
		<td><sub>optional</sub></td>
		<td></td>
	</tr>
	<tr>
        <td><b>Annotation</b></td>
		<td><b>Feedback for the task/task rating</b></td>
		<td></td>
		<td><b>optional</b></td>
		<td><b>optional</b></td>
    </tr>
	<tr>
		<td><sub>entity</sub></td>
		<td><sub>Creator of task feedback/rating </sub></td>
		<td></td>
		<td><sub>optional (anonymous feedback/rating allowed)</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>date</sub></td>
		<td><sub>Creating date of feedback</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Feedback content (text or rating)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td></td>
	</tr>
	<tr>
        <td><b>Classification</b></td>
		<td><b>Information to classify the task</b></td>
		<td></td>
		<td><b>mandatory (programming language + version)<br><br>optional (other classification purposes)</b></td>
		<td><b>mandatory</b></td>
    </tr>
	<tr>
		<td><sub>purpose</sub></td>
		<td><sub>Kind of classification (i.e. educational objective, programming language and version, covered programming principles/concepts)</sub></td>
		<td></td>
		<td><sub>mandatory</sub></td>
		<td><sub>mandatory</sub></td>
	</tr>
	<tr>
		<td><sub>description</sub></td>
		<td><sub>Specification/description of the classification</sub></td>
		<td><sub>if purpose = programming language and version: task element <b>prog-lang</b><sub></td>
		<td><sub>mandatory</sub></td>
		<td><sub>optional</sub></td>
	</tr>
	<tr>
		<td><sub>taxonPath</sub></td>
		<td><sub>Categories and sub categories to which the task belongs to (i.e. printed materials -> books -> picture books)</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>recommended</sub></td>
	</tr>
	<tr>
		<td><sub>keyword</sub></td>
		<td><sub>keyword description of learning objective relative to its stated purpose</sub></td>
		<td></td>
		<td><sub>n/a</sub></td>
		<td><sub>n/a</sub></td>
	</tr>
</table>