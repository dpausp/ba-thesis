**********
Grundlagen
**********

Diese Kapitel beschäftigt sich mit Grundlagen, die für das Verständnis der Modellierungsaspekte dieser Arbeit wichtig sind. 
Zuerst wird eine Einführung in die Beschreibung von (Prozess-)modellen durch Sprachen gegeben und das Konzept der perspektivenorientierten Prozessmodellierung :cite:`jabl` gezeigt. 
Schließlich wird die Metamodellierung, insbesondere das Linguistic Meta Model :cite:`volz_werkzeugunterstutzung_2011` vorgestellt, welches die Basis für die Anpassbarkeit des hier entwickelten Modellierungswerkzeugs darstellt.

Prozessmodellierungssprachen
============================

Modellierung hat im Rahmen des Prozessmanagements die Aufgabe, komplexe Abläufe aus der Realität in einer abstrahierten, das heißt vereinfachten, aber dennoch korrekten Form darzustellen. :cite:`?`
Oft werden solche Modelle erstellt, um Zusammenhänge besser zu erkennen und Optimierungsmöglichkeiten für den realen Prozess aufzuzeigen.
Andererseits können abstrakt modellierte Prozesse von einem Softwaresystem automatisch ausgeführt beziehungsweise simuliert werden. :cite:`survey`

Um Modelle formulieren zu können bedarf es einer passenden Modellierungssprache. 

Zu einer Sprache gehört eine *abstrakte Syntax*, die allgemein Elemente einer Sprache und deren Beziehungen beschreibt.
Erst eine *konkrete Syntax* legt sozusagen das "Aussehen" der Sprache fest. :cite:`Applied metamodelling`
Grunsätzlich lassen sich textuelle und grafische Notationen für Sprachen unterscheiden. 

Visuelle Sprachen und deren Klassifikation
------------------------------------------

Wohl die leistungsfähigste "Schnittstelle" des Menschen it das visuelle System :cite:`ware-per`, welches auf die Erkennung von Mustern und Strukturen ausgelegt ist.
Daher eignen sich besonders grafische Darstellungen dafür, einen Überblick über komplexe Modelle zu geben und Zusammenhänge zwischen einzelnen Modellelementen aufzuzeigen.
So spielen visuelle Sprachen auch eine große Rolle in der Prozessmodellierung. :cite:`survey` :cite:`jabl-vis`

Die konkrete Syntax einer grafischen (oder "visuelle") Sprache umfasst eine Ansammlung von grafischen Objekten (auch "Formen" oder "Figuren" genannt), die Sprachelemente repräsentieren.
Elemente können auf verschiedene Arten miteinander in Beziehung gesetzt werden. 
Grafische Sprachen lassen sich nach diesem Kriterium in zwei gegensätzliche Klassen und eine Mischform einteilen :cite:`costa`.

So können visuelle Sprachen einem graphbasierten Ansatz folgen.
Graphen bestehen aus Knoten und Kanten. Kanten drücken dabei eine Relation zwischen bestimmten Knoten aus, mit denen sie "verbunden" sind.
Dieses Prinzip lässt sich grafisch einfach darstellen, wie in :num:`Abbildung #ipm3d` an einem Beispiel aus dem im Rahmen dieser Arbeit entwickelten Modellierungswerkzeugs gezeigt wird.
Für die Bedeutung ist das "Verbundensein" über die Kante und nicht die Positionierung im Raum entscheidend.

Im Gegensatz dazu steht eine geometriebasierte Darstellung, welche die relative Positionierung von Formen für die Darstellung von Beziehungen nutzt.
So kann, wie in :num:`Abbildung #ipml` ein Objekt an einem anderen anhaften, oder in diesem enthalten sein.

Aus den beiden Ansätzen können Mischformen ("Hybride") gebildet werden, die so eine größere Auswahl an Möglichkeiten zur Visualisierung von Beziehungen bieten können.
In der Praxis sind daher solche Ansätze in der UML :cite:`booch` zu sehen und auch in der Prozessmodellierung zu finden, wie an den Beispielen in den folgenden Abschnitten zu sehen ist.

.. _popm:

Perspektivenorientierte Prozessmodellierung
-------------------------------------------

In einem Prozessmodell muss üblicherweise eine große Menge an Informationen dargestellt werden, die sich auf eine bestimmte Weise gruppieren lässt.
Nach dem Konzept der perspektivenorientierten Prozessmodellierung (kurz POPM) werden die "Informationsbestandteile" eines Prozesses in sog. "Perspektiven" (oder auch Aspekte genannt) eingeteilt. :cite:`jabl` 
Es wurden folgende fünf wichtigen Perspektiven identifiziert, die auch in :cite:`volz_werkzeugunterstutzung_2011` (S.251f) beschrieben werden:

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

Dies ist keine vollständige Aufzählung, sondern nur eine Zusammenfassung sehr häufig vorkommender Bestandteile. 
So kann es nötig sein, für einen Anwendungsfall weitere Perspektiven hinzuzufügen oder Perspektiven um neue Elemente zu erweitern.
Daraus ergibt sich, dass (grafische) Modellierungssprachen, die POPM unterstützen möglichst erweiterbar sein sollten. 

:num:`Abbildung #ipm-typ-verwendung-1` zeigt einen Prozess nach der perspektivenorientierten Prozessmodellierung.

Die funktionale Perspektive wird hier durch drei Prozesse sowie einen Entscheidungsknoten vertreten. 
Kontrollflüsse, die mit grauen Pfeilen visualisiert werden bilden die verhaltensorientierte Perspektive.
Am Entscheidungsknoten kann sich der Kontrollfluss je nach Ausgang des Kriteriums (Einschreiben / Paket?) verzweigen.
Mit dem mittleren Prozess (blau eingekreist) sind Daten assoziiert, die in einem an den Prozess angehängten Quadrat benannt werden.

Die drei bisher genannten Perspektiven werden, wie zu sehen ist, nach einem graphbasiertem Ansatz visualisiert. 
Im Gegensatz dazu werden durch an die Prozesse "angeklebte" Zeichenketten die organisationale (unten) und operationale (oben) Perspektive visualisiert.

.. _ipm-process:

.. figure:: _static/ext_pics/ipm-process.png
    :width: 16cm

    Perspektivenorientierte Prozessmodellierung aus :cite:`roth`


Grafische Modellierungswerkzeuge
--------------------------------

Für die Erstellung von grafischen Prozessmodellen am Rechner wird eine Unterstützung durch Softwarewerkzeuge benötigt.
Prinzipiell können "Modelle" einfach mit Hilfe von 2D-Zeichenwerkzeugen wie beispielsweise Dia oder MS Visio erstellt werden.
Solche Programme bieten oft schon passende Formen und Verbindungen, beispielsweise für BPMN [#f1]_ an. 

Ein Benutzer macht die Bedeutung eines solchen Diagrammes an den erkennbaren grafischen Formen und deren Aussehen fest; insofern wäre dies für Menschen durchaus ausreichend.

Durch ein Zeichenprogramm wird das Diagramm intern nur als eine "Ansammlung" von Bildpunkten oder geometrischen Primitiven dargestellt und auch entsprechend gespeichert ("persistiert").
Für ein solches Programm hat die Semantik des Modells keinerlei Bedeutung. 
So ergibt sich ein Problem, wenn der modellierte Prozess automatisch ausgeführt oder verändert werden soll. 
Wie soll den grafischen Elementen eine Bedeutung zugeordnet werden?

Daher sind eher Werkzeuge gefragt, die auch intern eine "Vorstellung" von Modellierungskonzepten haben. :cite:`volz_werkzeugunterstutzung_2011`
Solche Werkzeuge werden oft – auch in dieser Arbeit – "Modellierungswerkzeuge" genannt.

Ein solches Werkzeug bietet die Möglichkeit, Modelle zu erstellen, diese in sinnvoller Form zu persistieren und wieder aus einer physikalischen Repräsentation zu laden. 
Dem Benutzer wird überlicherweise eine Palette an Modellelementen angeboten, die in einem konkreten Prozessmodell eingesetzt werden können. 
Ein Anwender "baut" ein Modell, indem er grafische Objekte miteinander auf einer "Zeichenfläche" kombiniert.

Für BPMN gibt es verschiedene solcher Werkzeuge, wie beispielsweise ARIS oder ?.

Ein Modellierungswerkzeug für die perspektivenorientierten Prozessmodellierung wird in :num:`Abbildung #ipm2` gezeigt. 
Auf der linken Seite lässt sich die Palette mit den Modellelementen erkennen, die in verschiedene "Gruppen" eingeteilt sind.

.. _ipm2:

.. figure:: _static/ext_pics/ipm2d-editor.png
    :width: 16cm

    Prozessmodellierungswerkzeug i>PM2 aus :cite:`roth`

Als physische Repräsentation von Modellen ist es besonders praktisch, wenn diese in einem nicht-proprietärem Format verfügbar ist. 
Damit ist es möglich, solche Modelle mit verschiedenen Werkzeugen zu nutzen. 
Für BPMN ist beispielsweise XPDF als (XML-)Austauschformat verbreitet. Ein solches Format lässt sich auch als textuelle Darstellung eines (Prozess-)Modells bezeichnen.
Textuelle Darstellungen sind für die automatische Verarbeitung gut geeignet, können aber durchaus auch von Menschen gelesen und – mit Einschränkungen – bearbeitet werden.

.. _metamodellierung:

Metamodellierung
================

Die schon erwähnte, nötige Flexibilität von Prozessmodellen erfordert oft, dass die Modellierungssprache selbst verändert werden kann. 
Dadurch wird damit die Möglichkeit geschaffen, die Sprache an spezielle Bedürfnisse anzupassen. 
So lassen sich sogenannte domänenspezifische Sprachen (DSL) erstellen, die gegenüber fest vorgegebenen Sprachen den Vorteil besitzen, Sachverhalte in einer konkreten Domäne besser, also verständlicher und direkter darstellen zu können. (Volz und noch ein paar andere) 

Standardisierte Sprachen, wie BPMN definieren zahlreiche Elemente. Die Auswahl an Elementen ist dabei abgeschlossen, es können nicht einfach weitere Typen hinzugefügt werden.
Andererseits kann es auch sinnvoll sein, die verfügbaren Elemente für einen Anwendungsfall zu reduzieren.

Außerdem biete es sich an, die konkrete Darstellung von Modellelementen flexibel zu gestalten, um sie an spezielle Anforderungen anpassen zu können, wie es in :cite:`jabl-vis` vorgeschlagen wird.

Wie schon tt angedeutet wurde sind für Prozessmodelle eine Vielzahl von verschiedenen Entitäten und Beziehungstypen nötig.

Zur Beschreibung von (domänenspezifischen) Sprachen lässt sich das Konzept der "Metamodellierung" einsetzen.
Ein Metamodell stellt sozusagen ein Modell für eine Klasse von Modelle dar.

Zitat?!

Durch die Anpassung eines Metamodells lässt sich die abstrakte und konkrete Syntax einer Sprache verändern. 
So können neue Modellelemente hinzugefügt und bestehende angepasst oder entfernt werden. 
Im Falle einer visuellen Sprache lässt sich die konkrete Repräsentation von Modellelementen, also deren Aussehen und Form ändern.

Um Metamodelle zu "erstellen" ist es notwendig, diese auf eine wohldefinierte Weise beschreiben zu können. 
Dies leistet das im Folgenden vorgestellte Linguistic Meta Model (LMM), welches im Rahmen der Open Meta Modelling Environment (OMME), einer Metamodellierungsumgebung, entstanden ist. :cite:`volz_werkzeugunterstutzung_2011`

.. _lmm:

Linguistic Meta Model
---------------------

LMM stellt eine Sprache bereit, welche zur Definition von Metamodellen dient. 
:num:`Abbildung #lmm-model` zeigt die grundlegenden LMM-Elemente und deren Hierarchie.

.. _lmm-model:

.. figure:: _static/ext_pics/bernhard-lmmmodel.png
    :width: 16cm

    Hierarchie der LMM-Elemente aus :cite:`volz_werkzeugunterstutzung_2011`


Das zentrale Element im LMM ist das "Concept". 
Ein Concept kombiniert Eigenschaften einer Klasse und eines Objekts, wie sie aus objektorientierten Programmiersprachen bekannt sind.
So kann ein Concept – wie eine Klasse – Attribute definieren. Gleichzeitig kann ein Concept – wie ein Objekt –  Wertzuweisungen enthalten.
Anders ausgedrückt können Concepts sowohl eine "Typ-Facette", welche Attribute definiert als auch eine "Instanz-Facette", welche Zuweisungen vornimmt, beinhalten.

Ein Vergleich zwischen Klasse-Objekt-Beziehungen und Concept-Concept-Beziehungen  ist in :num:`Abbildung #vergleich-lmm` zu sehen.

.. _lmm-model:

.. figure:: _static/diags/vergleich_lmm.eps
    :width: 16cm

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

.. figure:: _static/diags/concreteuseof.eps
    :width: 16cm

    Instanz-Spezialisierung ausgehend von ConceptD

In der Abbildung spezialisiert ``UseA`` ``ConceptD`` (``concreteUseOf``). ``UseA`` übernimmt dabei alle Zuweisungen von ``ConceptD``, damit hat das Attribut in ``UseA`` ebenfalls den Wert 5.5.
``UseB`` dagegen setzt wiederum einen Wert für das Attribut ``c``. Das heißt, dass in ``UseB`` die bisherige Zuweisung "überschrieben" wird und damit den Wert 0 hat.
Für ``ConceptD`` ändert sich dabei nichts; die Überschreibung wirkt sich nur in ``UseB`` aus.

In LMM lässt sich für Attribute festlegen, inwieweit das Überschreiben von Werten zulässig ist und welche Bedeutung dies hat. 
Für die vorliegende Arbeit wird aber immer angenommen, dass Werte überschrieben werden dürfen.

LMM-(Meta-)Modelle lassen sich mit der Sprache Linguistic Meta Language (LML) :cite:`volz_werkzeugunterstutzung_2011` in einer textuellen Form beschreiben
Die Syntax ist an bekannte Programmiersprachen wie C++ oder C# angelehnt und kann daher als "menschenlesbar" angesehen werden. 
Gleichzeitig ist es damit möglich, LMM automatisch zu verarbeiten oder es sogar für die Beschreibung von Software zu nutzen, wie im Folgenden am Beispiel MDF deutlich wird.

Model Designer Framework
------------------------

Ebenfalls als Teil der Metamodellierungsumgebung OMME ist das Model Designer Framework (MDF) :cite:`roth` entstanden. 
Dieses erlaubt es, Modell-Editoren mit Hilfe von LMM-Metamodellen zu spezifizieren.
So lassen sich grafische Modellierungswerkzeuge auf Basis von MDF für beliebige (domänenspezifische) Modellierungssprachen erstellen.

:num:`Abbildung #mdf-modellhierarchie` zeigt die in MDF verwendeten Modelle. Hier sollen nur kurz die für die vorliegende Arbeit wichtigsten Aspekte verdeutlicht werden.
Details können bei :cite:`roth` im Kapitel 5, Modellhierarchie nachgelesen werden.

.. _mdf-modellhierarchie:

.. figure:: _static/ext_pics/mdf-modellhierarchie.png
    :width: 16cm

    Modellhierarchie von MDF mit Domain-Model- und Designer-Stack aus :cite:`roth`

Der *Domain-Model-Stack* (links) enthält alle Modelle, die für die Domäne relevant sind. 
Das *Domain-Metamodel* legt die Elemente der domänenspezifische Sprache fest, welche im *Domain-Model* genutzt werden um ein Modell zu beschreiben.

Rechts wird der *Designer-Model-Stack* gezeigt, der den Editor für die Dömane spezifiziert. 
Das *Graphical-Definition-Model* beschreibt Figuren, die sich für die Visualisierung der Domäne einsetzen lassen. 
Figuren werden über das *Editor-Definition-Model* mit den Domänenmodellelementen verbunden. So wird die grafische Repräsentation der Modellelemente im Editor festgelegt.

Bemerkenswert ist, dass auf allen Ebenen LMM – textuell dargetellt durch LML – verwendet wird. 
Damit wird LMM sowohl für die Beschreibung der Modellierungswerkzeugs als auch für die persistente Speicherung und interne Darstellung der mit dem Werkzeug erstellten Modelle genutzt.

:num:`Abbildung #ipm-typ-verwendung-1` zeigt einen Prozess, die in einem mit MDF definierten Editor ("i>PM2") für die :ref:`POPM <popm>` erstellt wurde. 
i>PM2 folgt den Prinzipien von i>PM, dem "intelligent ProcessModeler" (ref:), 
Im Gegensatz zu :num:`ipm-process`, welche einen sehr ähnlichen Prozess in i>PM zeigte, werden hier operationale und organisationale Perspektive durch geometrisches "Enthaltensein" im Prozess dargestellt.

Typ-Verwendungskonzept
======================

An :num:`Abbildung #ipm-typ-verwendung-1` und :num:`Abbildung #ipm-typ-verwendung-2` lässt sich ein Konzept – das "Typ-Verwendungs-Konzept" – welches von i>PM(2) umgesetzt wird zeigen. 

Das Grundprinzip des Typ-Verwendungs-Konzeptes ist es, einmal erstellte Objekte in unterschiedlichen Zusammenhängen zu verwenden. 

:num:`Abbildung #ipm-typ-verwendung-1` zeigt den Prozess "Notiz aufnehmen" (*A*). 
Nun wird eine sehr ähnliche Funktionalität für einen anderen Prozess benötigt, der in :num:`Abbildung #ipm-typ-verwendung-2` gezeigt ist. 
Hier ist der Prozess "Notiz erstellten / ergänzen" (*B*) zu sehen. 
Um diesen Prozess zu definieren könnte nun ein komplett neues "Objekt" erstellt werden.
Es ist allerdings schon ein "Objekt" mit nahezu gleichen Eigenschaften vorhanden, nämlich der vorher genannte Prozess *A*. 
Wie in der Informatik üblich wäre es wünschenwert, solche Redundanzen zu vermeiden und die "Wiederverwendbarkeit" zu erhöhen.

Dazu kann ein "Typ" definiert werden, vom dem mehrere "Verwendungen" erstellt werden, die dann in mehreren Kontexten eingesetzt werden können.
Hier könnte beispielsweise der Typ T angelegt werden. T ist eine "Instanz" eines Prozesses.
T legt fest, dass die Funktion des Prozesses "Notiz aufnehmen" (also der auf der Figur angezeigte Text) sein soll und "OneNote" und "Agent" mit ihm assoziiert sind.
*A* kann nun direkt als Verwendung von T gesehen werden. *A* übernimmt alle Eigenschaften von T.

Um den Prozess *B* darzustellen müssen jedoch zwei Änderungen vorgenommen werden. 
Das ist möglich, da eine Verwendung Werte des Typs überschreiben kann. 
So wird also in der Verwendung für *B* einfach die vordefinierte Funktion durch "Notiz erstellen / ergänzen" ersetzt und "Outlook" zu den operationalen Einheiten hinzugefügt.

.. _ipm-typ-verwendung-1:

.. figure:: _static/ext_pics/ipm2-typ-verwendung_2.png
    :width: 16cm

    Prozess in i>PM2 aus :cite:`volz_werkzeugunterstutzung_2011`


.. _ipm-typ-verwendung-2:

.. figure:: _static/ext_pics/ipm2-typ-verwendung_1.png
    :width: 16cm

    Prozess mit angepasster Verwendung aus :cite:`volz_werkzeugunterstutzung_2011`

Offensichtlich lässt sich dieses Konzept mit dem in :ref:`LMM <lmm>` eingeführten Modellierungsmuster der **Instanz-Spezialisierung** leicht realisieren.
Nach der Terminilogie des Typ-Verwendungs-Konzepts ist in der früher gezeigten :num:`Abbildung #concreteuseof` ``ConceptD`` ein "Typ", ``UseA`` und ``UseB`` sind "Verwendungen" davon.

Das Typ-Verwendungs-Konzept sowie die Realisierung mittels der Instanz-Spezialisierung wird auch in der vorliegenden Arbeit genutzt.

.. [#f1] Business Modeling and Notation, vereinfacht gesagt eine standardisierte und verbreitete grafische Prozessmodellierungssprache. Siehe 
