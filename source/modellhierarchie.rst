.. _metamodelle:

*****************
Modell-Hierarchie
*****************

Da es in der Prozessmodellierung oft sinnvoll ist, neben den Prozessmodellen selbst auch die zugrundeliegende Modellierungssprache und die Visualisierung derselbigen an spezielle Anforderungen anpassen zu können war diese Flexibilität auch für das vorliegende Arbeit erwünscht. 

Deshalb wurde der Ansatz verfolgt, die Visualisierung und die in einem Modell nutzbaren Modellelemente über austauschbare Metamodelle zu definieren. 

Ein wichtiger Punkt ist, dass sich Domänenmodellierungssprache und die Visualisierung getrennt beschreiben lassen. Demselben Konzept folgt das in :ref:`metamodellierung` vorgestellte *Model Designer Framework* (MDF) :ref:`ma-bastian`
mit dessen Hilfe sich grafische Editoren für beliebige domänenspezifische Modellierungssprachen erstellen lassen.

Das Konzept und die Implementierung der vorliegenden Arbeit erreicht jedoch nicht die Flexibilität von MDF, da hier die Modellierung von Prozessen nach den Vorgaben der perspektivenorientierte Prozessmodellierung und kein generisches Framework realisiert werden sollte. Dennoch ist es leicht möglich, Modifikationen an den verwendeten Prozessmodellelementen vorzunehmen und deren Visualisierung anzupassen. 

Grundsätzlich lässt sich auch die verwendete Modellierungssprache komplett austauschen, jedoch wird in dieser Arbeit davon ausgegangen, dass das vorgegebene Prozess-Metamodell genutzt wird. 

Inwieweit sich die Modelle anpassen lassen und welche Einschränkungen bestehen wird im vorliegenden und nächsten Kapitel :ref:`metamodelle` näher erläutert

Die Modelle werden mit Hilfe einer textuellen Modellierungssprache spezifiziert, die vom Linguistic Meta Model (LMM) der Open Meta Modeling Environment (OMME) abgeleitet ist. 

Die hier verwendete Sprache, im Folgenden *LMMLight* genannt folgt in vielen Aspekten LMM, ohne jedoch alle weiterführenden Modellierungsmuster zu unterstützen. Konkret hat dies zur Folge, dass der textuelle Modell-Editor von OMME für die Erstellung von LMMLight-Modellen sinnvoll genutzt werden kann wenn auf die nicht unterstützten Modellierungsmuster verzichtet wird.

Zum weiteren Verständnis ist es ausreichend, die Grundelemente und -prinzipien von LMM zu kennen, wie sie in :ref:`metamodellierung` kurz vorgestellt oder von :cite:`volz_werkzeugunterstuetzung_2011` detailliert beschrieben werden.

Zusätzlich sei noch gesagt, dass *LMMLight* das Muster der Instanz-Spezialisierung ("concreteUseOf") unterstützt, da dieses für die Realisierung des  genutzten Typ-Verwendungs-Konzepts hilfreich ist.

Details zu LMMLight und der Implementierung finden sich im späteren Kapitel :ref:`implementierung_lmmlight`.

:num:`Abbildung #modellhierarchie` zeigt, wie die für I>PM3D genutzte Hierarchie der Modelle aussieht, die sich in einen *Editor-Model-Stack* und einen *Domain-Model-Stack* aufteilt.
Im Rest dieses Kapitels wird eine Übersicht über die beiden abgegrenzten *Model-Stacks* gegeben.

Die detaillierte Spezifikation der für diese Arbeit verwendeten Metamodelle wird im nächsten Kapitel vorgestellt. 

.. _editor-model-stack:

Editor-Model-Stack
==================

Der *Editor-Model-Stack* von I>PM3D enthält alle Modelle die in erster Linie dafür zuständig sind, die Visualisierung eines Domänenmodells zu beschreiben. 
Außerdem können hier Parameter spezifiziert und gesetzt werden, die beispielsweise die physikalische Repräsentation oder die für das Modellelement angebotenen Funktionalitäten im interaktiven Modellierungswerkzeug beeinflussen.

Die Verknüpfung von Editormodell mit dem Domänenmodell wird dadurch herstellt, dass in den Concepts des Editor-Model-Stacks, die Domain-Model-Concepts repräsentieren, eine Referenz auf Letztere angegeben wird.

Die Gesamtheit aus Visualisierungs- und sonstigen Parametern, die für den Modell-Editor relevant lässt sich als "Repräsentation" eines einzelnen Domänenmodellelements oder des ganzen Domänenmodells bezeichnen.

Durch Anpassungen im Editor-Model-Stack können für ein Domänenmodell im Prinzip auch mehrere verschiedene Repräsentationen erstellt werden. 

Im Vergleich zu der Modellhierarchie von MDF ist zu sehen, dass das im *Designer-Model-Stack* von MDF definierte *Graphical-Meta-Model*, das die Visualisierung an sich festlegt und das *Editor-Meta-Model*, das die Verknüfung zwischen Domänenmetamodell und Graphical-Meta-Model herstellt zusammengelegt worden sind. 

Durch die fehlende Trennung von grafischer Darstellung und Editor-Mapping wird die Wiederverwendbarkeit im Vergleich zu MDF allerdings eingeschränkt.
Bei getrennten Modellen ist es möglich, eine "Bibliothek" von Visualisierungselementen bereitzustellen, aus der Elemente ausgewählt und in beliebig vielen Editor-Definitionen verwendet werden können.

Um die Implementierung zu vereinfachen wurde jedoch darauf verzichtet. 

Dabei wird hingenommen, dass die Repräsentationen der  neu beschrieben werden müssen.

Die in :num:`Abbildung #modellhierarchie` angegebenen Editor-Definition- und Editor-Meta-Modelle können zwar konzeptionell – wie im MDF – unterschieden werden, jedoch wird in dieser Arbeit davon ausgegangen, dass diese zusammen in einem Modell definiert werden, das im Folgenden vereinfachend als Editor-Metamodell bezeichnet wird.

Um eine andere Visualisierung festzulegen müsste daher das komplette Editor-Metamodell neu definiert werden, sinnvollerweise auf Basis des bestehenden Metamodells.\ [#f2]_

Übersicht über die Editor-Model-Ebenen
--------------------------------------

Auf der obersten Ebene des Stacks, die im Modell als Level M3 zu finden ist, wird die Abbildung auf eine Programmiersprache – in Rahmen dieser Arbeit also auf Scala – definiert, welche in :ref:`emm-scalamapping` beschrieben wird.

Darunter befindet sich auf Level M2 der prinzipiell von der Modellierungsdomäne unabhängige Teil der Editor-Spezifikation, der Concepts bereitstellt, die die Grundlagen der Repräsentation für Typen aus dem Domänenmodell darstellen.

Es wird vorausgesetzt, dass sich hier ein Package mit dem Namen *figures* befindet, welches vom Editor genutzt wird

Welche Konzepte hier definiert werden wird in :ref:`emm-meta` näher beschrieben.

Level M1 enthält die Modellebene, die festlegt, auf welche Weise ein Typ aus dem Domänenmodell repräsentiert wird, wie in :ref:`emm-definition` dargestellt wird. 

Auf dieser Ebene müssen die folgenden Packages definiert sein:

    * package "nodeFigures" definiert Concepts, die die Repräsentation von Knoten aus dem Domänenmodell beschreiben.
    * package "connectionFigures" definiert Concepts, die die Repräsentation von Kanten aus dem Domänenmodell beschreiben.
    * Das package "sceneryObjects" enthält die verwendbaren Szenenobjekte. Szenenobjekte haben keine Entsprechung im Domänenmodell und stehen für sich alleine.

Zusammen bilden diese Packages das in der :num:`Abbildung #modellhierarchie` gezeigte *Editor-Definition-Model*. 
Es dürfen auch noch weitere Packages vorkommen, die Concepts enthalten, die von Concepts aus den obigen Packages referenziert werden. 
Dies können beispielsweise Concepts für die Definition von Farben oder der Größe eines Objekts sein.

Auf demselben Level befindet sich das *Editor-Usage-Model*, das Verwendungen, also Spezialisierungen der Instanzen aus dem *Editor-Definition-Model* enthält. 
Analog zum *Editor-Definition-Model* sind die Verwendungen in drei Packages eingeteilt, die hier "nodeUsages", "connectionUsages" und "sceneryObjectsUsages" genannt werden müssen.

Zusammen ergeben diese Verwendungen die konkrete Repräsentation eines Domänenmodells. Concepts spezifizieren hier also die Objekte, die vom Modellierungswerkzeug erstellt und angezeigt werden.

Sie legen damit zum Beispiel fest, wo sich Modellelemente im Raum befinden und welche Ausrichtung sie haben. Dies sind auch typische Parameter, in denen sich alle Verwendungen einer Instanz unterscheiden.

Dem Konzept der Instanz-Spezialisierung folgend kann hier auch die konkrete Visualisierung des Objekts beeinflusst werden. 
Wird in den Verwendungen für ein Attribut kein Wert angegeben, wird der Wert aus der spezialisierten Instanz benutzt.

Modellelemente, die von derselben Instanz abstammen haben also grundsätzlich das gleiche Erscheinungsbild, solange keine Werte überschrieben werden.

.. _domain-model-stack:

Domain-Model-Stack
==================

Durch das **Domäin-Meta-Model** wird eine Sprache definiert, mit der ein Modell in der spezifischen Domäne erstellt werden kann. Es legt also die Syntax, also die verwendbaren Konstrukte sowie deren Beziehungen fest. 

Es wird davon ausgegangen, dass sich das Metamodell auf eine graphbasierte Darstellung, die vom Editor-Metamodell bereitgestellt wird, abbilden lässt. 
An die Struktur des Modells, also den Aufbau aus Levels und Packages werden keine besonderen Anforderungen gestellt.

Also gilt das Prinzip, dass Knoten definiert werden können, die mittels Kanten verbunden sind.

Knoten und Kanten werden über spezielle Attribute der Knoten logisch miteinander verbunden. 
So wird im Knotentyp jeweils ein Attribut für eingehende und ausgehende Kanten eines bestimmten Typs definiert. Die Attribute sind Concept-Attribute vom Typ des Kantentyps.

Die Existenz von zugehörigen Attributen legt damit fest, in welcher Weise Kanten mit Knoten assoziiert werden können.

Die Namen dieser Attribute können frei gewählt werden; jedoch wird in dieser Arbeit die Konvention benutzt, die Attributnamen für eingehende Kanten mit dem Präfix "inbound" und die ausgehenden mit "outbound" zu beginnen und den Typ der Kante anzuhängen.

Ist also beispielsweise in einem Knotentyp für einen bestimmten Kantentyp nur ein "outbound"-Attribut definiert, sind nur Verbindungen erlaubt, die ihren Startpunkt bei jenem Knotentyp haben. Der Endpunkt müsste dann bei einem anderen Knotentyp liegen, der ein entsprechendes "inbound"-Attribut besitzt.[#f3]_

Das **Domain-Usage-Model** enthält das eigentliche Domänenmodell, also im Kontext dieser Arbeit die im Prozessmodell verwendeten Elemente, die vom Modellierungswerkzeug erstellt wurden.

Zusammen mit dem Editor-Usage-Model ergibt das den aktuellen Zustand des Editors, welcher persistiert und wieder geladen werden kann.

Für die Erzeugung von Knoten im Domain-Usage-Modell wird ausschließlich das Typ-Verwendungs-Konzept verwendet. 

Konkret bedeutet das hier, dass im Domain-Meta-Model Concepts [#f4]_ definiert werden, zu denen ein Typ-Concept als Instanz im Domain-Usage-Model erzeugt werden muss. 
Von diesen Type-Concepts kann dann eine Verwendung im Usage-Model – also im Sinne von LMM eine Spezialisierung des Type-Concepts – erzeugt werden.

Für Kanten kommt das Typ-Verwendungs-Konzept im Domänenmodell nicht zum Einsatz. Kanten sind daher direkte Instanzen von Typen aus dem Domain-Meta-Modell.

Ebene D1
--------
.. [f1]: Eine andere Möglichkeit wäre es, die Rotation mit den Komponenten einer Rotationsmatrix darzustellen. Dafür sind aber 9 Werte nötig, was die Modelle unnötig überfrachtet, da für jeden Wert ein eigenes Attribut definiert werden muss. 

.. [f2]: Klarer Fall von Copy-And-Paste-"Wiederverwendung" ;)

.. [f3]: Technisch gesehen sind Kanten also immer "gerichtet"; jedoch können auch "ungerichtete" Kantentypen erstellt werden, indem in allen beteiligten Knotentypen beide Attribute definiert werden und die Unterschiedung zwischen Kanten, die dem "outbound" oder "inbound"-Attribut zugewiesen werden einfach ignoriert wird. Die Visualisierung der Kanten sollte dann allerdings auch unabhängig von der Richtung sein.

.. [f4]: kann und sollte man das "Metatyp" nennen?

