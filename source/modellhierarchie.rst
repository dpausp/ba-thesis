.. _modellhierarchie:

****************
Modellhierarchie
****************

In der Prozessmodellierung ist es sinnvoll, neben den Modellen auch die zugrundeliegende Modellierungssprache und Visualisierung an spezielle Anforderungen anpassen zu können (siehe :ref:`\Metamodellierung <metamodellierung>`). Daher war eine solche Flexibilität auch für das vorliegende Arbeit erwünscht (:ref:`Anforderung (d) <anforderungen>`). 

Daher wurde das Konzept verfolgt, die verwendete grafische Modellierungssprache über austauschbare Metamodelle zu definieren. (*Anforderung (c)*)
Ein wichtiger Punkt ist, dass sich die abstrakte Syntax der Sprache und die konkrete Syntax (die "Visualisierung") getrennt beschreiben lassen. 
Diesem Konzept folgt das bereits vorgestellte :ref:`Model Designer Framework<mdf>`.
Die hier vorgestellte Modellhierarchie ist ähnlich zu der von MDF definierten aufgebaut und übernimmt einige Begriffe von dort. 
Auf wichtige Unterschiede wird in diesem Kapitel explizit hingewiesen.

Das Konzept und die Implementierung der vorliegenden Arbeit erreicht jedoch nicht die Flexibilität von MDF, da hier ein Modellierungswerkzeug für die POPM und kein generisches Framework realisiert werden sollte. 
Dennoch ist es möglich, in einem gewissen Rahmen Modifikationen an den verwendeten Modellelementen vorzunehmen und deren Visualisierung anzupassen. 

Inwieweit sich die Modelle anpassen lassen und welche Einschränkungen bestehen wird im aktuellen Kapitel und bei der näheren Vorstellung der :ref:`Metamodelle<metamodelle>` deutlich.
Am Ende des nächsten Kapitels ist ein :ref:`Anwendungsbeispiel<beispiel-neues-element>` zu finden, welches zeigt, wie neue Elemente zur Modellierungssprache hinzugefügt werden können.

Die Anpassbarkeit der konkreten Syntax hat für den hier realisierten Prototypen vor allem den praktischen Nutzen, dass leicht mit der Visualisierung experimentiert werden kann, ohne den Quellcode der Anwendung ändern und neu übersetzen zu müssen.
Grundsätzlich lässt sich auch die verwendete Modellierungssprache komplett austauschen, jedoch wird in dieser Arbeit davon ausgegangen, dass das vorgegebene :ref:`Prozess-Metamodell <pmm>` genutzt wird. 

:num:`Abbildung #modellhierarchie-diagram` zeigt, wie sich die Hierarchie der Modelle darstellt, welche sich in einen **Editor-Model-Stack** und einen **Domain-Model-Stack** aufteilt.
Nach einer kurzen Vorstellung der Modellierungssprache wird eine Übersicht über die beiden Model-Stacks gegeben.

.. _modellhierarchie-diagram:

.. figure:: _static/diags/modellhierarchie.eps
    :width: 16cm

    Modellhierarchie in i>PM3D (angelehnt an MDF :cite:`roth_konzeption_2011`)


.. _lmmlight:

LMMLight
========

Die Modelle werden mit Hilfe einer Metamodellierungssprache spezifiziert, die vom :ref:`lmm` abgeleitet ist. 
Zum weiteren Verständnis ist es ausreichend, die in diesem Abschnitt gezeigten Grundelemente und Prinzipien zu kennen.

Die hier verwendete Sprache, im Folgenden **LMMLight** genannt, folgt in vielen Aspekten LMM, ohne jedoch alle weiterführenden Modellierungsmuster zu unterstützen, um eine einfache Implementierung zu ermöglichen. 
Konkret hat dies zur Folge, dass der textuelle Modell-Editor von OMME für die Erstellung von LMMLight-Modellen sinnvoll genutzt werden kann, solange auf die nicht unterstützten Modellierungsmuster verzichtet wird.

LMMLight unterstützt das Muster der **Spezialisierung von Instanzen** (``concreteUseOf``), welches unter anderem für die Realisierung des :ref:`Typ-Verwendungs-Konzepts<tvk>` genutzt wird.
Im Gegensatz zu LMM lassen sich in Spezialisierungen alle Attributzuweisungen des spezialisierten Concepts ohne Einschränkung überschreiben.

.. _editor-model-stack:

Editor-Model-Stack
==================

Der *Editor-Model-Stack* von i>PM3D enthält alle Modelle, die dafür zuständig sind, die Visualisierungsparameter eines Domänenmodells zu beschreiben. 
Außerdem werden hier Parameter spezifiziert oder gesetzt, welche die physikalische Repräsentation oder die für das Modellelement angebotenen Funktionalitäten im interaktiven Modellierungswerkzeug beeinflussen.
Näheres hierzu wird im nächsten Kapitel erläutert.
Mit "Repräsentation" ist im Folgenden die Gesamtheit dieser Parameter gemeint. 

Die Verknüpfung mit dem *Domain-Model-Stack* wird hergestellt, indem in Concepts des *Editor-Model-Stacks* eine Referenz auf *Domain-Model-Stack*-Concepts angegeben wird (:num:`Abbildung #editor-domain-conn`). 
In :num:`Abbildung #modellhierarchie-diagram` wird dies durch gestrichelte Pfeile dargestellt.
Besagte Referenzen werden durch das Attribut ``modelElementFQN`` angegeben, welchem der voll qualifizierte Name (FQN) des referenzierten Concepts zugewiesen wird. 
Vollqualifizierte Namen entstehen nach dem Schema <Model>.<Level>.<Package>.<Concept>, beispielsweise ``EMM.D1.nodeFigures.ProcessNode``.

.. _editor-domain-conn:

.. figure:: _static/diags/editor-domain-conn.eps
    :width: 15cm

    Assoziation zwischen abstraktem Modellelement und konkreter Repräsentation 


Anpassbarkeit
-------------

Durch Anpassungen im Editor-Model-Stack können für ein Domänen-Metamodell mehrere verschiedene Repräsentationen erstellt werden. 
Im Vergleich zur Modellhierarchie von :ref:`MDF<mdf>` ist das im *Designer-Model-Stack* von MDF definierte *Graphical-Meta-Model* und das *Editor-Meta-Model* zusammengelegt. 
Durch die fehlende Trennung von grafischer Darstellung und Editor-Mapping wird die Wiederverwendbarkeit im Vergleich zu MDF allerdings eingeschränkt.
Bei getrennten Modellen ist es möglich, eine "Bibliothek" von Visualisierungselementen bereitzustellen, aus der Elemente ausgewählt und in beliebig vielen Editor-Definitionen verwendet werden können.
Da der Fokus dieser Arbeit auf der (perspektivenorientierten) Prozessmodellierung liegt, wurde jedoch darauf verzichtet, um die Implementierung zu vereinfachen.
Dabei wird hingenommen, dass die Repräsentationen der einzelnen Domänenmodellelemente (auch "Figuren" genannt) für jede neue Repräsentation des Domänenmodells komplett neu beschrieben werden müssen.

Bei der Erstellung der Figuren muss berücksichtigt werden, dass durch die Implementierung der :ref:`modellkomponente` eine feste Auswahl an Visualisierungsparametern definiert ist. 
Welche dies sind, kann in der Beschreibung der :ref:`modellanbindung-svars` nachgelesen werden.

*Editor-Definition-* und *Editor-Meta-Model* können zwar konzeptionell – wie im MDF – unterschieden werden; 
jedoch wird in dieser Arbeit davon ausgegangen, dass diese zusammen in einem Modell (im Sinne von LMM) definiert werden, welches hier als das **Editor-Metamodell** bezeichnet wird. 

Um eine andere Visualisierung festzulegen müsste das komplette Editor-Metamodell neu definiert werden, sinnvollerweise auf Basis des bestehenden Metamodells\ [#f1]_.

Übersicht über die Editor-Model-Ebenen
--------------------------------------

:num:`Abbildung #modellhierarchie-diagram` veranschaulicht, wie die Editor-Model-Ebenen, die im Folgenden vorgestellt werden, von "oben nach unten" definiert sind. 
*Programming-Language-Mapping*, *Editor-Base-Level* und *Editor-Definition-Level* ergeben zusammen das **Editor-Metamodell**, 
welches die Repräsentation eines bestimmten Domain-Metamodells oder – anders gesagt – einen **Editor** für das Domain-Metamodell spezifiziert.

Programming-Language-Mapping
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Auf der obersten Ebene des Stacks, die im Modell als Level ``D3`` zu finden ist, wird die Abbildung auf eine Programmiersprache – in dieser Arbeit auf Scala – definiert, welche in :ref:`scalamapping` beschrieben wird.
In der :num:`Abbildung #modellhierarchie-diagram` wird diese Ebene als *Programming-Language-Mapping* bezeichnet.

Editor-Base-Level
^^^^^^^^^^^^^^^^^

Darunter befindet sich auf Level ``D2`` der prinzipiell von der Modellierungsdomäne unabhängige Teil der Editor-Spezifikation.
Hier werden Concepts bereitgestellt, die die Grundlage der Repräsentation für Typen aus dem Domänenmodell darstellen.

In der :num:`Abbildung #modellhierarchie-diagram` ist diese Ebene als *Editor-Base-Level* zu finden.

Die beiden bisher beschriebenen Ebenen ``D3`` und ``D2`` können prinzipiell beliebig definiert werden, soweit dies von LMMLight unterstützt wird. 

.. _edef:

Editor-Definition-Level
^^^^^^^^^^^^^^^^^^^^^^^

Die Modellebene ``D1`` legt fest, auf welche Weise ein Elementtyp aus dem *Domain-Meta-Model* repräsentiert wird. 

Auf dieser Ebene müssen die folgenden Packages definiert sein (vorgegeben durch die Implementierung):

    * package ``nodeFigures`` definiert Concepts, die die Repräsentation von Knoten aus dem Domänenmodell beschreiben.
    * package ``connectionFigures`` definiert Concepts, die die Repräsentation von Kanten aus dem Domänenmodell beschreiben.
    * package ``sceneryObjects`` enthält die verwendbaren "Szenenobjekte" (*Anforderung (h): Anzeige beliebiger 3D-Objekte*). Szenenobjekt-Concepts haben keine Entsprechung im Domänenmodell, da sie kein Modellelement repräsentieren.

Damit ist fest vorgegeben, dass sich die Modellelemente in Knoten und Kanten unterscheiden lassen, also prinzipiell ein graphbasierter Ansatz genutzt wird (*Anforderung (a)*).
Zusammen bilden diese Packages den in der :num:`Abbildung #modellhierarchie-diagram` gezeigten *Editor-Definition-Level*. 

Es dürfen auch noch weitere Packages vorkommen, die Concepts enthalten, welche von Concepts aus den obigen Packages referenziert werden. 
Dies können beispielsweise Concepts für die Definition von Farben oder der Größe eines Objekts sein.

.. _euse:

Editor-Usage-Model
^^^^^^^^^^^^^^^^^^

Ebenfalls auf Level ``D1`` befindet sich das *Editor-Usage-Model*, das Verwendungen, also Spezialisierungen von Concepts aus dem *Editor-Definition-Level* enthält. 

Analog zum *Editor-Definition-Level* sind die Verwendungen in drei Packages eingeteilt, die hier ``nodeUsages``, ``connectionUsages`` und ``sceneryObjectUsages`` genannt werden müssen.

Zusammen ergeben diese Verwendungen die konkrete Repräsentation eines Domänenmodells.
Diese Concepts spezifizieren hier also die Objekte, die vom Modellierungswerkzeug erstellt und auf der Zeichenfläche angezeigt werden. 

Sie legen damit beispielsweise fest, wo sich Modellelemente im Raum befinden und welche Ausrichtung sie haben. 
Dies sind auch typische Parameter, in denen sich alle Verwendungen einer Instanz unterscheiden.

Dem Konzept der Spezialisierung von Instanzen folgend kann hier auch die konkrete Visualisierung des Objekts beeinflusst werden. 
Wird in den Verwendungen für ein Attribut kein Wert angegeben, wird der Wert aus dem konkret verwendeten Concept benutzt.

Modellelemente, die von derselben Instanz abstammen haben also grundsätzlich das gleiche Erscheinungsbild, solange keine Werte überschrieben werden.

.. _domain-model-stack:

Domain-Model-Stack
==================

Der Domain-Model-Stack umfasst alle Modelle, welche die Modellierungsdomäne beschreiben. 
Durch das *Domain-Meta-Model* wird die abstrakte Syntax der Modellierungssprache festgelegt.

Domain-Meta-Model
-----------------

Durch das *Domain-Meta-Model* werden die im *Domain-Model* erlaubten Modellelemente vorgegeben.
An die Struktur des Modells, also den Aufbau aus Levels und Packages, werden durch die Implementierung keine besonderen Anforderungen gestellt.

Durch den :ref:`edef` wurde bereits vorgegeben, dass ein graphbasierter Visualisierungsansatz genutzt wird.
Passend dazu werden hier Knoten definiert, die mittels Kanten verbunden sind.

In der Implementierung von i>PM3D wird angenommen, dass Knoten und Kanten über spezielle Attribute der Knoten logisch miteinander verbunden sind. 
So muss im Concept, das den Knotentyp beschreibt, jeweils ein Attribut für eingehende und ausgehende Kanten eines bestimmten Typs definiert sein. 
Diesen Attributen werden die ein- bzw. ausgehenden Kanten durch das Modellierungswerkzeug zugewiesen.
Die Existenz von zugehörigen Attributen legt daher fest, in welcher Weise Kanten mit Knoten assoziiert werden können.
Es wird vorgesetzt, dass die Attributnamen für eingehende Kanten mit dem Präfix ``inbound`` und die ausgehenden mit ``outbound`` beginnen.
Der Rest des Attributnamens kann im Prinzip frei gewählt werden; jedoch wird in dieser Arbeit die Konvention benutzt, den Typnamen der Kante anzuhängen.

Ist also beispielsweise in einem Knotentyp für einen bestimmten Kantentyp nur ein ``outbound``-Attribut definiert, sind nur Verbindungen erlaubt, die ihren Startpunkt bei jenem Knotentyp haben. 
Der Endpunkt müsste dann bei einem anderen Knotentyp liegen, der ein entsprechendes ``inbound``-Attribut besitzt\ [#f2]_.
Das Prinzip wird im nächsten Kapitel bei der Vorstellung des verwendeten :ref:`Prozess-Metamodells<pmm>` und anschließend in einem :ref:`Anwendungsbeispiel<beispiel-neues-element>` verdeutlicht.

Ansonsten können im Modellierungswerkzeug modifizierbare Modellattribute frei definiert werden, wobei beachtet werden muss, dass von der Implementierung nur Literaltyp-Attribute allgemein (außer für die Assoziation von Knoten und Kanten, wie vorher beschrieben) unterstützt werden. 
Attribute, die Concepts referenzieren, können im Editor nicht angezeigt oder verändert werden und werden ignoriert\ [#f3]_.

.. _domain-model:

Domain-Model
------------

Das *Domain-Model* enthält das konkrete Domänenmodell, wie es im Modellierungswerkzeug durch die zugehörigen Concepts aus dem :ref:`euse` visualisiert wird.
Zusammen mit dem :ref:`euse` ergibt dies den aktuellen Zustand des angezeigten Modells, welcher persistiert und wieder geladen werden kann.
Das *Domain-Model* muss den Level ``M1`` enthalten, auf dem die im Folgenden genannten Packages definiert sind.

Für die Erzeugung von Knoten im *Domain-Model* wird immer das :ref:`tvk` verwendet. 
Dies bedeutet, dass im *Domain-Meta-Model* Concepts definiert sind, von welchen im *Domain-Model* ein Instanz ("Typ-Concept")erstellt wird. 
Von *Typ-Concepts* kann eine Verwendung (in Form einer Spezialisierung der Instanz) im *Domain-Model* erzeugt werden.

Die Implementierung gibt vor, dass die benutzerdefinierten Typen in einem Package mit dem Namen ``types`` abgelegt werden.
Verwendungen davon werden im Package ``nodeUsages`` abgelegt.

Für Kanten kommt das Typ-Verwendungs-Konzept im Domänenmodell nicht zum Einsatz. 
Kanten sind daher direkte Instanzen von Typen aus dem *Domain-Meta-Model* und werden zum Package ``connections`` hinzugefügt.


.. [#f1] "Copy-And-Paste-Wiederverwendung"

.. [#f2] Im Domänenmodell sind Kanten also technisch gesehen immer "gerichtet".

.. [#f3] Als "Ausweg" kann ein zusätzlicher Knotentyp und eine passende Verbindung definiert werden, so dass der Sachverhalt vom Editor visualisiert und modifiziert werden kann.

