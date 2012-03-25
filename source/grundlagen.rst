**********
Grundlagen
**********

Diese Kapitel beschäftigt sich mit Grundlagen, die für das Verständnis der Modellierungsaspekte dieser Arbeit nötig sind. 
Zuerst wird eine kurze Einführung in die Beschreibung von Prozessmodellen durch Sprachen gegeben. 
Darauf aufbauend wird das in dieser Arbeit verwendete Konzept der perspektivenorientierten Prozessmodellierung und das Typ-Verwendungs-Konzept gezeigt.
Schließlich wird kurz die Metamodellierung, insbesondere das Linguistic Meta Model vorgestellt, welches die Basis für die Anpassbarkeit des hier entwickelten Modellierungswerkzeugs darstellt.

Prozessmodellierungssprachen
============================

Prozessmodellierung hat die Aufgabe, komplexe Abläufe in einer abstrahierten, das heißt vereinfachten, aber dennoch korrekten Form darzustellen.
Oft werden solche Modelle erstellt, um Zusammenhänge besser zu erkennen und Optimierungsmöglichkeiten aufzuzeigen.
Andererseits können abstrakt modellierte Prozesse auch von einem Softwaresystem automatisch ausgeführt beziehungsweise simuliert werden.

Um Modelle formulieren zu können bedarf es einer passenden Modellierungssprache. 

Zu einer Sprache gehört eine abstrakte Syntax, die allgemein Elemente einer Sprache und deren Beziehungen beschreibt.
Erst die konkrete Syntax legt sozusagen das "Aussehen" der Sprache fest. 
Prinzipiell lassen sich textuelle und grafische Notationen unterscheiden. 

Visuelle Sprachen und deren Klassifikation
------------------------------------------

Wohl die leistungsfähigste "Schnittstelle" des Menschen ist das visuelle System (ref), welches auf die Erkennung von Mustern und Strukturen ausgelegt ist.
So eignen sich besonders grafische Darstellungen dafür, einen Überblick über komplexe Modelle zu geben und Zusammenhänge zwischen einzelnen Modellelementen aufzuzeigen.

Die konkrete Syntax einer grafischen (oder "visuelle") Sprache umfasst eine Ansammlung von grafischen Objekten (auch "Formen" oder "Figuren" genannt), die Sprachelemente repräsentieren.
Elemente können auf verschiedene Arten miteinander in Beziehung gesetzt werden. 
Grafische Sprachen lassen sich nach diesem Kriterium in zwei gegensätzliche Klassen und eine Mischform einteilen (ref).

So können visuelle Sprachen einem graphbasierten Ansatz folgen.
Wie aus der Graphentheorie bekannt, bestehen Graphen aus Knoten und Kanten. Kanten drücken dabei eine Relation zwischen bestimmten Knoten aus, mit denen sie "verbunden" sind.
Dieses Prinzip lässt sich grafisch einfach darstellen, wie in :num:`Abbildung #ipm3d` an einem Beispiel aus dem im Rahmen dieser Arbeit entwickelten Modellierungswerkzeugs gezeigt wird.
Für die Bedeutung ist das "Verbundensein" über die Kante und nicht die Positionierung im Raum entscheidend.

Im Gegensatz dazu steht eine geometriebasierte Darstellung, welche die relative Positionierung von Formen für die Darstellung von Beziehungen nutzt.
So kann, wie in :num:`Abbildung #ipml` ein Objekt an einem anderen anhaften, oder in diesem enthalten sein.

Aus den beiden Ansätzen können Mischformen ("Hybride") gebildet werden, die so eine größere Auswahl an Möglichkeiten zur Visualisierung von Beziehungen bieten können.
In der Praxis sind daher solche Ansätze in der UML zu sehen und auch in der Prozessmodellierung zu finden, wie an den Beispielen in den folgenden Abschnitten zu sehen ist.

.. _popm:

Perspektivenorientierte Prozessmodellierung
-------------------------------------------

Die verschiedenen "Bestandteile" eines Prozessmodells werden von (ref) in "Perspektiven" (oder auch Aspekte genannt) eingeteilt. Identifiziert wurden folgende fünf Perspektiven:

Funktionale Perspektive 
    Diese umfasst die funktionalen Einheiten, allgemein "Prozess" genannt. Weiterhin sind hier auch Entscheidungsknoten, Konnektoren eingeschlossen.

Verhaltensorientierte Perspektive 
    Dies wird auch als "Kontrollfluss" bezeichnet und gibt die zeitlichen bzw. logischen Abhängigkeiten zwischen Elementen der funktionalen Perspektive an. Durch diese Perspektive wird also die Ausführungsreihenfolge festgelegt. 

Organisationale Perspektive 
    Einem Prozess lässt sich eine ausführende Entität, beispielsweise eine abstrakte Rolle oder eine konkrete Person zuordnen, die für die Ausführung verantwortlich ist.

Datenbezogene Perspektive 
    Prozesse sind ohne Daten, die im Ablauf erstellt, modifiziert und ausgetauscht werden quasi undenkbar. Datenflüsse legen oft auch die Abhängigkeiten zwischen Prozessen fest.

Operationale Perspektive 
    Zur Ausführung von Prozessen sind verschiedene Betriebsmittel wie Maschinen, Werkzeuge oder Rechnerressourcen erforderlich, welche in dieser Perspektive abgebildet werden.

Dies soll explizit keine vollständige Aufzählung, sondern nur eine Zusammenfassung sehr häufig vorkommender relevanter Aspekte sein. 
Das bedingt eine flexible Modellierungssprache. So können nach Bedarf weitere Perspektiven hinzugefügt werden.
In diesem Zusammenhang soll auch erwähnt werden, dass es oft notwendig ist, die Granularität von Prozessen dynamisch zu verändern, je nachdem, welche Informationen im konkreten Fall gefragt sind. 

Prozesse können daher komposit (oder auch komplex genannt) sein und weitere, Subprozesse enthalten, die in einem grobgranularen Diagramm nicht darstellt werden. B

ei Bedarf können diese Unterprozesse separat in einem Diagramm betrachten werden.

Die POPM wird vom Modellierungswerkzeug i>ProcessManager (kurz: i>PM) implementiert. 

:num:`Abbildung #ipm-typ-verwendung-1` zeigt einen Prozess in i>PM. 
Die funktionale Perspektive wird hier durch drei Prozesse sowie einen Entscheidungsknoten vertreten. 
Kontrollflüsse, die mit grauen Pfeilen visualisiert werden bilden die verhaltensorientierte Perspektive.
Am Entscheidungsknoten kann sich der Kontrollfluss je nach Ausgang des Kriteriums (Einschreiben / Paket?) verzweigen.
Mit dem mittleren Prozess (blau eingekreist) sind Daten assoziiert, die in einem an den Prozess angehängten Quadrat benannt werden.

Die drei bisher genannten Perspektiven werden, wie zu sehen ist, nach einem graphbasiertem Ansatz visualisiert. 
Im Gegensatz dazu werden durch an die Prozesse "angeklebte" Zeichenketten die organisationale (unten) und operationale (oben) Perspektive visualisiert.

.. _ipm-process:

.. figure:: _static/ext_pics/ipm-process.png
    :height: 8cm

    Prozess in i>PM aus :cite:`roth`

BPMN
----

Für die Modellierung von Prozessen wird häufig BPMN, eine standardisierte, visuelle Sprache genutzt. 
:num:`Abbildung #bpmn-process` zeigt einen in BPMN modellierten Prozess.
An diesem Beispiel lassen sich grundlegende Elemente von Prozessmodellen erkennen.

So besteht ein Modell aus Aktitvitäten (auch "Prozess" genannt), welche über Kanten verbunden sind, die einen Kontrollfluss, also eine Abhängigkeit darstellen.
Wie zu sehen ist, handelt es sich dabei um einen graphbasierten Ansatz.

BPMN definiert allerdings auch geometriebasierte Beziehungen. Als Beispiel ist in :num:`Abbildung #bpmn-swimlane` eine "Swimlane" gezeigt.
So werden zusammengehörige Prozessschritte, die von einer bestimmten Entität ausgeführt werden in einer solchen Lane gruppiert.

BPMN ist im Standard als eine Ansammlung von zweidimensionalen Formen definiert. Später wird eine dreidimensionale Adaption gezeigt.


Modellierungswerkzeuge
----------------------

Wie gesagt, eignen sich grafische Darstellung besonders für die Interpretation durch Menschen. 
Prinzipiell lassen sich solche Modelle einfach mit Hilfe von 2D-Zeichenwerkzeugen wie beispielsweise Dia oder MS Visio erstellen.
Solche Programme können schon passende Formen und Verbindungen, beispielsweise nach dem BPMN-Standard anbieten. 

Ein Benutzer macht die Bedeutung eines solchen Diagrammes an den erkennbaren grafischen Formen fest.

Durch ein Zeichenprogramm wird das Diagramm intern nur als eine "Ansammlung" von Bildpunkten oder geometrischen Primitiven dargestellt und auch entsprechend persistiert ("gespeichert").
Für ein solches Programm hat die Semantik des Modells keinerlei Bedeutung. 
Dies ist ein Problem, wenn der modellierte Prozess automatisch ausgeführt oder verändert werden soll.

Daher wären eher Werkzeuge sinnvoll, die auch intern eine "Vorstellung" von Modellierungskonzepten haben. 
Solche Werkzeuge werden oft – auch in dieser Arbeit – Modellierungswerkzeuge genannt.

Ein solches Werkzeug bietet die Möglichkeit, Modelle zu erstellen, diese in sinnvoller Form zu persistieren und wieder aus einer physikalischen Repräsentation zu laden. 
Dem Benutzer wird überlicherweise eine Palette an Modellelementen angeboten, die in einem konkreten Prozessmodell eingesetzt werden können. 
Ein Anwender "baut" ein Modell, indem er grafische Objekte miteinander auf einer "Zeichenfläche" kombiniert.

Solche Werkzeuge gibt es für Sprachen wie BPMN, wie beispielsweise ARIS oder ?.
Als physische Repräsentation von Modellen ist es besonders praktisch, wenn diese in einem nicht-proprietärem Format verfügbar ist. 
Damit ist es möglich, solche Modelle mit verschiedenen Werkzeugen zu nutzen. 
Für BPMN ist beispielsweise XPDF als (XML-)Austauschformat verbreitet. Ein solches Format lässt sich auch als textuelle Darstellung eines (Prozess-)Modells bezeichnen.
Textuelle Darstellungen sind für die automatische Verarbeitung gut geeignet, können aber durchaus auch von Menschen gelesen und – mit Einschränkungen – bearbeitet werden.
Dies wird im nächsten Abschnitt noch deutlicher.

.. _metamodellierung:

Metamodellierung
================

Die schon erwähnte, nötige Flexibilität von Prozessmodellen erfordert oft, dass die Modellierungssprache selbst verändert werden kann. 
Dadurch wird damit die Möglichkeit geschaffen, die Sprache an spezielle Bedürfnisse anzupassen. 
So lassen sich sogenannte domänenspezifische Sprachen (DSL) erstellen, die gegenüber fest vorgegebenen Sprachen den Vorteil besitzen, Sachverhalte in einer konkreten Domäne besser, also verständlicher und direkter darstellen zu können. (Volz und noch ein paar andere) 

Standardisierte Sprachen, wie BPMN definieren zahlreiche Elemente. Die Auswahl an Elementen ist dabei abgeschlossen, es können nicht einfach weitere Typen hinzugefügt werden.
Andererseits kann es auch sinnvoll sein, die verfügbaren Elemente für einen Anwendungsfall zu reduzieren.

Wie schon tt angedeutet wurde sind für Prozessmodelle eine Vielzahl von verschiedenen Entitäten und Beziehungstypen nötig.

Zur Beschreibung von (domänenspezifischen) Sprachen lässt sich das Konzept der "Metamodellierung" einsetzen.
Ein Metamodell stellt sozusagen ein Modell für eine Klasse von Modelle dar.

Zitat?!

Durch die Anpassung eines Metamodells lässt sich die abstrakte und konkrete Syntax einer Sprache verändern. 
So können neue Modellelemente hinzugefügt und bestehende angepasst oder entfernt werden. 
Im Falle einer visuellen Sprache lässt sich die konkrete Repräsentation von Modellelementen, also deren Aussehen und Form ändern.

Um Metamodelle zu "erstellen" ist es notwendig, diese auf eine bestimmte Weise beschreiben zu können. 
Dies leistet das im Folgenden vorgestellte Linguistic Meta Model (LMM), welches im Rahmen der Open Meta Modelling Environment (OMME), einer Metamodellierungsumgebung, entstanden ist. :cite:`volz_werkzeugunterstutzung_2011`

.. _lmm:

Linguistic Meta Model
---------------------

LMM stellt eine Sprache bereit, welche zur Definition von Metamodellen dient. 
:num:`Abbildung #lmm-model` zeigt die grundlegenden LMM-Elemente und deren Hierarchie.

.. _lmm-model:

.. figure:: _static/ext_pics/bernhard-lmmmodel.eps
    :height: 6cm

    Hierarchie der LMM-Elemente aus :cite:`volz_werkzeugunterstutzung_2011`


Das zentrale Element im LMM ist das "Concept". 
Ein Concept kombiniert Eigenschaften einer Klasse und eines Objekts, wie sie aus objektorientierten Programmiersprachen bekannt sind.
So kann ein Concept – wie eine Klasse – Attribute definieren. Gleichzeitig kann ein Concept – wie ein Objekt –  Wertzuweisungen enthalten.
Anders ausgedrückt können Concepts sowohl eine "Typ-Facette", welche Attribute definiert als auch eine "Instanz-Facette", welche Zuweisungen vornimmt, beinhalten.

Ein Vergleich zwischen Klasse-Objekt-Beziehungen und Concept-Concept-Beziehungen  ist in :num:`Abbildung #vergleich-lmm` zu sehen.

.. _lmm-model:

.. figure:: _static/ext_pics/vergleich_lmm.eps
    :height: 8cm

    Vergleich von objektorientierter Modellierung (links) und Metamodellierung mit Clabjects


Im objektorientierten System stellen Klassen Typen dar, Objekte sind Instanzen von Klassen, welche Werte an die Attribute der Klasse zuweisen.

Im Gegensatz zu der von Klasse und Objekt vorgegebenen Hierarchie aus 2 "Ebenen" lassen sich mit Concepts Hierarchien mit beliebig vielen Ebenen darstellen. 
Concepts können gleichzeitig den Typ für Concepts auf der darunterliegenden Ebene und eine Instanz eines Concepts (``instanceOf``) auf der nächsthöheren Ebene darstellen.
Ebenso gibt es die Möglichkeit für Concepts, andere Concepts analog zu Klassen zu "erweitern" (``extends``), also einen Subtyp zu bilden. 

In der Abbildung besitzt ``ConceptC`` eine Instanz-Facette, welche den Attributen aus ``ConceptA`` und ``ConceptB`` Werte zuweist.
Die Typ-Facette von ``ConceptC`` stellt das Attribut ``c`` bereit welches von ``ConceptD`` mit dem Wert 5.5 belegt wird.

Concepts werden wie in :num:`Abbildung #lmm-model` gezeigt in "Packages" eingeordnet. Packages bilden zusammen einen Level, welcher eine Ebene in der Metamodellierungshierarchie repräsentiert.
Levels stellen zusammen das vollständige "Model" dar.

Levels können ebenfalls zueinander in einer Instanzbeziehung (``instanceOf``) stehen. 
Ein Level *MA* ist die Instanz eines anderen Levels *MB*, wenn alle in *MA* definierten Concepts Instanzen von Concepts in *MB* sind.

Neben der schon erwähnten Instanziierung und Subtypbildung werden von LMM zusätzliche Modellierungsmuster unterstützt. 
Von diesen ist für die vorliegende Arbeit die sog. "Spezialisierung von Instanzen"  bedeutend, deren Vorteile für die Modellierung von :cite:`volz_werkzeugunterstutzung_2011` beschrieben werden.

Dieses Muster wird in :num:`Abbildung #concreteuseof` veranschaulicht.

.. _concreteuseof:

.. figure:: _static/ext_pics/concreteuseof.eps
    :height: 8cm

    Instanz-Spezialisierung ausgehend von ConceptD

In der Abbildung spezialisiert ``UseA`` ``ConceptD`` (``concreteUseOf``). ``UseA`` übernimmt dabei alle Zuweisungen von ``ConceptD``, damit hat das Attribut in ``UseA`` ebenfalls den Wert 5.5.
``UseB`` dagegen setzt wiederum einen Wert für das Attribut ``c``. Das heißt, dass in ``UseB`` die bisherige Zuweisung "überschrieben" wird und damit den Wert 0 hat.
Für ``ConceptD`` ändert sich dabei nichts; die Überschreibung wirkt sich nur in ``UseB`` aus.

In LMM lässt sich für Attribute festlegen, inwieweit das Überschreiben von Werten zulässig ist und welche Bedeutung dies hat. 
Für die vorliegende Arbeit wird aber immer angenommen, dass Werte überschrieben werden dürfen.

Die textuelle Darstellung von LMM-Modellen erfolgt mit der Sprache Linguistic Meta Language (LML) :cite:`volz_werkzeugunterstutzung_2011`, deren Syntax an bekannte Programmiersprachen wie C++ oder C# angelehnt ist.
Hier ist ein Beispiel für ein einfaches Modell in textueller Form zu sehen:

MDF
---

Ebenfalls als Teil der Metamodellierungsumgebung OMME ist das Model Designer Framework (MDF) :cite:`roth` entstanden.

:num:`Abbildung #ipm-typ-verwendung-1` und :num:`Abbildung #ipm-typ-verwendung-2` zeigen Prozesse, die in einem mit MDF definierten Editor ("i>PM2") für die :ref:`POPM <popm>` erstellt wurden. 
Die Visualisierung ist ähnlich zu dem vorher vorgestellten i>PM, jedoch werden hier operationale und organisationale Perspektive durch geometrisches "Enthaltensein" im Prozess dargestellt.

An den Abbildungen lässt sich ein weiteres wichtiges Konzept – das "Typ-Verwendungs-Konzept" – welches von diesem Werkzeug umgesetzt wird zeigen. 
Dieses Konzept wird auch in der vorliegenden Arbeit für die Modellierung von Prozessen genutzt.


.. _ipm-typ-verwendung-1:

.. figure:: _static/ext_pics/ipm2-process-verwendung_1.png
    :height: 10cm

    Prozess in i>PM2 aus :cite:`volz_werkzeugunterstutzung_2011`


.. _ipm-typ-verwendung-2:

.. figure:: _static/ext_pics/ipm2-typ-verwendung_2.png
    :height: 10cm

    Prozess mit angepasster Verwendung aus :cite:`volz_werkzeugunterstutzung_2011`

Durch das mit :ref:`LMM <lmm>` eingeführte Modellierungsmuster der **Instanz-Spezialisierung** lässt sich das Typ-Verwendungs-Konzept leicht realisieren.

Nach der Terminilogie des Typ-Verwendungs-Konzepts ist in :num:`Abbildung #concreteuseof` ``ConceptD`` ein "Typ", ``UseA`` und ``UseB`` sind "Verwendungen" davon.


