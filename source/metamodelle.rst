.. _metamodelle:

***********
Metamodelle
***********

Diese Arbeit konzentriert sich zwar auf die Visualisierung von Prozessmodellen, aber es wurde dennoch als vorteilhaft gesehen, die Visualisierung und die Modellanbindung möglichst flexibel zu gestalten. 
Das führt dazu, dass Modifikationen schnell vorgenommen werden könnnen und ein Experimentieren mit der Prozessvisualisierung einfach durchführbar ist. 

In späteren Arbeiten könnte das Konzept so erweitert werden, dass es wie das Model Designer Framework (MDF) sich auch für die Erstellung von Editoren für beliebige Domänenmodelle eignet.

I>PM3D folgt dabei dem Ansatz von MDF, die Visualisierung und die verfügbaren Modellelemente über Metamodelle zu definieren. Die Definition des Prozess-Metamodells wird analog zu MDF im Folgenden Domänen-Metamodell genannt.

Im Gegensatz zu MDF wird allerdings aus Gründen der einfacheren Implementierung das Graphical-Meta-Model, das die Visualisierungsparameter festlegt und das Editor-Metamodel, das die Verknüfung zwischen Domänenmetamodell und Graphical-Meta-Model herstellt zusammengelegt. Der Editor wird also nur über ein Editor-Meta-Modell definiert, das mit einem Domänen-Metamodell verbunden ist.
So können für ein Domänenmetamodell problemlos mehrere verschiedene Visualisierungen definiert werden.

Die Modelle werden mit Hilfe einer textuellen Modellierungssprache spezifiziert, die vom Linguistic Meta Model (LMM) der Open Meta Modeling Environment (OMME) abgeleitet ist. 

Die hier verwendete Sprache, im Folgenden LMMLight genannt folgt in vielen Aspekten LMM, ohne jedoch alle weiterführenden Modellierungsmuster zu unterstützen. 
Details zu LMMLight und der Implentierung finden sich im späteren Kapitel :ref:`implementierung_lmmlight`.

Die Sprache ist vollständig kompatibel zum textuellen Editor von OMME. Das heißt, dass gültige LMMLight-Modelle vom Editor nicht beanstandet werden. Dies ermöglicht ein einfaches Bearbeiten der Modelle ohne einen eigenen Editor entwickeln zu müssen.

Im Folgenden soll nun beschrieben werden, welche Anforderungen Editor-Metamodell und Domänenmetamodell erfüllen müsen und wie die für diese Arbeit verwendeten Metamodelle aussehen.

Editor-Metamodell
=================

Das Editor-Metamodell enthält alle Informationen, die für die Visualisierung eines Domänenmetamodells nötig sind.

Das vollständige Editor-Metamodell, wie es im realisierten Protoypen im Einsatz ist, kann in :ref:`anhang_emm` nachgelesen werden.

Ebene D3
--------

Die oberste Ebene des EMM beinhaltet nur ein Paket ("base") mit einem einzelnen Konzept, ScalaMapping. Dieses Konzept definiert Attribute, die festlegen, wie ein Konzept aus dem Modell auf einen entsprechenden Scala-Typ beziehungsweise auf ein Objekt davon abgebildet wird.
Jedes Konzept muss das Attribut "scalaType" setzen, das den korrespondierenden Scala-Typ definiert. Optional ist das Attribut "typeConverter", das eine Klasse spezifiziert, die dazu genutzt wird, ein LMM-Konzept in ein entsprechendes Scala-Objekt umzuwandeln und umgekehrt. 


Ebene D2
--------

Ebene D2 ist als Instanz der Ebene D3 definiert.

Das *types*-Package definiert grundlegende Typen wie beispielsweise Farbe, Schrift oder Konzepte zur Definition von Physikparametern für Szenenobjekte.

Im *figures*-Package werden die grundlegenden Figuren definiert, die zur Visualisierung von Prozessmodellelementen zur Verfügung stehen. 
Das Package wird durch 2 abstrakte Basistypen, EditorElement und SceneryObject strukturiert. 

EditorElement ist der Basistyp aller Prozessmodellelemente, welche sich wiederum in Kanten (edge) und Knoten (node) aufteilen.

Für die Visualisierung von Knoten sind ein texturierter (TexturedNode) und ein beschrifteter (TextLabelNode) Basistyp vorgesehen.

Nodes müssen folgende Attribute definieren:

  * toolingAttrib: legt fest, welches (String)-Attribut aus dem Domänenkonzept zur Identifikation des Node-Typs in einer Palette angezeigt werden soll.
  * toolingTitle: hierdurch wird angegeben, unter welcher Überschrift dieser Node-Typ in einer Palette einsortiert werden soll. 

Hier wird davon ausgegangen, dass für Knoten im Domänenmodell das Typ-Verwendungskonzept genutzt wird. Siehe :ref:`domaenenmodell`.

  Details zur Palette siehe :cite:`uli` und :cite:`buchi`

Kanten werden entweder durch eine farbige (ColoredLine) oder eine texturierte (TexturedLine) Linie dargestellt.

Kanten müssen folgende Attribute definieren:

  * toolingName: definiert, wie der Kantentyp in einer Palette genannt wird
  * inboundAttrib, outboundAttrib: legen die Namen der Attribute im Domänenmodell fest, denen die Konzepte der zugehörigen Verbindungen zugewiesen werden

Szenenobjekte werden vom Basistyp SceneryObject abgeleitet. In dieser Kategorie stehen momentan nur Objekte zur Verfügung, die aus einer COLLADA-Datei geladen werden.
Für Szenenobjekte kann eine Physikrepräsentation definiert werden.

Details zur Visualisierung und den zur Verfügung stehenden grafischen Objekten sind im nächsten Kapitel :ref:`konzept_visualisierung` zu finden.

.. _domaenenmodell:

Domänenmodell
=============

Das Domänen-Metamodell ist im prinzipiell frei wählbar. Für diese Arbeit kommt ein Metamodell zum Einsatz, das an die Metamodelle der perspektivenorientierten Prozessmodellierung wie sie in :cite:`volz_werkzeugunterstuetzung_2011` andefiniert wurden, angelehnt ist.

Es wird davon ausgegangen, dass das Modell einem graphbasierten Ansatz folgt. So können Knoten definiert werden, die mittels Kanten, gerichtet oder ungerichtet, miteinander verbunden sind.

Für die Erzeugung von Knoten im Domain-Usage-Modell muss das Typ-Verwendungs-Konzept verwendet werden. Das bedeutet, dass von Typen aus dem Domain-Meta-Model erst ein Typ im Domain-Usage-Model erzeugt werden muss, von dem dann eine Verwendung im Usage-Model erzeugt werden kann.

Für Kanten kommt das Typ-Verwendungs-Konzept im Domänenmodell nicht zum Einsatz. Kanten sind direkte Instanzen von Typen aus dem Domain-Meta-Modell.

Prozess-Meta-Modell
===================

Das hier für die Prozessmodellierung benutzte Metamodell definiert nur ein Paket, *processLanguage*. Hier findet sich die Idee der perspektivenorientierten Prozessmodellierung wieder, Prozessmodelle in verschiedene Perspektiven einzuteilen, wie in :ref:`popm` oder in :cite:`jablonski_perspective_2008` näher erläutert wird.

Nodes gehören beispielsweise zur funktionalen Perspektive, während Kontrollflüsse Nodes verbinden und der Verhaltensperspektive zugeordnet werden. Dies drückt sich im Metamodell durch die Vererbungshierarchie der Konzepte aus.

Im Unterschied zu den Metamodellen von POPM müssen Beziehungen zwischen Knoten mit Hilfe von Connections spezifiziert werden. Dies wurde . Näheres dazu unter :ref:`konzept_visualisierung`
Ein DataItem muss also beispielsweise über eine NodeDataConnection an Prozess- oder Entscheidungsknoten angebunden werden.


Das vollständige Prozess-Meta-Modell, wie es im Protoypen genutzt wird, kann in :ref:`anhang_pmm` nachgelesen werden.
