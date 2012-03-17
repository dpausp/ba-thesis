.. _modellhierarchie:

*****************
Modelle in I>PM3D
*****************

Da es in der Prozessmodellierung oft sinnvoll ist, neben den Prozessmodellen selbst auch die zugrundeliegende Modellierungssprache und die Visualisierung derselbigen an spezielle Anforderungen anpassen zu können :cite:`jablonski` war diese Flexibilität auch für das vorliegende Arbeit erwünscht. 

Daher wurde der Ansatz verfolgt, die Visualisierung und die in einem Modell nutzbaren Modellelemente über austauschbare Metamodelle zu definieren, wie auch in :cite:`volz_werkzeugunterstuetzung_2011` beschrieben wird. 

Ein wichtiger Punkt ist, dass sich Domänenmodellierungssprache und die Visualisierung getrennt beschreiben lassen. Diesem Konzept folgt das in :ref:`metamodellierung` vorgestellte *Model Designer Framework* (MDF) :ref:`ma-bastian`
mit dessen Hilfe sich grafische Editoren für beliebige domänenspezifische Modellierungssprachen erstellen lassen. 

Die hier vorgestellte Modellhierarchie ist prinzipiell ähnlich zu der von MDF definierten aufgebaut und übernimmt einige Begriffe von dort. 
Auf wichtige Unterschiede wird im Folgenden explizit hingewiesen.

Das Konzept und die Implementierung der vorliegenden Arbeit erreicht jedoch nicht die Flexibilität von MDF, da hier die Modellierung von Prozessen nach den Vorgaben der perspektivenorientierte Prozessmodellierung und kein generisches Framework realisiert werden sollte. Dennoch ist es möglich, in einem gewissen Rahmen Modifikationen an den verwendeten Prozessmodellelementen vorzunehmen und deren Visualisierung anzupassen. 

Dies hat für den hier realisierten Prototypen vor allem den praktischen Nutzen, dass leicht mit der Visualisierung experimentiert werden kann ohne den Quellcode der Anwendung ändern und neu übersetzen zu müssen.

Grundsätzlich lässt sich auch die verwendete Modellierungssprache komplett austauschen, jedoch wird in dieser Arbeit davon ausgegangen, dass das vorgegebene Prozess-Metamodell genutzt wird. 

Inwieweit sich die Modelle anpassen lassen und welche Einschränkungen bestehen wird im vorliegenden und nächsten Kapitel :ref:`metamodelle` näher erläutert

:num:`Abbildung #modellhierarchie` zeigt wie sich die für I>PM3D genutzte Hierarchie der Modelle darstellt, die sich in einen *Editor-Model-Stack* und einen *Domain-Model-Stack* aufteilt.
Nach einer kurzen Vorstellung der Modellierungssprache wird im Rest dieses Kapitels wird eine Übersicht über die beiden abgegrenzten *Model-Stacks* gegeben.

Die detaillierte Spezifikation der für diese Arbeit verwendeten Metamodelle wird im nächsten Kapitel vorgestellt. 

LMMLight
========

Die Modelle werden mit Hilfe einer textuellen Modellierungssprache spezifiziert, die vom Linguistic Meta Model (LMM) der Open Meta Modeling Environment (OMME) abgeleitet ist. 

Die hier verwendete Sprache, im Folgenden *LMMLight* genannt, folgt in vielen Aspekten LMM, ohne jedoch alle weiterführenden Modellierungsmuster wie Powertypes, Materialization und Deep Instantiation zu unterstützen. 
Konkret hat dies zur Folge, dass der textuelle Modell-Editor von OMME für die Erstellung von LMMLight-Modellen sinnvoll genutzt werden kann, solange auf die nicht unterstützten Modellierungsmuster verzichtet wird.

*LMMLight* unterstützt allerdings das Muster der Instanz-Spezialisierung ("concreteUseOf"), da dies unter Anderem für die Realisierung des genutzten Typ-Verwendungs-Konzepts hilfreich ist.

Zum weiteren Verständnis ist es ausreichend, die Grundelemente und -prinzipien von LMM zu kennen, wie sie von :cite:`volz_werkzeugunterstuetzung_2011` detailliert beschrieben werden.

.. _editor-model-stack:

Editor-Model-Stack
==================

Der *Editor-Model-Stack* von I>PM3D enthält alle Modelle die in erster Linie dafür zuständig sind, die Visualisierung eines Domänenmodells zu beschreiben. 
Außerdem können hier Parameter spezifiziert und gesetzt werden, die beispielsweise die physikalische Repräsentation oder die für das Modellelement angebotenen Funktionalitäten im interaktiven Modellierungswerkzeug beeinflussen.

Die Verknüpfung von Editormodell mit dem Domänenmodell wird dadurch hergestellt, dass in den Concepts des Editor-Model-Stacks, die Domain-Model-Concepts repräsentieren, eine Referenz auf Letztere angegeben wird.

Die Gesamtheit aus Visualisierungs- und sonstigen Parametern, die für den Modell-Editor relevant lässt sich als "Repräsentation" eines einzelnen Domänenmodellelements oder des ganzen Domänenmodells bezeichnen.

Anpassbarkeit
-------------

Durch Anpassungen im Editor-Model-Stack können für ein Domänenmodell im Prinzip auch mehrere verschiedene Repräsentationen erstellt werden. 

Im Vergleich zu der Modellhierarchie von MDF ist zu sehen, dass das im *Designer-Model-Stack* von MDF definierte *Graphical-Meta-Model*, das die Visualisierung an sich festlegt und das *Editor-Meta-Model*, das die Verknüfung zwischen Domänenmetamodell und Graphical-Meta-Model herstellt zusammengelegt worden sind. 

Durch die fehlende Trennung von grafischer Darstellung und Editor-Mapping wird die Wiederverwendbarkeit im Vergleich zu MDF allerdings eingeschränkt.
Bei getrennten Modellen ist es möglich, eine "Bibliothek" von Visualisierungselementen bereitzustellen, aus der Elemente ausgewählt und in beliebig vielen Editor-Definitionen verwendet werden können.
Um die Implementierung zu vereinfachen wurde jedoch darauf verzichtet. 
Dabei wird hingenommen, dass die Repräsentationen der einzelnen Domänenmodellelemente (auch "Figuren" genannt) für jede neue Repräsentation des Domänenmodells komplett neu beschrieben werden müssen.

Bei der Erstellung der Figuren muss berücksichtigt werden, dass durch die Implementierung der Modell-Komponente nur ein feste Auswahl an Visualisierungsparametern angeboten wird. 
Welche dies sind kann im späteren Kapitel zur Modellanbindung unter :ref:`modellanbindung-svars` nachgelesen werden.

Editor-Definition- und Editor-Meta-Modelle können zwar konzeptionell – wie im MDF – unterschieden werden; 
jedoch wird in dieser Arbeit davon ausgegangen, dass diese zusammen in einem Modell definiert werden, welches hier als das **Editor-Metamodell** bezeichnet wird. 

Um eine andere Visualisierung festzulegen müsste das komplette Editor-Metamodell neu definiert werden, sinnvollerweise auf Basis des bestehenden Metamodells\ [#f1]_.

Übersicht über die Editor-Model-Ebenen
--------------------------------------

In :num:`Abbildung #modellhierarchie` wird dargestellt, wie die Editor-Model-Ebenen, die im Folgenden vorgestellt werden von "oben nach unten" definiert sind. 
**Editor-Base-Level** und **Editor-Definition-Level** ergeben zusammen das **Editor-Metamodell**.

Programming-Language-Mapping
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Auf der obersten Ebene des Stacks, die im Modell als Level D3 zu finden ist, wird die Abbildung auf eine Programmiersprache – in Rahmen dieser Arbeit also auf Scala – definiert, welche in :ref:`emm-scalamapping` beschrieben wird.
In der :num:`Abbildung #modellhierarchie` wird diese Ebene als **Programming-Language-Mapping** bezeichnet.

Editor-Base-Level
^^^^^^^^^^^^^^^^^

Darunter befindet sich auf Level D2 der prinzipiell von der Modellierungsdomäne unabhängige Teil der Editor-Spezifikation 
Hier werden Concepts bereitgestellt, die die Grundlage der Repräsentation für Typen aus dem Domänenmodell darstellen.

In der :num:`Abbildung #modellhierarchie` ist diese Ebene als **Editor-Base-Level** zu finden.
Welche Konzepte im verwendeten Metamodell auf dieser Ebene definiert werden, wird in :ref:`emm-meta` näher beschrieben.

Die beiden Ebenen D3 und D2, die bisher beschrieben worden sind können prinzipiell beliebig definiert werden, soweit dies von LMMLight unterstützt wird. 

Editor-Definition-Level
^^^^^^^^^^^^^^^^^^^^^^^

Level D1 enthält die Modellebene, die festlegt, auf welche Weise ein Typ aus dem Domänenmodell repräsentiert wird, wie in :ref:`emm-definition` dargestellt wird. 

Auf dieser Ebene müssen die folgenden Packages definiert sein:

    * package "nodeFigures" definiert Concepts, die die Repräsentation von Knoten aus dem Domänenmodell beschreiben.
    * package "connectionFigures" definiert Concepts, die die Repräsentation von Kanten aus dem Domänenmodell beschreiben.
    * Das package "sceneryObjects" enthält die verwendbaren Szenenobjekte. Szenenobjekte haben keine Entsprechung im Domänenmodell und stehen für sich alleine.

Zusammen bilden diese Packages das in der :num:`Abbildung #modellhierarchie` gezeigte **Editor-Definition-Level**. 

Es dürfen auch noch weitere Packages vorkommen, die Concepts enthalten, die von Concepts aus den obigen Packages referenziert werden. 
Dies können beispielsweise Concepts für die Definition von Farben oder der Größe eines Objekts sein.

Editor-Usage-Model
^^^^^^^^^^^^^^^^^^

Ebenfalls auf Level D1 befindet sich das **Editor-Usage-Model**, das Verwendungen, also Spezialisierungen der Instanzen aus dem Editor-Definition-Level enthält. 
Diese Concepts dürfen alle in der Instanz definierten Attributzuweisungen überschreiben.

Analog zum Editor-Definition-Level sind die Verwendungen in drei Packages eingeteilt, die hier "nodeUsages", "connectionUsages" und "sceneryObjectsUsages" genannt werden müssen.

Zusammen ergeben diese Verwendungen die konkrete Repräsentation eines Domänenmodells. Diese Concepts spezifizieren hier also die Objekte, die vom Modellierungswerkzeug erstellt und angezeigt werden.

Sie legen damit zum Beispiel fest, wo sich Modellelemente im Raum befinden und welche Ausrichtung sie haben. Dies sind auch typische Parameter, in denen sich alle Verwendungen einer Instanz unterscheiden.

Dem Konzept der Instanz-Spezialisierung folgend kann hier auch die konkrete Visualisierung des Objekts beeinflusst werden. 
Wird in den Verwendungen für ein Attribut kein Wert angegeben, wird der Wert aus der spezialisierten Instanz benutzt.

Modellelemente, die von derselben Instanz abstammen haben also grundsätzlich das gleiche Erscheinungsbild, solange keine Werte überschrieben werden.

.. _domain-model-stack:

Domain-Model-Stack
==================

Domain-Meta-Model
-----------------

Durch das **Domäin-Meta-Model** wird eine Sprache definiert, mit der ein Modell in der spezifischen Domäne erstellt werden kann. Es legt also die Syntax, also die verwendbaren Konstrukte sowie deren Beziehungen fest. 

An die Struktur des Modells, also den Aufbau aus Levels und Packages werden keine besonderen Anforderungen gestellt.

Es wird davon ausgegangen, dass sich das Metamodell auf eine graphbasierte Darstellung, die vom Editor-Metamodell bereitgestellt wird, abbilden lässt. 
Also gilt das Prinzip, dass Knoten definiert werden können, die mittels Kanten verbunden sind.

Knoten und Kanten werden über spezielle Attribute der Knoten logisch miteinander verbunden. 
So wird im Knotentyp jeweils ein Attribut für eingehende und ausgehende Kanten eines bestimmten Typs definiert. Die Attribute sind Concept-Attribute vom Typ des Kantentyps.

Die Existenz von zugehörigen Attributen legt damit fest, in welcher Weise Kanten mit Knoten assoziiert werden können.

Die Namen dieser Attribute können frei gewählt werden; jedoch wird in dieser Arbeit die Konvention benutzt, die Attributnamen für eingehende Kanten mit dem Präfix "inbound" und die ausgehenden mit "outbound" zu beginnen und den Typ der Kante anzuhängen.

Ist also beispielsweise in einem Knotentyp für einen bestimmten Kantentyp nur ein "outbound"-Attribut definiert, sind nur Verbindungen erlaubt, die ihren Startpunkt bei jenem Knotentyp haben. Der Endpunkt müsste dann bei einem anderen Knotentyp liegen, der ein entsprechendes "inbound"-Attribut besitzt.\ [#f2]_

Ansonsten können im Modellierungswerkzeug modifizierbare Modellattribute frei definiert werden, wobei beachtet werden muss, dass von der Implementierung nur literale Datentypen unterstützt werden. 
Concept-Attribute können im Editor nicht angezeigt oder verändert werden und werden ignoriert. \ [#f3]_

Domain-Usage-Model
-----------------

Das **Domain-Usage-Model** enthält das eigentliche Domänenmodell, also im Kontext dieser Arbeit die im Prozessmodell verwendeten Elemente, die vom Modellierungswerkzeug erstellt wurden.

Zusammen mit dem Editor-Usage-Model ergibt das den aktuellen Zustand des Editors, welcher persistiert und wieder geladen werden kann.

Für die Erzeugung von Knoten im Domain-Usage-Modell wird ausschließlich das Typ-Verwendungs-Konzept verwendet. 

Konkret bedeutet das hier, dass im Domain-Meta-Model Concepts\ [#f4]_ definiert werden, zu denen ein Typ-Concept als Instanz im Domain-Usage-Model erzeugt werden muss. 
Von diesen Type-Concepts kann dann eine Verwendung im Usage-Model – also im Sinne von LMM eine Spezialisierung des Type-Concepts – erzeugt werden.

Für Kanten kommt das Typ-Verwendungs-Konzept im Domänenmodell nicht zum Einsatz. Kanten sind daher direkte Instanzen von Typen aus dem Domain-Meta-Modell.


.. [#f1] Klarer Fall von Copy-And-Paste-"Wiederverwendung".

.. [#f2] Technisch gesehen sind Kanten also immer "gerichtet"; jedoch können auch "ungerichtete" Kantentypen erstellt werden, indem in allen beteiligten Knotentypen beide Attribute definiert werden und die Unterschiedung zwischen Kanten, die dem "outbound" oder "inbound"-Attribut zugewiesen werden einfach ignoriert wird. Die Visualisierung der Kanten sollte dann allerdings auch unabhängig von der Richtung sein.

.. [#f3] kann und sollte man das "Metatyp" nennen?

.. [#f4] Als "Ausweg" kann natürlich ein zusätzlicher Knotentyp und eine passende Verbindung definiert werden, was vom Editor visualisiert und modifiziert werden kann.

