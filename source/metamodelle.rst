.. _metamodelle:

*****************************
Spezifikation der Metamodelle
*****************************

Nachdem im vorherigen Kapitel eine Übersicht über die von i>PM3D unterstützten Metamodelle gegeben wurde, werden hier die im Projekt verwendeten Metamodelle für Editor und Domäne sowie deren Concepts genauer vorgestellt.
Das verwendete *Editor-Metamodell* wird im Folgenden mit **EMM** bezeichnet, das Domain-Meta-Model mit **PMM**.
Vollqualifizierte Namen (FQN) von Levels setzen sich aus dem Modellnamen und dem Levelnamen zusammen, getrennt durch einen Punkt. 
FQNs für Packages und Concepts entstehen, indem deren Namen analog angehängt werden.

.. _scalamapping:

Scala-Mapping
=============

Die oberste Ebene des *Editor-Model-Stacks* beinhaltet nur ein Package ``base`` (FQN ``EMM.D3.base``) mit einem einzelnen Concept, ``ScalaMapping``. 

Dieses Concept definiert Attribute, die festlegen, wie Concepts aus dem Modell auf Scala abgebildet werden, um im Modellierungswerkzeug genutzt werden zu können.

Für jedes Concept, das sich auf den weiter unten liegenden Ebenen befindet, muss das Attribut ``scalaType`` definiert werden, welches den korrespondierenden Scala-Typ angibt. 

Optional ist das Attribut ``typeConverter``, welches eine Klasse spezifiziert, die dazu genutzt wird, ein LMM-Concept in ein passendes Scala-Objekt umzuwandeln und umgekehrt.\ [#f1]_ 

Ohne TypeConverter-Angabe wird ``scalaType`` direkt als voll qualifizierter Klassenname interpretiert. 
Von dieser so angegebenen Klasse wird ein Objekt erstellt, welches das entsprechende Concept in der Anwendung vertritt.

Wird ein TypeConverter genutzt, muss der ``scalaType`` nicht zwingend ein Klassenname sein. 
Wie das Attribut interpretiert wird hängt vom jeweiligen TypeConverter ab. 

.. _ebl:

Editor-Base-Level
=================

Level ``D2`` ist als Instanz von ``D3`` definiert. Daraus folgt, dass alle hier definierten Concepts Instanzen von ScalaMapping sein müssen.

Die auf dieser Ebene definierten Concepts sind prinzipiell von der Prozessmodellierung unabhängig, orientieren sich aber an deren Bedürfnissen.

Auf ``D2`` werden zwei Packages, ``types`` und ``figures``, definiert.

.. _ebl-types:

Paket "types"
------------

Das ``EMM.D2.types``-Package definiert grundlegende Typen, die Visualisierungsparameter von Objekten und die Positionierung im Raum sowie deren Größe beschreiben.

Dazu werden folgenden Typen angeboten:

  * ``Dimension``, ``Position``: Spezifikation der Größe und der Position eines Objektes im dreidimensionalen Raum, welche in einem kartesischen Koordinatensystem angegeben werden.
    Die drei Attribute x, y, z werden im Editor auf einen Vektor mit 3 Komponenten abgebildet. Hierfür wird der Vektortyp ``Vec3`` von :ref:`Simplex3D` angeboten.

  * ``Rotation``: Angabe der Rotation mittels eines Quaternions. Die vier Attribute x0, x1, x2 und x3 werden auf ein Quaternionen-Objekt ``Quat4``  abgebildet, das ebenfalls von *Simplex3D* bereitgestellt wird.

  * ``Color``: Hiermit lassen sich Farben, die mittels im RGBA-Farbsystem als rot, grün, blau und alpha (Transluzenzwert) angegeben werden.
    Zu beachten ist hier, dass die Farben als Gleitkommazahl angegeben werden und einen Wertebereich von 0 bis 1 abdecken.
    Dieser Typ wird auf die ``java.awt.Color``-Klasse abgebildet.

  * ``Font``: Definiert Parameter für die Schriftdarstellung. Die Attribute und deren erlaubte Werte orientieren sich an ``java.awt.Font``, worauf dieses Concept abgebildet wird.
    Die Schriftfarbe muss separat mittels eines ``Color``-Concepts angegeben werden.

        * ``face``: Name der Schriftart
        * ``size``: Größe, als Ganzzahl angegeben
        * ``style``: Name des Schriftstils. Erlaubt sind hier "normal", "bold" und "italic", die den Werten der Enumeration FontStyle von java.awt entsprechen.


  * ``PhysicsSettings``: Sub-Concepts dieses abstrakten Concepts werden genutzt, um Objekten eine physikalische Repräsentation zu geben, wenn diese nicht auf anderem Wege definiert wurde.
    Es werden kugel- (``PhysSphere``) und quaderförmige (``PhysBox``) Geometrien angeboten, wie sie von der durch :ref:`simulatorx` bereitgestellten Physikkomponente unterstützt werden.
    Für eine ``PhysSphere`` muss der Radius angegeben werden; eine ``PhysBox`` wird analog über die halben Seitenlängen (Attribut ``halfExtends``, Typ ``Dimension``) festgelegt.


.. _ebl-figures:

Paket "figures"
--------------

Im ``EMM.D2.figures``-Package werden die grundlegenden Figuren definiert, die zur Visualisierung von Domänenmodellelementen zur Verfügung stehen. 

Hier wird eine graphbasierte Darstellungsform vorausgesetzt, das heißt, dass hier die speziell dafür benötigten Concepts bereitgestellt werden. 

Das Package wird durch zwei abstrakte Basistypen, ``EditorElement`` und ``SceneryObject`` strukturiert. 

``EditorElement`` ist der Basistyp aller Graphelemente, welche sich wiederum in Kanten (``Edge``) und Knoten (``Node``) aufteilen.

Jedes ``EditorElement`` muss das Attribut ``modelElementFQN`` setzen, dass den voll qualifizierten Namen des repräsentierten Domänenkonzeptes angibt. 
Dadurch wäre es prinzipiell möglich, einem Domänenkonzept mehrere Repräsentationen im Editor zuzuweisen, allerdings wird in der aktuellen Implementierung davon ausgegangen, dass eine 1:1-Beziehung zwischen den Concepts besteht.
Über das Attribut ``interactionAllowed`` lässt sich festlegen, ob eine Interaktion mit dem Modellelement durch den Benutzer erlaubt ist. Dies ist standardmäßig für alle Element auf "true" gesetzt.

Das von ``ScalaMapping`` definierte Attribut ``scalaType`` legt für Concepts in diesem Package fest, durch welche Objekte diese konkret im Modellierungswerkzeug grafisch dargestellt werden. 
Es ist zu beachten, dass die Interpretation von ``scalaType`` hier nicht den :ref:`scalamapping` angegebenen Konventionen folgt und der Wert kein Klassenname sein muss, obwohl kein TypeConverter angegeben wird. 

Wie die Werte interpretiert werden ist später unter :ref:`beispiel-neue-modellfigur` zu sehen.
    
Knoten
^^^^^^

Das abstrakte Basis-Concept aller Knoten, ``Node`` definiert die Attribute ``dim`` (Typ ``Dimension``), ``pos`` (``Position``) und ``rotation`` (``Rotation``), die dazu benutzt werden, sowohl das Erscheinungsbild als auch das physikalische Verhalten zu beschreiben.

In der Implementierung wird sichergestellt, dass Visualisierung und physikalische Repräsentation immer zueinander passen. 
Das bedeutet beispielsweise, dass die für den Benutzer sichtbare Ausdehnung genau die ist, die auch für die Erkennung von Kollisionen oder bei der Auswahl von Elementen durch ein Eingabegerät genutzt wird.

Für die Visualisierung von Knoten sind ein texturierter (``TexturedNode``) und ein beschrifteter (``TextLabelNode``) Basistyp vorgesehen, die folgende Attribute definieren:

    * TexturedNode: 

      * ``texture``: Pfad zu einer Bilddatei, die auf dem Knoten angezeigt wird.\ [#f5]_
      * ``backgroundColor``: Hintergrundfarbe des Knoten. 

    * TextLabelNode:

      * ``displayAttrib``: Gibt den Namen eines Attributs aus dem zugeordneten Domänenkonzepts an, dessen textuelle Darstellung als Schrift auf dem Knoten angezeigt wird.
      * ``fontColor``: Schriftfarbe, als Color-Instanz spezifiziert. 
      * ``backgroundColor``: Hintergrundfarbe, die an nicht von der Schrift abgedeckten Stellen angezeigt wird.
      * ``font``: Schriftart, angegeben als ``Font``-Instanz

Es wird davon ausgegangen, dass für Knoten im Domänenmodell das Typ-Verwendungs-Konzept genutzt wird. Siehe :ref:`\ <pmm>`.
Wie in :ref:`ipm3d-gui` erwähnt sollen verfügbare Knotentypen in einem Menü angezeigt werden, dass die Erstellung von neuen Modellelementen erlaubt. 

Im Kontext des Typ-Verwendungs-Konzepts werden Knotentypen ebenfalls "Typ" genannt, die konkreten Modellelemente, die in einem Modell genutzt werden, stellen "Verwendungen" der vorher definierten Typen dar.

Daher müssen alle Nodes folgende Attribute setzen:

  * ``toolingAttrib``: Legt fest, welches (String)-Attribut aus dem Domänenkonzept zur Identifikation des Node-Typs in einer Palette angezeigt werden soll.
  * ``toolingTitle``: Hierdurch wird angegeben, unter welcher "Überschrift" ein Node-Typ in einer Palette einsortiert werden soll. 
    Diese "Überschriften" korrespondieren mit den Knotentypen, die im Domain-Meta-Model definiert werden.

.. _ebl-figures-kanten:

Kanten
^^^^^^

Für Kanten stehen ein einfarbiger (``ColoredLine``) und ein texturierter Basistyp (``TexturedLine``) zur Verfügung. 

``TexturedLine`` bietet die gleichen Attribute wie ``TexturedNode`` an; bei ``ColoredLine`` muss die Grundfarbe gesetzt werden (``color``)
Zusätzlich muss bei beiden noch eine spekulare Farbe\ [#f3]_, ``specularColor`` angegeben werden.

Bei Kanten wird davon ausgegangen, dass das Typ-Verwendungs-Konzept im Domänenmodell nicht zum Einsatz kommt und Verbindungen direkt instanziiert werden. 

Wie Kantentypen innerhalb der grafischen Benutzeroberfläche bezeichnet werden sollen wird durch das Attribute ``toolingName`` festgelegt.

In Concepts, die Kantentypen repräsentieren müssen außerdem die Attribute von Knotentypen aus dem Domänenmodell angegeben werden, denen die Domain-Concepts der zugehörigen Verbindungen zugewiesen werden.
``InboundAttrib`` legt den Namens des Attributs fest, dem eingehende Kanten zugewiesen werden; ``outboundAttrib`` ist entsprechend das Attribut für die ausgehenden Kanten.

Außerdem sind für Kanten noch die beiden Attribute ``startNode`` und ``endNode`` definiert, denen im Editor-Usage-Model das Editor-Concept zugewiesen wird, das den Ausgangs- beziehungsweise den Endknoten darstellt.

Szenenobjekte
^^^^^^^^^^^^^

Typen für Szenenobjekte werden vom Basistyp SceneryObject abgeleitet. Wie für Knoten werden Attribute für die Position, Größe und Rotation definiert.
Wie der Typ innerhalb der grafischen Benutzeroberfläche bezeichnet werden soll wird durch das Attribut ``toolingName`` festgelegt.

Für Szenenobjekte kann eine Physikrepräsentation (Typ ``PhysicsSettings``) definiert werden, falls diese nicht anderweitig festgelegt wird.

Es gibt momentan nur eine Art von Szenenobjekten, das ``ColladaSceneryObject``. Über das Attribut ``modelPath`` kann ein Pfad zu einer COLLADA-Datei\ [#f7]_ angegeben werden.
Eine Physikdefinition innerhalb des COLLADA-Modells wird nicht unterstützt. 

Daher muss für ColladaSceneryObjects im Modell eine Physikrepräsentation gesetzt werden wenn die Objekte bei der Kollisionsberechnung berücksichtigt werden sollen und Selektion durch den Benutzer möglich sein soll.

Näheres zur COLLADA-Unterstützung in I>PM3D lässt sich bei :cite:`uli` nachlesen.

.. _edl:

Editor-Definition-Level
=======================

Auf dieser Ebene sind die Concepts zu finden, die die Repräsentationen für Knoten und Kanten aus dem Prozessmodell darstellen. 
Da hier nur Werte gesetzt und keine neuen Attribute definiert werden, wird hier auf eine gesonderte Beschreibung verzichtet.
Eine Auswahl der hier definierten Concepts kann in :ref:`anhang-b` nachgelesen werden. 
Das Aussehen einiger hier spezifizierter Figuren wird im nächsten Kapitel :ref:`visualisierung` gezeigt.

.. _pmm:

Prozess-Meta-Modell
===================

Das in dieser Arbeit verwendete Domain-Metamodell (**PMM** genannt) orientiert sich an den Metamodellen für die :ref:`POPM<popm>` , wie sie in :cite:`volz_werkzeugunterstutzung_2011` vorgestellt werden.

Das Prozess-Metamodel definiert nur ein Paket, ``processLanguage``, welches sich auf Ebene ``M2`` befindet. 

Die einzelnen Perspektiven sind als abstrakte Basis-Concepts definiert, die ``Perspective`` erweitern. 
:num:`Abbildung #pmm-hierarchie` zeigt die Concept-Hierarchie, die sich unterhalb von ``Perspective`` aufspannt.

.. _pmm-hierarchie:

.. figure:: _static/diags/pmm-hierarchie.eps
    :width: 16cm

    Perspektiven-Hierarchie im Prozess-Meta-Modell

``Node`` gehört zur funktionalen Perspektive, davon sind wiederum ``Process`` und ``FlowElement`` abgeleitet.
``Process`` stellt einen Prozess im Sinne der POPM dar.
Von ``FlowElement`` sind Kontrollflusselemente wie Konnektoren (``AndConnector``, ``OrConnector``) und Entscheidungsknoten (``Decision``) abgeleitet.

Die Datenperspektive teilt sich auf in ``DataItem``, welches einzelne Dateneinheiten repräsentiert, und in ``DataContainer`` , der ``DataItems`` zu einer Gruppe zusammenfasst. 

Die bisher genannten Concepts bzw. Perspektiven lassen sich als Knoten des Prozessgraphen interpretieren. 
Die verhaltensorientierte Perspektive hingegen — vertreten durch ``ControlFlow`` – lässt sich als Kante betrachten, welche ``Nodes`` miteinander verbindet.

``DataItems`` können über (gerichtete) Datenflüsse (``DataFlow``) miteinander verbunden werden.
``DataContainer`` ist gleichzeitig Teil der funktionalen Perspektive und kann daher über Kontrollflüsse mit anderen Nodes verbunden werden.

Im Unterschied zu den von :cite:`volz_werkzeugunterstutzung_2011` definierten Metamodellen werden Beziehungen zwischen Knoten immer mittels expliziter Verbindungs-Concepts spezifiziert, die in der Editor-Repräsentation auf Kanten abgebildet werden.
Ein ``DataItem`` wird beispielsweise über eine ``NodeDataConnection`` an einen ``Node`` angebunden.
:num:`Abbildung #pmm-conn` zeigt beispielhaft, auf welche Weise Kanten wie ``NodeDataConnection`` und ``ControlFlow`` mit Knoten assoziiert sind.

.. _pmm-conn:

.. figure:: _static/diags/pmm-conn.eps
    :width: 16cm

    Die Kanten ControlFlow, NodeDataConnection und deren Assoziationen

Das vollständige Prozess-Meta-Modell, wie es im Prototypen genutzt wird, kann in :ref:`anhang_pmm` nachgelesen werden.

.. _beispiel-neues-element:

Anwendungsbeispiel: Hinzufügen eines neuen Modellelements
=========================================================

Zur Verdeutlichung des bisher Gesagten soll hier gezeigt werden, wie ein neues Sprachelement zum Prozess-Meta-Modell hinzugefügt werden kann. 
Anschließend wird die dazugehörige Repräsentation im Editor-Meta-Modell ergänzt.

Änderungen am Prozess-Metamodell
--------------------------------

Im Prozess-Metamodell fehlt bisher die Möglichkeit, die operationsbezogene Perspektive der :ref:`POPM<popm>` abzubilden. 
Ein Operations-Element soll durch einen Knoten dargestellt werden, der sich einem Prozess zuordnen lässt.


Die folgenden Änderungen erfolgen im Package ``PMM.M2.processLanguage``.

Zuerst wird die Verbindung zwischen Prozessknoten und dem neuen Operationsknoten hinzugefügt:

.. code-block:: java

    concept ProcessOrgConnection extends Connection {  }

Anschließend wird der Knoten definiert:

.. code-block:: java

    concept OrganizationalPerspective extends Perspective {
        string name;
        0..* concept ProcessOrgConnection inboundProcessOrgConnection;
    }

Das Attribut ``name`` kann später vom Modellierungswerkzeug ausgelesen und verändert werden.
``InboundProcessOrgConnection`` drückt aus, dass dieser Knoten Endpunkt einer ``ProcessOrgConnection`` sein kann. 

Abschließend muss die Verbindung noch im Prozessknoten bekannt gemacht werden:


.. code-block:: java

    concept Process extends Node {
        0..* concept ProcessOrgConnection outboundProcessOrgConnection;
        // weitere Attribute ...
    }

Ein ``Process`` kann somit der Startpunkt einer solchen Verbindung sein.


Änderungen am Editor-Metamodell
-------------------------------

Der soeben definierte Organisationsknoten soll durch eine Pyramide dargestellt werden, auf deren Seiten der Wert des Attributs ``name`` zu lesen ist.
Bisher gibt es noch kein Basis-Concept für eine beschriftete Pyramide, also wird diese zum package ``figures`` im *Editor-Base-Level* (``EMM.M2.figures``) hinzugefügt:

.. code-block:: java

    concept TextPyramid extends TextLabelNode {
        scalaType = "test.TextPyramid";
    }

TextLabelNode stellt schon alle für einen Text-Knoten benötigten Attribute bereit; daher muss in diesem Concept nur noch der Typ des Grafikobjektes angegeben werden.
Wie ein passendes Grafikobjekt erstellt werden kann wird in einer Fortsetzung dieses Beispiels unter :ref:`beispiel-neue-modellfigur` gezeigt nachdem die Grundlagen dafür erläutert worden sind.

Auf dem Editor-Definition-Level (EMM.M2) kann nun die Repräsentation für den Organisationsknoten-Typen als Instanz der TextPyramid im package ``nodeFigures`` definiert werden. 

Als Vorlage wird das vorhandene Concept ``Process`` genutzt. 
In folgendem Code werden nur notwendige Änderungen gezeigt; die restlichen Zuweisungen können belassen oder nach eigenem "Geschmack" gesetzt werden.

.. code-block:: java

    TextPyramid OrganizationalNode {
        modelElementFQN = pointer PM.M2.processLanguage.OrganizationalPerspective;
        displayAttrib = "name";
        toolingAttrib = "name";
        toolingTitle = "Organizational Unit";
        // weitere Attribute ...
    }

Die unter :ref:`ebl-figures` erläuterten Attribute werden hier noch einmal am konkreten Beispiel gezeigt:

    * ``modelElementFQN`` gibt das zugehörige Concept aus dem Prozess-Metamodell an, das weiter oben definiert wurde.
    * ``displayAttrib`` legt fest, dass das Attribut "name" jenes Concepts als Text angezeigt werden soll.

Knoten werden wie gesagt nach dem Typ-Verwendungs-Konzept erstellt. ``OrganizationalPerspective`` ist also ein "Metatyp", von dem im Modellierungswerkzeug erst konkrete Typen erstellt werden müssen.
Die Bezeichnung des Metatyps im Modellierungswerkzeug wird von der Zuweisung ``toolingTitle`` auf "Organizational Unit" festgelegt. 
Dagegen gibt ``toolingAttrib`` an, dass ein erzeugter Typ mit dem Wert seines "name"-Attributs benannt wird. 


Im nächsten Schritt wird eine Repräsentation für oben definierte Verbindung zwischen Prozess und Organisationsknoten im package ``connectionFigures`` definiert.
Als Vorlage dient das ``nodeDataEdge``-Concept.

.. code-block:: java

    ColoredLine ProcessOrgEdge {
        modelElementFQN = pointer PM.M2.processLanguage.ProcessOrgConnection;
        toolingName = "Process-Organizational Assoc";
        outboundAttrib = "outboundProcessOrgConnection";
        inboundAttrib = "inboundProcessOrgConnection";
        // weitere Attribute ...
    }

Der Wert von ``inboundAttrib`` entspricht dem Namen des Attributs im weiter oben definierten ``OrganizationalPerspective``-Concepts.
So wird dem dem Werkzeug mitgeteilt, dass eingehende Verbindungen im Domänenmodell dem Attribut "inboundProcessOrgConnection" zugewiesen werden sollen.


.. [#f1] Die Implementierung stellt momentan TypeConverter für verschiedene Simplex3D-Vektoren und Quaternionen sowie für die Klassen java.awt.Font und .Color zur Verfügung. Weitere TypeConverter können auf Basis des TypeConverter-Traits (Scala-Package mmpe.model.lmm2scala) definiert werden.

.. [#f2] Quaternionen erlauben eine kompakte Darstellung von Rotationen im 3D-Raum :cite:`www:quat`.

.. [#f3] "Spekulare Farbe" ist ein Begriff, der oft im Zusammenhang mit dem Phong-Lichtmodell benutzt wird und dort für die spiegelnden Anteile des zurückgeworfenen Lichts steht.

.. [#f5] Unterstützt werden PNG, JPEG, BMP und TGA

.. [#f7] COLLADA ist ein XML-Austauschformat für 3D-Modelle und weitere Aspekte (Physik, Szenenbeschreibungen etc.) :cite:`www:collada`
