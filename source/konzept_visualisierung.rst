.. _konzept_visualisierung:

***************************************
Konzept 3D-Visualisierung von Prozessen
***************************************

Nachdem die allgemeinen und prinzipiell von der Prozessmodellierung unabhängigen Teile des Editor-Meta-Models in den beiden vorherigen Kapiteln besprochen wurden, wird hier nun die Visualisierung von Prozessen im I>PM3D-Prototypen vorgestellt. 

Anschließend werden darauf aufbauend bisher nicht umgesetzte Erweiterungsmöglichkeiten vorgeschlagen, die für eine höhere Benutzerfreundlichkeit und Verständlichkeit sorgen könnten.

Anwender, die bereits Erfahrung mit verbreiteten grafischen 2D-Prozessmodellierungssprachen haben sollten durch das Aussehen der Modellelemente möglichst intuitiv verstehen können, welche Konzepte aus der Prozessmodellierung dadurch dargestellt werden. 

.. sollte man das als Anforderung definieren?

Wie im vorherigen Kapitel unter :ref:`emm-base` erläutert werden im Editor-Base-Model grundlegende Figuren und deren Darstellung durch grafische Objekte im Modellierungswerkzeug definiert.

Die konkreten Repräsentationen für bestimmte Typen aus dem Prozessmodell werden im Editor-Definition-Model festgelegt. 

Durch die Metamodelle wurde schon vorgegeben, dass ein graphbasierter Visualisierungsansatz genutzt wird. Wie nun Knoten und Kanten konkret dargestellt werden ist Gegenstand der folgenden Abschnitte.


Darstellung von Knoten
======================

Für die Darstellung von Informationen auf den Knoten wurden gibt es durch die im :ref:`emm-base` definierten *TextLabelNode* und *TexturedNode* grundsätzlich zwei Möglichkeiten.

Die Beschriftung von TextLabelNodes kann dazu verwendet werden, Attribute aus dem Prozessmodell direkt anzuzeigen, beispielsweise die Funktion eines Prozesses oder den Namen einer Dateneinheit. 
Ein Beispiel für zwei Prozessknoten ist in :num:`prozessknoten` zu sehen.

.. BILD

Andererseits können Grafiken (Texturen) genutzt werden, die Bedeutung eines Knotentyps zu visualisieren. So steht ein Pluszeichen beispielsweise für einen AND-Connector, wie in :num:`Abbildung #and-connector` gezeigt wird. 

.. BILD

Durch die freie Beweglichkeit der Kamera und der Objekte (siehe :ref:`ipm3d-visualisierung`) ergeben sich sehr unterschiedliche Beobachtungsperspektiven. So können die Entfernungen zwischen Kamera und Modellelementen sehr groß sein; auch können die Objekte durch die freien Rotationsmöglichkeiten von allen Seiten betrachtet werden. 

Daher ist es sinnvoll, möglichst große Schriften zu verwenden und für einen guten Kontrast zwischen Text- und Hintergrundfarbe zu sorgen.

Trotz der freien Drehbarkeit soll sichergestellt werden, dass Texte oder Symbole auf den Objekten jederzeit lesbar sind. Daher werden diese auf allen Seiten dargestellt. 

Jedoch führt dies bei bestimmten Drehpositionen zu recht unschönen Darstellungen, wenn beispielesweise bei einem Würfel zwei oder sogar drei Seiten zu sehen sind, die dasselbe anzeigen.

Um dies etwas zu verbessern werden die Seiten abhängig von Betrachtungswinkel dargestellt. Wird eine Seite vom Benutzer weggedreht, wird die Schrift oder Textur nach und nach ausgeblendet.
Ab einer gewissen Abweichung wird nur noch die Hintergrundfarbe angezeigt.

Näheres dazu siehe :ref:`implementierung`.


An die für Knoten verwendbaren geometrischen Objekte gibt es einige Anforderungen, die davon bestimmt sind, dass die Lesbarkeit und die Verständlichkeit des Prozessmodells möglichst hoch sein soll.

Für die Darstellung der Objekte wurden einfache, dreidimensionale geometrische Körper mit möglichst ebenen Seitenflächen wie Würfel oder Quader gewählt. 
Ebene Flächen eignen sich besonders gut zur Darstellung von Information; gekrümmte Flächen beeinträchtigen besonders die Lesbarkeit von (längeren) Textdarstellungen. 

Bei Würfeln oder ähnlichen Körpern ist es auch relativ einfach, einen (dreidimensionalen) Rahmen darzustellen, dessen Verwendung weiter unten in :ref:`visualisierungsvarianten` dargestellt wird.

Außerdem ist es sinnvoll, auf Quader oder annähernd quaderförmige Geometrien zu setzen, da die Knoten wie in :ref:`ipm3d-visualisierung` erwähnt in die physikalische Simulation eingebunden sind und Quader von der verwendeten Physik-Engine direkt unterstützt werden. 

Da dieser Prototyp neben der klassischen Desktop-Bedienung mit Maus und Tastatur auch zur Evaluierung von neuartigen Eingabegeräten eingesetzt werden soll müssen auch die Besonderheiten dieser Eingabemethoden berücksichtigt werden. 

Die hier verwendeten 3D-Eingabegeräte, die Kinect und WiiMote haben nur eine relativ begrenzte Genauigkeit bei der Auswahl und Platzierung von Objekten. 
Vor allem ungeübten Benutzern kann es durchaus schwer fallen, Objekte zu selektieren und zu bewegen, besonders wenn die Objekte relativ klein sind.

Daher sollten die Modellelemente möglichst groß sein\ [#f1]_ und eine geringe geometrische Komplexität\ [#f2]_ aufweisen damit die Arbeit mit dem Modell für den Benutzer nicht zu anstrengend wird.

Darstellung von Kanten
======================

Kanten sollen optisch leicht als Verbindungen zwischen zwei Knoten erkannt werden können. 

In I>PM3D werden Kanten werden durch einen (in y-Richtung) gestreckten 3D-Quader dargestellt, der vom Startknoten bis zum Endknoten reicht. Die Länge und Ausrichtung der Kanten wird automatisch angepasst, wenn die beteiligten Knoten im Raum verschoben werden. Dies wird von der von :cite:`uli` beschriebenen Editor-Komponente durchgeführt.

Die durch das Concept *TexturedConnection*  (:ref:`emm-base`) bereitgestellte texturierte Verbindung dient dazu, gerichtete Kanten zu visualisieren. 

Eine Möglichkeit ist es, eine Textur mit farblich vom Hintergrund abgehobenen Dreiecken zu verwenden, die so platziert ist, dass an zwei Ecken der Verbindung ein Pfeil entsteht.

:num:`Abbildung #gerichtete-verbindung` zeigt als Beispiel zwei Prozesse, die mit einem Kontrollfluss verbunden sind. Der Kontrollfluss läuft von Prozess A zu Prozess B.

.. BILD

Zusätzlich zu den Elementen des eigentlichen Prozessmodells gibt es noch die Möglichkeit, beliebige 3D-Modelle in die Szene einzufügen, die im Metamodell als *SceneryObject* bezeichnet werden. 

Solche Szenenobjekte können zum Beispiel dafür eingesetzt werden, Abbilder von realen Objekten anzuzeigen, die zur Illustration von Prozessschritten dienen. 


.. _visualisierungsvarianten:

Visualisierungsvarianten für interaktive Modelleditoren
=======================================================

Da das hier vorgestellte Visualisierungskonzept in einem interaktiven Modelleditor eingesetzt wird ergibt sich noch die weitere Anforderung, Visualisierungsvarianten der Modellelemente zu unterstützen.

So sollen Interaktionen des Benutzers mit den Modellobjekten sichtbar gemacht werden, indem die Visualisierung der Objekte temporär modifiziert wird. 
Diese Modifikationen werden nicht im Editor-Usage-Model abgelegt und gehen damit bei Beendigung des Programms verloren.

Nachdem ein Modell neu geladen wurde werden also alle Objekte im Normalzustand angezeigt.

Hervorhebung
------------

Diese Variante wird dafür eingesetzt, ein Objekt kurzzeitig beim Überfahren durch einem Cursor eines Eingabegeräts hervorzuheben. 
Dargestellt wird das abhängig von der Helligkeit der Grundfarbe des Objekts durch eine Aufhellung bzw. einer Abdunkelung der Farbe. Der Farbton wird dabei nicht verändert.

:num:`Abbildung #hervorhebung` zeigt im Vergleich einen hervorgehobenen und einen AND-Connector im Normalzustand (rechts).

Selektion
---------

Prozessmodellelemente und Szenenobjekte können durch den Benutzer ausgewählt werden. 
Selektierte Objekte sollen von unselektierten Objekten auch bei großer Entfernung und ungünstigen Blickwinkeln unterscheidbar sein, wobei aber jederzeit noch erkennbar sein muss, um welche Art von Modellelement es sich handelt. 

Die Visualisierung des Selektionszustandes soll daher möglich auffällig sein ohne das Erscheinungsbild allzu stark zu beeinflussen. 

Um die Selektion von der Hervorhebung unterscheidbar zu machen wird für die Selektion der Rand des Objekts in der Komplementärfarbe eingefärbt. Wie der "Rand" definiert ist je nach Objekttyp unterschiedlich.

In :num:`Abbildung #selektion` wird links ein Prozess und rechts ein AND-Connector im selektierten Zustand gezeigt.

Deaktivierung
-------------

Objekte können durch den Modelleditor deaktiviert werden. Welche Bedeutung dies hat wird vom Editor festgelegt. 
Zur Visualisierung dieses Zustandes wird das Objekt transluzent in einem Grauton dargestellt, der von der normalen Farbe abhängig ist. 

So kann man auch Elemente erkennen, die hinter dem deaktivierten liegen und von diesem verdeckt werden.

:num:`Abbildung #deaktivierung` zeigt einen deaktivierten Prozess, hinter dem sich ein anderer Prozess befindet.

Die drei vorgestellen Visualisierungsvarianten können frei kombiniert werden. Damit ist es zum Beispiel auch möglich, ein gleichzeitig hervorgehobenes, selektiertes und deaktiviertes Modellelement darzustellen.


Maßnahmen zu Verbesserung der Benutzerfreundlichkeit
====================================================

2D-Modellierungsflächen
-----------------------

Für eine übersichtliche Darstellung des Prozessmodells ist es häufig erwünscht, Elemente in einer bestimmten Weise anzuordnen. 

Durch die freie Positionier- und Drehbarkeit kann zwar prinzipiell jede beliebige geometrische Anordnung erreicht werden, doch ist dies mit einem relativ hohen Aufwand bei der Platzierung durch den Benutzer verbunden. 

Um das Platzieren zu vereinfachen werden in 2D-Modellierwerkzeugen oft im Hintergrund dargestellte Gitter genutzt, die eine optische Hilfe darstellen. 
Noch hilfreicher können "magnetische" Gitter sein, die grob in der Nähe platzierte Objekte automatisch auf feste, regelmäßige Positionen verschieben.

Eine ähnliche Technik war auch für den I>PM3D-Prototypen erwünscht. 

Da schon eine Physik-Engine integriert ist war es naheliegend, diese auch für die Platzierung von Objekten zu nutzen. 
Sobald sich ein Objekt nahe genug an einer solchen Modellierungsebene befindet, wird es nach dem Loslassen durch den Benutzer (Deselektion) von der "Gravitation" der Ebene angezogen, solange bis der Mittelpunkt des Objekts die Fläche erreicht hat, wo es angehalten wird.

Näheres zur Implementierung dieser "Gravitationsflächen" findet sich in :cite:`buchi`.

Grafisch werden diese Ebenen transluzent dargestellt, wobei darauf Gitterlinien zu erkennen sind. 
Diese Linien haben allerdings keine physikalische Bedeutung sondern diesen nur als optische Platzierungshilfe.

Beleuchtung
-----------

Für die Beleuchtung der Szene werden mehrere Lichtquellen eingesetzt. Die primäre Lichtquelle befindet direkt an der Kamera sich und bewegt sich mit dieser. 
Die Lichtfarbe ist weiß, also wird der Farbton der beleuchteten Objekte unverfälscht dargestellt. 

Zur Verbesserung der Orientierung befindet sich jeweils eine weniger intensive Lichtquelle an drei festen Position unterhalb, links und rechts der Szene. 
Dadurch ist es für den Benutzer leichter zu erkennen, welche Seite der Objekte nach unten, links beziehungsweise nach rechts zeigt. 

Hiermit soll vermieden werden, dass der Benutzer bei Rotationen der Kamera schnell die Orientierung verliert.

Die von der Renderbibliothek bereitgestellten Lichtquellen nach dem Phong-Lichtmodell sorgen für eine relativ realistische Beleuchtung bei vertretbarem Rechenaufwand.

Für die Visualisierung von 3D-Graphmodellen stellt sich die Frage, wie die Lichtparameter am besten gewählt werden sollten um eine möglichst hohe Lesbarkeit und eine gute Orientierung im Raum zu ermöglichen.

Im Phong-Lichtmodell wird das von einem Objekt reflektierte Licht in drei Beiträge unterschieden. 

Der "ambient"-Anteil (Umgebungslicht) ist unabhängig von der Ausrichtung des Objekts relativ zur Lichtquelle.

Üblicherweise wird der Hauptanteil des reflektierten Lichts vom "diffuse"-Anteil (diffuses Licht) beigesteuert. 
Dieser Beitrag ist abhängig vom Winkel zur Lichtquelle und ist für den räumlichen Eindruck besonders wichtig.

Der "specular-Anteil" erzeugt spiegelnde Reflexionen auf Objekten, die auch von der Betrachterposition relativ zum Objekt abhängen. 
Dieser Anteil kann deshalb die räumliche Orientierung unterstützen, was auch für die Darstellung der Prozessdiagramme hilfreich ist. 
Allerdings führt die starke Aufhellung an bestimmten Stellen dazu, dass sich vor allem Text dort schlecht ablesen lässt.

Außerdem kann bei den Lichtquellen noch angegeben werden, wie stark die Helligkeit mit steigender Entfernung von der Lichtquelle abfällt. 
Hierdurch kann ebenfalls den Tiefeneindruck und die räumliche Darstellung verbessert werden. 

Ein starker Abfall der Beleuchtung führt aber beispielsweise zu Problemen, wenn gleichzeitig Objekte mit Text in der Nähe der Lichtquelle und weit entfernt in lesbarer Form dargestellt werden sollen.
Objekte in der Nähe werden zu hell dargestellt, während weit entfernte Objekte zu dunkel sind.
Genauso ergibt sich bei gerichteten Verbindungen, die sich weit im Hintergrund befinden das Problem, dass die darauf abgebildeten Richtungsmarkierungen schlecht zu erkennen sind.

Insgesamt hat sich bei Versuchen gezeigt, dass es relativ schwierig ist, die Lichtparameter so zu setzen, dass eine in allen Situationen optimale Beleuchtung entsteht.

.. Konfigurierbarkeit?

Nicht umgesetze Erweiterungsmöglichkeiten
-----------------------------------------

Zur besseren Orientierung könnten noch andere Grafikeffekt genutzt werden, die jedoch im vorliegenden Prototypen noch nicht realisiert sind. Dazu gehört die Stereoskopie, Schattenberechnungen und die bereits erwähnte dynamische Transparent (->).


Eine andere Möglichkeit, den gerichteten Charakter einer Verbindung darzustellen wäre das Anzeigen einer dreidimensionalen Pfeilspitze am Ende der Linie oder innerhalb der Verbindung. 

Andere Varianten, um Kanten darzustellen: "Bezier-Röhren" :cite:`spratt_using_1994`

Benutzerstudie zur Darstellung von Verbindungen: :cite:`holten_user_2009`

Level of Detail: Anzeige automatisch vereinfachen bei weit entfernten Objekten, Text abkürzen (automatisch nach bestimmten Regeln oder Attribut für Abkürzung definieren)



Auf die in diesem Projekt realisierten geometrischen Ansatz, also Beziehungen zwischen Elementen durch grafisches Enthaltensein darzustellen wurde hier aus Gründen der einfacheren Implementierung verzichtet. Dies könnte später noch hinzugefügt werden jedoch ist dabei zu beachten, dass dies im 3D-Raum wohl deutlich schwieriger darzustellen und zu verstehen ist als in 2D-Diagrammen. [Beispiel? oder irgendein Beleg?]


.. [#f1] a

.. [#f2] b
