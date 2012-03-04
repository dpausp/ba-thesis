***************
Implementierung
***************

Render-Bibliothek
=================

Im Rahmen dieser Arbeit wurde eine an die Erfordernisse des vorher vorgestellten Prozessvisualisierungskonzeptes angepasste Render-Bibliothek auf Basis von OpenGL erstellt. 
Hier sollen nur die wichtigsten Eigenschaften dieser Bibliothek dargestellt werden. Für eine genauere Beschreibung sei auf (-> MMPERenderer-Doku-Projekt) verwiesen.

Die Render-Bibliothek, im Folgenden vereinfacht "Renderer" genannt, basiert auf der für die Java Virtual Machine verfügbare OpenGL-Anbindung LWJGL in der Version 2.8.2. Zur Implementierung der Grafikfunktionen wurden nur OpenGL-Funktionen genutzt, die in der Version 3.3 standardmäßig verfügbar sind. 
Insbesondere wurde auf im OpenGL-Standard "deprecated" markierte Funktionalität vollständig verzichtet. Damit ist zu erwarten, dass der Renderer auch mit den aktuellesten und zukünftigen OpenGL-Versionen in den nächsten Jahren kompatibel sein wird.


Grundlegende Konzepte 
---------------------

Drawable: Jedes Objekt, das vom Renderer dargestellt werden soll muss durch ein Objekt einer Klasse repräsentiert werden, die Drawable implementiert.

Diese Klassen bestehen aus einer Beschreibung der Geometrie (Mesh), Positions-Dimensions-Beschreibung (Transformation) und einer Darstellungsweise (Effect).

Drawables entstehen üblicherweise durch das Zusammenmischen von verschiedenen Traits. Im einfachsten Fall sind das drei traits, die Mesh, Transformation bzw. Effect implementieren. 

Durch diese Aufteilung lassen sich die einzelnen Aspekte leicht wiederverwenden, was eine sehr wichtige Anforderung an eine solche Bibliothek ist. 

Effects können recht kompliziert sein und sind selbst oft zusammengesetzt aus verschiedenen traits, die Teilfunktionalitäten implementieren und wieder einfache Wiederverwendbarkeit erlauben. Diese Traits werden mit der Endung -Addon benannt; beispielsweise existieren ein PhongLighting- und ein TextDisplayAddon.

Resourcen, die potenziell von vielen verschiedenen Drawables geteilt werden können werden im Drawable nur indirekt angegeben. Dies gilt für die Definition von Shaderprogrammen und Texturen. Die eigentlichen OpenGL-Objekte werden von den RenderStages verwaltet.

Für RenderStages existieren eine Reihe von Plugins, die spezielle Funktionen implementieren. So gibt es ein Plugin für die Textdarstellung oder für die Verwaltung von Lichtquellen.

Anbindung an das Gesamtsystem
-----------------------------

Um den Renderer für das Projekt I>PM3D nutzbar zu machen wurde eine Anbindung an den Simulator X erstellt. Prinzipiell ist die Renderbibliothek aber auch unabhängig von Simulator X für andere Projekte nutzbar.

Es ist vorgesehen, jedoch nicht implementiert, für die Darstellung der Szene mehrere Bildschirme nutzen zu können, wie es unter Anderem für ein CAVE-System nötig wäre. 

Der RenderConnector selbst ist als Simulator X-Component implementiert; die eigentliche Render-Arbeit wird von einem RenderActor übernommen, der vom RenderConnector gestartet wird. Für den RenderActor musste eine eigene Implementierung eines single-threaded Scala-Actor-Schedulers erstellt werden, da der Standard-Scheduler die Actor-Arbeitspakete in beliebig wechselnden Threads ausführen kann. Dies ist für die Ausführung von OpenGL-Funktionen problematisch. 

Im RenderActor sind 2 Render-Ebenen definiert; die zuerst gerenderte Ebene enhält alle 3D-Szenenelemente wie den Prozessgraphen und Scenery objects. Darüber wird eine Ebene gezeichnet, die 2D-Elemente wie beispielsweise Cursor für Eingabegeräte oder das Eightpen-Menü (-> buchi) enthalten kann.

Es gibt die Möglichkeit, den RenderActor durch Plugins um Funktionalität zu erweitern, was im Rahmen dieses Projekts für das Rendern der Nifty-GUI-Menüs (-> uli) und des Eightpen-Menüs (-> buchi) genutzt wird.

Bei der Anbindung wurde darauf geachtet, nach Möglichkeit schon bestehende Infrastruktur von Simulator X weiterzunutzen.

So unterstützt die Renderkomponente die von Simulator X defininierten Aspekte für die Definition von Lichtquellen-Entities sowie den Aspekt "ShapeFromFile", der die Renderkomponente anweist, neue grafischen Objekte aus einer COLLADA-Modelldatei zu laden.

Zusätzlich ist es möglich, den Aspekt "ShapeFromFactory" für die Definition von darzustellenden Entities zu nutzen.
Die Drawables, die die eigentliche grafische Repräsentation eines Modellelements darstellen können nun von einer Factory außerhalb des RenderActors geholt werden. 
Genutzt wird dies beispielsweise für die Erstellung aller Modellelemente.

SVarSupport
-----------

Um den Umgang mit den Drawables für Modellelemente zu vereinfachen wurden verschiedene traits erstellt, die das abstrakte trait SVarSupport implementieren. Damit lassen sich SVars direkt mit Attributen der Drawables verbinden.

In den SVarSupports werden in der Methode connectSVars für passende SVar-Typen Observe-Handler registriert. Im einfachsten Fall bestehen diese Handler nur aus einem Setter, der direkt Attribute des Drawables setzt sobald sich der Wert der SVar ändert.


Fähigkeiten
-----------

Hier eine Übersicht über die in dieser Arbeit genutzte Funktionalität:

* Darstellung von Text, auch mehrzeilig 
* Texturierung mit Unterstützung für Mipmapping und anisotropisches Filtern: für Text und Symbole auf Modellelementen
* Phong-Beleuchtungsmodell, pixelgenau
* Unterstützung für beliebig viele Lichtquellen
* Transluzenz auf Objektebene (deaktivierte Modellelemente)
* COLLADA-Loader
* COLLADA2Scala-Compiler
* Rendern in Texturen (render-to-texture, offscreen rendering): genutzt für Eightpen-Menü
* Unterstützung für deaktivierte, hervorgehobene und selektierte Elemente


Unterstützung für deaktivierte, hevorgehobene und selektierte Elemente
----------------------------------------------------------------------

Die drei Visualisierungsvarianten lassen sich einfach über entsprechende Properties setzen:

.. code-block:: scala

    drawable.disabled = false
    drawable.highlighted = false
    drawable.selectionState = DrawableSelectionState.Normal

DrawableSelectionState wurde als enum vorgesehen, damit in Zukunft weitere Selektionszustände unterstützt werden können. 

Die Properties werden nur an den Shader durchgereicht; die Auswahl der richtigen Visualisierungsparameter wird komplett innerhalb eine Shaderfunktion realisiert.

Zusätzlich können noch folgende Parameter eingestellt werden:

* borderWidth: Breite des Selektionsrahmens, von 0-1.
* highlightFactor: Wert, mit dem die berechnete Farbe multipliziert wird um Hervorhebung darzustellen. Bei dunklen Grundfarben wird mit 1 / highlightFactor multipliziert.

"Deaktiviert" wird durch einen Grauton dargestellt, der wie folgt aus den Komponenten der Grundfarbe berechnet wird: grauwert = (rot + blau + grün) * 0.2. 
Der Selektionsrahmen wird abhängig von der resultierenden Helligkeit von "grauwert" entweder hellgrau oder dunkelgrau dargestellt.

Darstellung von Text
--------------------

Für die Beschriftung von Prozessmodellknoten wurde eine gut lesbare und trotzdem einfach umsetzbare Technik für das Rendering von Schrift benötigt.
Hierfür wurde die 2D-API (java.awt) der Java-Klassenbibliothek zur Hilfe genommen. Zur Verwendung mit OpenGL wird die Schrift in eine Textur geschrieben, die dann auf die Objekte aufgebracht werden kann.
Zur Verbesserung der Darstellung wird die Antialiasing-Funktion von Graphics2D genutzt. 

Um auch bei größeren Entferungen von der Kamera und kleiner Schrift noch eine angemessene Lesbarkeit zu erreichen kann Mipmapping genutzt werden. Aufgrund von Problemen mit verschiedenen Grafikkarten ist das standardmäßig jedoch nicht aktiviert.

Um Text darstellen zu können müssen beschriftbare Drawables den trait "TextDisplayAddon" einmischen und die genutzte RenderStage muss das Plugin TextDisplayRenderStagePlugin sowie TextureRenderStagePlugin einbinden.

Der angezeigte Text kann im Drawable mit 

.. code-block:: scala
    drawable.text = "irgendein Text" 

verändert werden. Außerdem werden Einstellmöglichkeiten für die Schriftart, -größe und -stil (über java.awt.Font) und die Schriftfarbe (java.awt.Color) angeboten.

Der Text wird zentriert angezeigt und wird am Wortende umgebrochen, falls der horizontale Platz nicht ausreicht. Für alle Seiten des Objekts wird dieselbe Textur genutzt. Dies funktioniert problemlos, wenn ein Objekt gleichmäßig in alle 3 Richtungen skaliert wird. Die Schriftgröße wird als Mindestgröße interpretiert; falls ein Objekt eine Skalierung von > 1 aufweist wird die Größe der Schrift proportional mitskaliert. Bei einer Skalierung kleiner 1 wird der für die Schrift zur Verfügung stehende Platz verkleinert. 

[wie siehts jetzt wirklich aus?: Ungleichmäßigen Skalierungen verursachen jedoch ein Problem.] 


Aufwändigere Rendertechniken wurden in Betracht gezogen (-> Vektorrendering), jedoch war die Darstellungsqualität des umgesetzten, einfachen Ansatzes gut genug für den hier entwickelten Prototypen. 
Für weitere Arbeiten auf diesem Gebiet sollte dies jedoch erneut evaluiert werden. Besonders die Möglichkeiten aktuellster Grafikhardware mit OpenGL4-Unterstützung, neue Geometrien direkt auf der Grafikeinheit zu erzeugen könnten für die Implementierung von sehr gut lesbaren und trotzdem performanten Schrift-Renderern interessant sein.

COLLADA2Scala-Compiler
----------------------





