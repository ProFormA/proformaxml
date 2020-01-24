<table>
	<tr style="font-size: 19px">
		<th>Element</th>
    	<th>Beschreibung (im ProFormA Kontext)</th>
    	<th>Nutzung</th>
	</tr>
    
	<tr style="font-size: 17px; font-weight: bold">
        <td>General</td>
		<td>Allgemeine Angaben</td>
		<td>mandatory</td>
    </tr>
	<tr style="font-size: 14px">
		<td>identifier</td>
		<td>UUID der Aufgabe</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>title</td>
		<td>Aufgabentitel</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>language</td>
		<td>natürliche (Haupt-)sprache der Aufgabe</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>description</td>
		<td>Kurzbeschreibung/Teaser</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>keywords</td>
		<td>Schlagwörter für die Suche</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>coverage</td>
		<td>Abgedeckte Programmierprinzipien / Programmiersprache + Version</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>structure</td>
		<td>Strukturierung des Learning Objects (z. B. atomic, hierarchical, collection [z. B. zip File])</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>aggregation level</td>
		<td>Inwieweit lässt sich das Learning Object in weitere kleinere Learning Objects aufteilen?</td>
		<td>n/a</td>
	</tr>
	
	<tr style="font-size: 17px; font-weight: bold">
        <td>Life Cycle</td>
		<td>Lebenszyklus (Änderungen, aktueller Status)</td>
		<td>mandatory</td>
    </tr>
	<tr style="font-size: 14px">
		<td>version</td>
		<td>Aufgabenversion</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>status</td>
		<td>Release Status (z. B. draft, final)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px; font-weight: bold">
		<td>contribute</td>
		<td></td>
		<td></td>
	</tr>
	<tr style="font-size: 14px">
		<td>role</td>
		<td>Rolle des Contributors (z. B. Aufgabenautor)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>entity</td>
		<td>Name des Contributors (Person/Organisation)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>date</td>
		<td>Zeitstempel der Contribution</td>
		<td>mandatory</td>
	</tr>
	
	<tr style="font-size: 17px; font-weight: bold">
        <td>Technical</td>
		<td>Technische Aspekte</td>
		<td>optional</td>
    </tr>
	<tr style="font-size: 14px">
		<td>format</td>
		<td>Datentyp(en) des Learning Objects</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>size</td>
		<td>Filesize des Learning Objects in bytes</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>location</td>
		<td>URL für den Zugriff auf das Learning Object</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>requirement</td>
		<td>Technische Voraussetzungen zur Nutzung der Aufgabe (z. B. technische Voraussetzungen für automatische Bewertung)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>installationRemarks</td>
		<td>Installationsanweisungen</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>otherPlattformRequirements</td>
		<td>Weitere technische Voraussetzungen, die nicht bereits durch die anderen Elemente angegeben werden konnten/abgedecktz werden</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>duration</td>
		<td>Dauer für das Abspielen des Learning Objects (z. B. für Videos)</td>
		<td>n/a</td>
	</tr>
	
	<tr style="font-size: 17px; font-weight: bold">
        <td>Rights</td>
		<td>Rechtemanagement</td>
		<td>mandatory</td>
    </tr>
	<tr style="font-size: 14px">
		<td>cost</td>
		<td>Kosten für Download/Nutzung der Aufgabe</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>copyrightAndOtherRestrictions</td>
		<td>Restriktionen/Copyright vorhanden (yes/no)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>description</td>
		<td>Beschreibung Restriktionen/Angaben zum Copyright</td>
		<td>mandatory (wenn coyrightAndOtherRestrictions = yes)</td>
	</tr>

	<tr style="font-size: 17px; font-weight: bold">
        <td>Relation</td>
		<td>Beziehung zu anderen Learning Objects</td>
		<td>optional</td>
    </tr>
	<tr style="font-size: 14px; font-weight: bold">
		<td>ressource</td>
		<td></td>
		<td></td>
	</tr>
	<tr style="font-size: 14px">
		<td>identifier</td>
		<td>Parent UUID</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>description</td>
		<td>Beschreibung der Ressource</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>kind</td>
		<td>Art der Beziehung zwischen den Learning Objects</td>
		<td>mandatory</td>
	</tr>
	
	<tr style="font-size: 17px; font-weight: bold">
        <td>Annotation</td>
		<td>Feedback zur Aufgabe</td>
		<td>optional</td>
    </tr>
	<tr style="font-size: 14px">
		<td>entity</td>
		<td>Wer hat das Feedback zur Aufgabe erstellt?</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>date</td>
		<td>Wann wurde das Feedback erstellt?</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>description</td>
		<td>Feedback zur Aufgabe</td>
		<td>mandatory</td>
	</tr>

	<tr style="font-size: 17px; font-weight: bold">
        <td>Classification</td>
		<td>Aufgabe Klassifizieren</td>
		<td>optional</td>
    </tr>
	<tr style="font-size: 14px">
		<td>purpose</td>
		<td>Art der Klassifizierung (für Lernziel: educational objective)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>taxonPath</td>
		<td>Kategorien zu dem das Learning Object gehört (z. B. printed materials -> books -> picture books)</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>description</td>
		<td>Genaue Beschreibung der Lernziele / des Aufgabenzwecks</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>keyword</td>
		<td></td>
		<td>n/a</td>
	</tr>

	<tr style="font-size: 17px; font-weight: bold">
        <td>Educational</td>
		<td>Lehrbezogene Angaben</td>
		<td>mandatory</td>
    </tr>
	<tr style="font-size: 14px">
		<td>interactivity</td>
		<td>User Input benötigt oder nur zur Information oder beides?</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>learningRessource</td>
		<td>Typ des Learning Objects (z. B. Assessment, Übung...)</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>interactivityLevel</td>
		<td>Grad der Interaktivität, der zur Bearbeitung des Learning Objects notwendig ist</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>semanticDensity</td>
		<td></td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>intendedEndUserRole</td>
		<td>Zielgruppe</td>
		<td>mandatory</td>
	</tr>
	<tr style="font-size: 14px">
		<td>context</td>
		<td>Lernkontext / Lernumgebung, -situation</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>typicalAgeRange</td>
		<td>Empfohlenes Alter der Zielgruppe</td>
		<td>n/a</td>
	</tr>
	<tr style="font-size: 14px">
		<td>difficulty</td>
		<td>Schwierigkeitsgrad der Aufgabe</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>typicalLearningTime</td>
		<td>Zeitlicher Aufwand zur Bearbeitung der Aufgabe</td>
		<td>optional</td>
	</tr>
	<tr style="font-size: 14px">
		<td>description</td>
		<td>Interne Beschreibung mit Bewertungshinweisen für manuelle Bewertung / Aufgabentext / Instruktionen wie das Learning Object genutzt werden soll</td>
		<td>optional</td>
	</tr>
</table>