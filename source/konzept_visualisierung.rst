.. _konzept_visualisierung:

***************************************
Konzept 3D-Visualisierung von Prozessen
***************************************

Im Folgenden soll das Konzept zur dreidimensionalen Visualisierung von Prozessen im I>PM3D-Prototypen vorgestellt werden.

Grundsätzlich handelt es sich um einen graphbasierten Visualisierungsansatz. Prozessmodell-Konzepte wie Prozesse, Entscheidungsknoten und Daten werden als Knoten, Beziehungen wie Kontroll- oder Datenflüsse werden als Kanten dargestellt.

Das Aussehen der Modellelemente ist an die 2D-Repräsentation von Prozessen in IPM2 angelehnt. Auf die in diesem Projekt realisierten geometrischen Ansatz, also Beziehungen zwischen Elementen durch grafisches Enthaltensein darzustellen wurde hier aus Gründen der einfacheren Implementierung verzichtet. Dies könnte später noch hinzugefügt werden jedoch ist dabei zu beachten, dass dies im 3D-Raum wohl deutlich schwieriger darzustellen und zu verstehen ist als in 2D-Diagrammen. [Beispiel? oder irgendein Beleg?]


Für die Darstellung von Informationen auf den Knoten gibt es grundsätzlich zwei Möglichkeiten. Zum einen können mehrzeilige Texte, zum Anderen statische 2D-Grafiken (Texturen) auf den Seiten angezeigt werden. 
Texte können dazu verwendet werden, Attribute aus dem Prozessmodell anzuzeigen, beispielsweise den Namen oder die Funktion des Elements. 

An die für Knoten verwendbaren geometrischen Objekte gibt es einige Anforderungen, die davon bestimmt sind, dass die Lesbarkeit und die Verständlichkeit des Prozessmodells möglichst hoch sein soll.
Durch die freie Beweglichkeit der Kamera und der Objekte ergeben sich sehr unterschiedliche Beobachtungsperspektiven. So können die Entfernungen zwischen Kamera und Modellelementen sehr groß sein; auch können die Objekte durch die freien Rotationsmöglichkeiten von allen Seiten betrachtet werden.


Für die Darstellung der Objekte wurden einfache, dreidimensionale geometrische Körper mit ebenen Seitenflächen wie Würfel oder Quader gewählt. 
Ebene Flächen eignen sich gut zur Darstellung von Information; gekrümmte Flächen beeinträchtigen besonders die Lesbarkeit von Text. 

Für die Realisierung des Prototypen ist es daneben sinnvoll, auf Quader oder annähernd quaderförmige Geometrien zu setzen, da die Knoten in die physikalische Simulation eingebunden sind, um Selektion zu ermöglichen, Kollisionen zu erkennen oder Gravitationsebenen zu realisieren. Quader werden von der verwendeten Physik-Engine direkt unterstützt und sind somit einfach zu benutzen. 

Da dieser Prototyp neben der klassischen Desktop-Bedienung mit Maus und Tastatur auch zur Evaluierung von neuartigen Eingabegeräten eingesetzt werden soll müssen die Eigenschaften dieser Eingabemethoden berücksichtigt werden. Aktuelle 3D-Eingabegeräte  wie die Kinect oder WiiMote haben nur eine relativ begrenzte Genauigkeit bei der Auswahl und Platzierung von Objekten. Daher müssen die Modellelemente auch eine gewisse Größe und geringe geometrische Komplexität aufweisen damit die Arbeit mit dem Modell für den Benutzer nicht zu anstrengend wird.

Näheres zur Auswahl und Positionierung von Modellelementen durch Eingabegeräte dazu findet sich in (-> buchi, uli)


Verbindungen zwischen Knoten werden durch einen (in y-Richtung) gestreckten 3D-Quader dargestellt. Verbindungen können texturiert werden. Genutzt wird das im Prototypen, um die Richtung von gerichteten Kanten anzuzeigen. Dazu wird eine Textur mit farblich vom Hintergrund abgehobenen Dreiecken verwendet, die so platziert ist, dass an zwei Ecken der Verbindung ein Pfeil entsteht. [BILD]


Das Aussehen der Modellelemente soll über verschiedene visuelle Parameter beinflussbar sein. So soll bei allen Knoten die Hintergrundfarbe gewählt werden können; bei beschrifteten Knoten soll zusätzlich die Schriftart, -farbe und -stil konfigurierbar sein. Bei texturierten Knoten kann eine Datei angegeben werden, welche die darzustellende Grafik enthält.

Bei Verbindungen soll es möglich sein, die Dicke der gezeichneten 3D-Line zu bestimmen.


Trotz der freien Drehbarkeit soll sichergestellt werden, dass Texte oder Symbole auf den Objekten jederzeit lesbar sind. Daher werden diese auf allen Seiten dargestellt. 

Ab einem gewissen Winkel wird nur noch die Hintergrundfarbe angezeigt.

Zusätzlich zu den Elementen des eigentlichen Prozessmodells gibt es noch die Möglichkeit, beliebige 3D-Modelle in die Szene einzufügen. Diese Objekte werden als "scenery objects" bezeichnet. Diese Objekte können zum Beispiel dafür eingesetzt werden, Abbilder von realen Objekten anzuzeigen, die zur Illustration von Prozessschritten diesen können. 


.. _visualisierungsvarianten:

Visualisierungsvarianten für interaktive Modelleditoren
=======================================================

Da mit dieser Arbeit ein Visualisierungskonzept entwickelt wird, das innerhalb eines interaktiven Modelleditors eingesetzt werden soll gibt es noch zusätzliche Anforderungen.

So sollen die Modellelemente Interaktion unterstützen, die für den Benutzer sichtbar gemacht wird, indem die Visualisierung der Objekte temporär modifiziert wird. Diese Modifikationen werden nicht in einer persistenten Form abgelegt und gehen bei Beendigung des Programms verloren.

Hervorhebung
------------

Diese Variante kann beispielsweise dafür eingesetzt werden, ein Objekt kurzzeitig beim Überfahren mit einem Cursor hervorzuheben. Dargestellt wird das abhängig von der Helligkeit der Grundfarbe des Objekts durch eine Aufhellung bzw. einer Abdunkelung der Farbe. Der Farbton wird dabei nicht verändert.

Selektion
---------

Objekte können durch den Benutzer entweder direkt mit Hilfe von Eingabegeräten (zum Beispiel durch Anklicken mit der Maus) oder durch globale Befehle des Modelleditors (zum Beispiel "selektiere alles") ausgewählt werden. Selektierte Objekte sollen von unselektierten Objekten auch bei großer Entfernung und ungünstigen Blickwinkeln unterscheidbar sein, wobei aber jederzeit noch erkennbar sein muss, um welche Art von Modellelement es sich handelt. Die Visualisierung des Selektionszustandes soll daher möglich auffällig sein ohne das Erscheinungsbild allzu stark zu beeinflussen. 

Um die Selektion von der Hervorhebung unterscheidbar zu machen wird für die Selektion der Rand des Objekts in der Komplementärfarbe eingefärbt. Wie der "Rand" definiert ist wird von den Texturkoordinaten des Objekts bestimmt.  

Deaktivierung
-------------

Objekte können durch den Modelleditor deaktiviert werden. Welche Bedeutung dies hat wird vom Editor festgelegt. 
Zur Visualisierung dieses Zustandes wird das Objekt transluzent in einem Grauton dargestellt, der von der normalen Farbe abhängig ist. So kann man auch Elemente erkennen, die hinter dem deaktivierten liegen und von diesem verdeckt werden.

Diese 3 Varianten können frei kombiniert werden; also ist es zum Beispiel auch möglich, ein gleichzeitig hervorgehobenes, selektiertes und deaktiviertes Modellelement zu zeichnen.


Maßnahmen zu Verbesserung der Benutzerfreundlichkeit
====================================================

2D-Modellierungsflächen
-----------------------

Für eine übersichtliche Darstellung des Prozessmodells ist es häufig erwünscht, Elemente in einer bestimmten Weise anzuordnen. Durch die prinzipiell freie Positionier- und Drehbarkeit kann zwar prinzipiell jede beliebige geometrische Anordnung erreicht werden, doch ist dies mit einem relativ hoheren Aufwand bei der Platzierung durch den Benutzer verbunden. Um das Platzieren zu vereinfachen werden in 2D-Modellierwerkzeugen oft Gitter genutzt, die eine optische Hilfe darstellen. Noch hilfreicher können "magnetische" Gitter sein, die grob in der Nähe platzierte Objekte automatisch auf feste, regelmäßige Positionen verschieben.

Eine ähnliche Technik war auch für den I>PM3D-Prototypen erwünscht. Da schon eine Physik-Engine integriert ist war es naheliegend, diese auch für die Platzierung von Objekten zu nutzen. Sobald sich ein Objekt nahe genug an einer solchen Modellierungsebene befindet, wird es nach dem Loslassen durch den Benutzer (Deselektion) von der "Gravitation" der Ebene angezogen, solange bis der Mittelpunkt des Objekts die Fläche erreicht hat, wo es angehalten wird.

Näheres zur Implementierung der "Gravitationsflächen" findet sich in (-> buchi)

Grafisch werden diese Ebenen transluzent dargestellt, wobei darauf Gitterlinien zu erkennen sind. Die Dichte und Dicke der Linien kann konfiguriert werden.
Diese Linien haben allerdings keine physikalische Bedeutung sondern diesen nur als optische Platzierungshilfe.

Grafikeffekte
-------------

Die Szene wird von Lichtquellen beleuchtet, wobei die Lichtberechnungen nach dem (pixelgenauen) Phong-Verfahren durchgeführt werden. Dies führt zu einer relativ realistischen Beleuchtung bei vertretbarem Rechenaufwand.

Standardmäßig werden zwei Lichtquellen eingesetzt. Eine befindet direkt an der Kamera sich an der Kamera und bewegt sich mit dieser. Die Lichtfarbe ist weiß, also wird der Farbton der beleuchteten Objekte unverfälscht dargestellt. Zur Verbesserung der Orientierung befindet sich eine zweite, farbige Lichtquelle an einer festen Position unterhalb der Szene (ohne Rotation). Dadurch ist es möglich zu erkennen, welche Seite der Objekte nach unten zeigt. Das soll vermeiden, dass der Benutzer bei Rotationen der Kamera schnell die Orientierung verliert.


[BILD]

[Konfigurierbarkeit?]

Texte oder Symbole  werden auf den Objekten auf allen Seiten dargestellt. 
Das hat allerdings den Nachteil, dass die Information abhängig vom Rotationszustand mehrfach sichtbar sein kann, was für den Benutzer etwas verwirrend sein könnte und die Verständlichkeit des Modells senkt.  [BILD]

Um dieses Problem abzumildern wird jedoch die Anzeige von der Blickrichtung des Benutzers (der Kamera) abhängig gemacht. Das hat zur Folge, dass die Information nur auf der dem Benutzer zugewandten Seite mit hoher Intensität dargestellt wird. Zur Berechnung wird der Winkel bzw. das Skalarprodukt zwischen Kameravektor und der Normalen der jeweiligen Objektfläche herangezogen. Dessen Wert bestimmt, zu welchem Anteil die Vordergrundfarbe (Schriftfarbe bzw. Texturfarbe) zur Hintergrundfarbe gemischt wird und welchen Einfluss sie damit auf den endgültig sichtbaren Farbton hat hat. 

Ab einem gewissen Winkel wird nur noch die Hintergrundfarbe angezeigt.

Nicht umgesetze Erweiterungsmöglichkeiten
-----------------------------------------

Zur besseren Orientierung könnten noch andere Grafikeffekt genutzt werden, die jedoch im vorliegenden Prototypen noch nicht realisiert sind. Dazu gehört die Stereoskopie, Schattenberechnungen und die bereits erwähnte dynamische Transparent (->).


Eine andere Möglichkeit, den gerichteten Charakter einer Verbindung darzustellen wäre das Anzeigen einer dreidimensionalen Pfeilspitze am Ende der Linie oder innerhalb der Verbindung. 

Andere Varianten, um Kanten darzustellen: "Bezier-Röhren" :cite:`spratt_using_1994`

Benutzerstudie zur Darstellung von Verbindungen: :cite:`holten_user_2009`

Level of Detail: Anzeige automatisch vereinfachen bei weit entfernten Objekten, Text abkürzen (automatisch nach bestimmten Regeln oder Attribut für Abkürzung definieren)


Das ebenfalls für die Prozessmodellierung interessante Konzept der dynamischen Transparent von Modellobjekten, abhängig von deren Relevanz, wird von :cite:`elmqvist_dynamic_2009` vorgestellt. Es handelt sich hierbei um einen Lösungsansatz für das typische Problem der Verdeckung in der 3D-Visualisierung.

Die Grundidee ist hier, Objekte nach ihrer Wichtigkeit für die aktuelle Betrachtungssituation einzuteilen. Unwichtige, die Ansicht störende Objekte werden als "distractors", informationstragende Elemente als "targets" bezeichnet. Das Ziel ist nun, sicherzustellen, dass "targets" nie von "distractors" verdeckt werden können. Letztere werden, sobald sie wichtige Objekte verdecken transparent dargestellt, damit das relevante Element jederzeit erkannt werden kann. Dazu wird ein Algorithmus angegeben, der diesen Effekt in Echtzeit auf Sub-Objekt-Ebene berechnen kann.
