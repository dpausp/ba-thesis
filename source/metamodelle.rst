*****************************
Spezifikation der Metamodelle
*****************************

Nachdem im vorherigen Kapitel der Aufbau der Modellschichten sowie grundsätzliche Aspekte zu Metamodellen und den Usage-Modellen besprochen wurden, werden hier die in I>PM3D verwendeten Metamodelle genauer vorgestellt.
Bei der Beschreibung des Editor-Model-Stacks wird hier vor allem auf die Aspekte Wert gelegt, die prinzipiell unabhängig von der gewählten Modellierungsdomäne sind. 

Die Aspekte, die speziell die Visualisierung von Prozessmodellen betreffen werden im nächsten Kapitel :ref:`konzept-visualisierung` dargestellt.

Spezifikation der Editor-Meta-Modelle
=====================================

Das Editor-Metamodell enthält alle Informationen, die für die Spezifikation eines Editors und damit auch der Visualisierung eines Domänenmodells nötig sind.

Das vollständige Editor-Metamodell, wie es im realisierten Protoypen im Einsatz ist, kann in :ref:`anhang_emm` nachgelesen werden.

.. _emm-scalamapping:

Scala-Mapping
-------------

Die oberste Ebene des Editor-Model-Stacks beinhaltet nur ein Paket ("base") mit einem einzelnen Konzept, ScalaMapping. 

Dieses Konzept definiert Attribute, die festlegen, wie Konzepte aus dem Modell auf die verwendete Programmiersprache, hier Scala, abgebildet werden, um im Modellierungswerkzeug genutzt werden zu können.

Für jedes Konzept, das sich auf den weiter unten liegenden Ebenen befindet muss das Attribut "scalaType" definiert werden, das den korrespondierenden Scala-Typ angibt. 
Optional ist das Attribut "typeConverter", welches eine Klasse spezifiziert, die dazu genutzt wird, ein LMM-Konzept in ein passendes Scala-Objekt umzuwandeln und umgekehrt. 

.. _emm-base:

Editor-Base-Model
-----------------

Die Ebene D2 ist als Instanz der Ebene D3 definiert. Daraus folgt, dass alle hier definierten Konzepte Instanzen von ScalaMapping sein müssen.

.. _emm-types:

Paket types
^^^^^^^^^^^

Das *types*-Package definiert grundlegende Typen, die Visualisierungsparameter von Objekten und die Positionierung im Raum sowie deren Größe beschreiben.

Dazu werden folgeden Typen angeboten:

  * Dimension, Position: Spezifikation der Größe und der Position eines Objektes im dreidimensionalen Raum, welche in einem kartesischen Koordinatensystem angegeben werden.
    Die drei Attribute x, y, z werden im Editor auf einen Vektor mit 3 Komponenten abgebildet. Hierfür wird der Vektortyp *Vec3* von :ref:`Simplex3D` angeboten.

  * Rotation: Angabe der Rotation mittels eines Quaternions. Quaternionen erlauben die kompakte Darstellung von Rotationen im 3D-Raum :ref:`quaternions`.\ [#f1]_
    Die vier Attribute x0, x1, x2 und x3 werden auf ein Quaternionen-Objekt *Quat4*  abgebildet, das ebenfalls von Simplex3D bereitgestellt wird.

  * Color: Hiermit lassen sich Farben, die mittels im RGBA-Farbsystem als rot, grün, blau und alpha (Transluzenzfaktor) angegeben werden.
    Zu beachten ist hier, dass die Farben als Gleitkommazahl angegeben werden und einen Wertebereich von 0-1 abdecken.
    Dieser Typ wird auf die java.awt.Color-Klasse gemappt.

  * Font: Definiert Parameter für die Schriftdarstellung. Die Attribute und deren erlaubte Werte orientieren sich hierbei an der java.awt.Font-Klasse, auf die dieses Konzept auch abgebildet wird.
    Die Schriftfarbe muss separat mittels des Color-Typs angegeben werden.

        * face: Name der Schriftart
        * size: Größe, als Ganzzahl angegeben
        * style: Name des Schriftstils. Erlaubt sind hier "normal", "bold" und "italic", die den Werten der Enumeration FontStyle von java.awt entsprechen.


  * PhysicsSettings: Unterkonzepte dieses abstrakten Konzept werden genutzt, um Objekten eine physikalische Repräsentation zu geben, wenn diese nicht auf anderem Wege definiert wurde 

    Es werden kugel- (*PhysSphere*) und quaderförmige (*PhysBox*) Geometrien definiert, wie sie von der von :ref:`simulator-x` bereitgestellten Physikkomponente angeboten werden.
    Für eine *PhysSphere* muss der Radius angegeben werden; eine *PhysBox* wird analog über die halben Seitenlängen (Attribut *halfExtends*, Typ *Dimension*) festgelegt.

"""

.. _emm-figures:

Paket figures
^^^^^^^^^^^^^

Im *figures*-Package werden die grundlegenden Figuren definiert, die zur Visualisierung von Domänenmodellelementen zur Verfügung stehen. Hier wird eine graphbasierte Darstellungsform vorausgesetzt, das heißt, dass hier die speziell dafür benötigten Konzepte bereitgestellt werden. Die auf dieser Ebene definierten Konzepte sind prinzipiell von der Prozessmodellierung unabhängig, orientieren sich aber an deren Bedürfnissen.

Das Package wird durch 2 abstrakte Basistypen, EditorElement und SceneryObject strukturiert. 

*EditorElement* ist der Basistyp aller Graphmodellelemente, welche sich wiederum in Kanten (*Edge*) und Knoten (*Node*) aufteilen.

Jedes *EditorElement* muss das Attribut *modelElementFQN* setzen, dass den voll qualifizierten Namen des repräsentierten Domänenkonzeptes angibt. Dadurch wäre es prinzipell möglich, einem Domänenkonzept mehrere Repräsentationen im Editor zuzuweisen, allerdings wird in der aktuellen Implementierung davon ausgegangen, dass eine 1:1-Beziehung zwischen den Konzepten besteht.
    

Für die Visualisierung von Knoten sind ein texturierter (TexturedNode) und ein beschrifteter (TextLabelNode) Basistyp vorgesehen, die folgende Attribute definieren:

    * TexturedNode: 

      * texture: Pfad zu einer Bilddatei, die auf dem Knoten angezeigt wird. Näheres zu unterstützten Formaten lässt sich in :ref:`implementierung` nachlesen.
      * backgroundColor: Hintergrundfarbe des Knoten. Die Interpretation ist von der Implementierung der Visualisierung des Knotens abhängig.

    * TextLabelNode:

      * displayAttrib: Gibt den Namen eines Attributs aus dem zugeordneten Domänenkonzepts an, dessen textuelle Darstellung als Schrift auf dem Knoten angezeigt wird.
      * fontColor: Schriftfarbe, als Color-Instanz spezifiziert. 
      * backgroundColor: Hintergrundfarbe, die an nicht von der Schrift abgedeckten Stellen angezeigt wird oder bei Transluzenz-Effekten mit der Schriftfarbe gemischt wird.
      * font: Schriftart, als Font-Instanz

Es wird davon ausgegangen, dass für Knoten im Domänenmodell das Typ-Verwendungskonzept genutzt wird. Siehe :ref:`domaenenmodell`.
Wie in :ref:`ipm3d-gui` erwähnt sollen verfügbare Knotentypen in einem Menü angezeigt werden, dass die Erstellung von neuen Modellelementen erlaubt. 

Im Kontext des Typ-Verwendungskonzepts werden Knotentypen ebenfalls "Typ" genannt, die konkreten Modellelemente, die in einem Modell genutzt werden, stellen "Verwendungen" der vorher definierten Typen dar.

Daher müssen Nodes folgende Attribute setzen:

  * toolingAttrib: Legt fest, welches (String)-Attribut aus dem Domänenkonzept zur Identifikation des Node-Typs in einer Palette angezeigt werden soll.
  * toolingTitle: Hierdurch wird angegeben, unter welcher "Überschrift" ein Node-Typ in einer Palette einsortiert werden soll. 
    Diese "Überschriften" korrespondieren mit den Knotentypen, die im Domain-Meta-Model definiert werden.


Für Kanten stehen ein einfarbiger (*ColoredLine*) und ein texturierter Basistyp (*TexturedLine*) zur Verfügung. 

*TexturedLine* bietet die gleichen Attribute wie *TexturedNode* an; bei *ColoredLine* muss die Grundfarbe gesetzt werden (**color**)i
Zusätzlich muss bei beiden noch eine spekulare Farbe[#f2]_ (**specularColor**) angegeben werden.

Bei Kanten wird davon ausgegangen, dass das Typ-Verwendungskonzept im Domänenmodell nicht zum Einsatz kommt und Verbindungen direkt instanziiert werden. 

Wie Kantentypen innerhalb der grafischen Benutzeroberfläche bezeichnet werden sollen wird durch das Attribute *toolingName* festgelegt.


In Konzepten, die Kantentypen repräsentieren müssen außerdem die Attribute von Knotentypen aus dem Domänenmodell angegeben werden, denen die Konzepte der zugehörigen Verbindungen zugewiesen werden.

  * inboundAttrib: 
  * outboundAttrib: Legen die Namen der Attribute im Domänenmodell fest, 

Außerdem sind für Kanten noch die beiden Attribute **startNode** und **endNode** definiert, denen im *Editor-Usage-Model* das Editor-Concept zugewiesen wird, das den Ausgangs- beziehungsweise den Endknoten darstellt.

Szenenobjekte werden vom Basistyp SceneryObject abgeleitet. In dieser Kategorie stehen momentan nur Objekte zur Verfügung, die aus einer COLLADA-Datei geladen werden.
Für Szenenobjekte kann eine Physikrepräsentation definiert werden.

Details zur Visualisierung und den zur Verfügung stehenden grafischen Objekten sind im nächsten Kapitel :ref:`konzept_visualisierung` zu finden.


Die Typen Dimension, Position und Rotation werden benutzt, um das Erscheinungsbild sowie das physikalische Verhalten zu beschreiben, das bei der Erkennung von Kollisionen sowie bei der Auswahl von Elementen mittels Eingabegerät eine Rolle spielt. 
In der Implementierung wird sichergestellt, dass die Visualisierung und physikalische Repräsentation 

Editor-Definition-Model
-----------------------

Auf dieser Ebene sind die Concepts zu finden, die die Repräsentationen für Knoten und Kanten aus dem Prozessmodell darstellen. Das dies speziell die Visualisierung von Prozessmodellen betrifft wird hier auf eine genauere Beschreibungverzichtet.
Die zugehörigen Concepts können in :ref:`anhang-a` nachgelesen werden.


Prozess-Meta-Modell
===================

In dieser Arbeit wird ein Metamodell verwendet, das sich an den Metamodellen für die perspektivenorientierten Prozessmodellierung orientiert, wie sie in :cite:`volz_werkzeugunterstuetzung_2011` definiert worden sind.

Das Prozess-Metamodel definiert nur ein Paket, *processLanguage*. 
Hier findet sich die Idee der perspektivenorientierten Prozessmodellierung wieder, Prozessmodelle in verschiedene Perspektiven einzuteilen

Nodes gehören beispielsweise zur funktionalen Perspektive, während Kontrollflüsse Nodes verbinden und der Verhaltensperspektive zugeordnet werden. Dies drückt sich im Metamodell durch die Vererbungshierarchie der Konzepte aus.

Im Unterschied zu den Metamodellen von POPM müssen Beziehungen zwischen Knoten mit Hilfe von Connections spezifiziert werden. Dies wurde . Näheres dazu unter :ref:`konzept_visualisierung`
Ein DataItem muss also beispielsweise über eine NodeDataConnection an Prozess- oder Entscheidungsknoten angebunden werden.


Das vollständige Prozess-Meta-Modell, wie es im Protoypen genutzt wird, kann in :ref:`anhang_pmm` nachgelesen werden.


