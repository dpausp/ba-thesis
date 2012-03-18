.. _render-bibliothek:

*****************
Render-Bibliothek
*****************

Im Rahmen dieser Arbeit wurde eine an die Erfordernisse des Projekts angepasste Render-Bibliothek auf Basis von OpenGL erstellt, die allerdings auch für andere Projekte verwendet werden kann.

Die Render-Bibliothek basiert auf der für die Java Virtual Machine verfügbare OpenGL-Anbindung LWJGL in der Version 2.8.x (siehe :ref:`opengl`). 
Zur Implementierung der Grafikfunktionen wurden nur OpenGL-Funktionen genutzt, die in der Version 3.3 standardmäßig verfügbar sind. 
Insbesondere wurde auf im OpenGL-Standard "deprecated" markierte Funktionalität vollständig verzichtet. 
Damit ist zu erwarten, dass die Render-Bibliothek auch mit den aktuellesten und zukünftigen OpenGL-Versionen – zumindest in den nächsten Jahren – kompatibel sein wird.


Bei der Konzeption der Render-Bibliothek stand die Wiederverwendbarkeit von einzelnen Bestandteilen im Vordergrun 


Daher war es erforderlich, die Abhängigkeiten der Bestandteile der Bibliothek möglichst gering. 
Dies sollte auf verschiedenen Abstraktionsebenen möglich sein. 

und für eine Entlastung des Programmierers von oft wiederkehrenden Aufgaben.

Im Gegensatz zu umfassenden "3D-Engines", die einem Programmier sehr viel Arbeit abnehmen können, sollten dem Benutzer der Bibliothek möglichst wenig Einschränkungen bei der Gestaltung seines Programms auferlegt werden.

Die Library lässt sich grob in zwei Schichten, eine **Low-Level-API** und einer **Higher-Level-API** aufteilen. 

Low-Level-API
=============

Das als Grundlage genutzte LWJGL bietet nur eine sehr dünne Abstraktion oberhalb von OpenGL, die vor allem dazu dient, OpenGL-Datentypen auf die Java VM abzubilden und umgekehrt.
Die von LWJGL angebotenen Funktionen entsprechend weitestgehend denen, die durch den OpenGL-Standard vorgegeben und aus Programmiersprachen wie C bekannt sind.

Eine Low-Level-API\ [#I]_ sorgt nun für die objektorientierte Kapselung von OpenGL-Basiselementen.
Diese API ermöglicht es, sehr nahe an den Konzepten von OpenGL zu entwickeln, ohne bei Routineaufgaben selbst viel OpenGL-Code schreiben zu müssen. 
Klassen- und Methodennamen orientieren sich vorwiegend an den gekapselten OpenGL-Funktionen.

Dies umfasst unter Anderem folgende Funktionalitäten:
    
Vertex Buffer Object (VBO) und UniformBlock-Klassen 
    Vereinfachen die Verwaltung des Grafikspeichers, beispielsweise den Transfer von Daten.

Uniform- und VertexAttribute-Klassen
    Daten lassen sich so bequem zum Shaderprogramm auf der Grafikkarte übertragen.

Beispiel für eine Uniform-Verwendung:

.. code-block:: scala
    val color = ConstVec4(1, 1, 1, 1)
    val colorUniform = GLUniform4f("color")
    colorUniform.set(color)

Shader- und ShaderProgram-Klasse
    Übernehmen das Kompilieren und Linken von Shadern sowie die Verwaltung von Uniforms und UniformBlocks.

Sonstige Abstraktionen: Textur- und Textursampler, Renderbuffer, Framebuffer (FBO), OpenGL-Einstellungen (wie glEnable oder glDepthFunc)

Higher-Level-API
================

Diese Schicht stellt im Wesentlichen Schnittstellen und häufig benötigte Implementierungen für die Aufgabe bereit, die grafischen Objekte und den eigentlichen Rendervorgang zu beschreiben, der jene Objekte schließlich "auf den Bildschirm bringt". 
Zur Implementierung werden die von der Low-Level-API bereitgestellten OpenGL-Abstraktionen genutzt.

Der Rendervorgang umfasst oft mehrere Schritte, welche zusammen als *Render-Pipeline* bezeichnet werden. 
In dieser Bibliothek werden die einzelnen Stufen durch sogenannte *RenderStages* beschrieben.
Objekte, die von solchen RenderStages angezeigt werden können werden als *Drawable* bezeichnet. 

.. _drawable:

Drawable
---------

Zu zeichnende Objekte werden durch eine Klasse beschrieben, welche von der Basisklasse *Drawable* abgeleitet ist.
Solche Drawable-Klassen müssen eine Beschreibung der Geometrie (trait *Mesh*), der Position und Größe (trait *Transformation*) und der Darstellungsweise (trait *Effect*) enthalten.
Die Implementierung ist dabei sehr flexibel möglich und kann an die Anforderungen der Anwendung angepasst werden. 
Es sind nur Methoden vorgegeben, die die vom Renderer benötigten Daten liefern müssen.

Drawables stellen im Normalfall eine Schnittstelle für die Anwendung bereit, über die sich Attribute des Grafik-Objektes auslesen und setzen lassen.
So könnte eine Transformation, die für ein bewegliches Objekt eingesetzt wird, einen Setter bereitstellen, der das Verändern der aktuellen Position erlaubt.

Die Render-Bibliothek stellt eine Reihe von Implementierungen dieser Traits zur Verfügung. 
Diese sind zwar auf die Bedürfnisse des I>PM3D-Projekts abgestimmt, aber möglichst allgemein gehalten und damit wiederverwendbar.

Sinnvollerweise werden Drawables erstellt, indem traits zusammengemischt werden, die die genannten Basis-Traits Mesh, Transformation und Effect implementieren.

So kann mit diesem Konzept beispielsweise leicht ein Würfel definiert werden, indem eine entsprechende Mesh-Implementierung erstellt wird, die die Vertexkoordinaten und weitere Informationen wie Normalen und Texturkoordinaten liefert. Davon können verschieden dargestellte Varianten erstellt werden, indem unterschiedliche Effect-Traits verwendet werden, die beispielsweise eine Darstellung in einer einheitlichen Farbe oder mit einer Textur vorgeben.

Besonders Effects können relativ kompliziert aufgebaut sein. Es ist sinnvoll, diese wieder aus verschiedenen traits zusammenzusetzen, die Teilfunktionalitäten implementieren.

Solche Traits werden mit der Endung -Addon benannt; beispielsweise enthält die Renderbibliothek ein PhongLightingAddon für die Bereitstellung von Lichtparametern und ein TextDisplayAddon, das die Anzeige von Schrift auf den Objekten implementiert.

.. _render-stage:

RenderStage
-----------

RenderStages sind für das Zeichnen der grafischen Objekte zuständig. Die Anwendung übergibt diesen RenderStages einmal pro Frame [#f1]_ alle zu zeichnenden Objekte. 
Diese werden in der bereitgestellten Implementierung der RenderStage zuerst sortiert und anschließend gezeichnet. 
Eine Sortierung wird durchgeführt, um transluzente Objekte in der richtigen Reihenfolge zu zeichnen sowie um unnötige Zeichenoperationen und OpenGL-Zustandswechsel zu vermeiden.

Ressourcen, die potenziell von vielen verschiedenen Drawables geteilt werden können werden im Drawable nur durch eine abstrakte Beschreibung dargestellt. 
Texturen werden über eine *TextureDefinition* beschrieben, Shaderquelldateien über eine *ShaderDefinition*. 
Die dazugehörigen Objekte der Low-Level-API werden von der RenderStage nach Bedarf erzeugt und für das Zeichnen von mehreren Drawables wiederverwendet, um Grafikspeicher und Zeit zu sparen.

Weitere Funktionalitäten können über RenderStagePlugins hinzugefügt werden. So gibt es beispielsweise Plugins für die Verwaltung von Texturen und die Umsetzung von Lichtquellen.

COLLADA2Scala-Compiler
======================

Da Laden von Modellen direkt aus COLLADA-XML-Dateien ist relativ zeitaufwändig. Außerdem unterstützt der genutzte COLLADA-Loader bisher noch nicht die Wiederverwendung der geladenen Geometriedaten. 
So wird für jede Instanz eines COLLADA-Modellobjekts zusätzlicher Grafikspeicher belegt. 

Um die Effizienz zu steigern und nachträgliche Modifikationen an den Modelldaten zu erlauben wurde ein eigenständiges Programm entwickelt, dass mit Hilfe des COLLADA-Loaders ein Modell lädt und daraus eine Repräsentation in Scala-Code erstellt. Die erzeugte Scala-Meshdatei lässt sich dann dafür nutzen, neue Modellobjekte zu konstruieren.

In I>PM3D genutzte Funktionalitäten
===================================

Übersicht
---------

Hier eine Übersicht über die in dieser Arbeit genutzte Funktionalität:

* Darstellung von Text, auch mehrzeilig 
* Texturierung mit Unterstützung für Mipmapping und anisotropisches Filtern: für Text und Symbole auf Modellelementen
* Lichtquellen nach dem Phong-Beleuchtungsmodell, pixelgenau
* Unterstützung für beliebig viele Lichtquellen
* Transluzenz auf Objektebene (deaktivierte Modellelemente)
* COLLADA-Loader
* COLLADA2Scala-Compiler
* Rendern in Texturen (render-to-texture, offscreen rendering): genutzt für Eightpen-Menü.
* Unterstützung für deaktivierte, hervorgehobene und selektierte Elemente.
* Darstellung von transluzenten Flä

.. _darstellung-text:

Darstellung von Text
--------------------

..  nach Anhang A

Unter Anderem für die Beschriftung von Modellknoten wurde eine gut lesbare und trotzdem einfach umsetzbare Technik für das Rendering von Schrift benötigt.
Hierfür wurde die 2D-API (java.awt) der Java-Klassenbibliothek zur Hilfe genommen. 
Zur Verwendung mit OpenGL wird die Schrift in eine Textur geschrieben, die dann auf die Objekte aufgebracht werden kann.
Zur Verbesserung der Darstellung wird die Antialiasing-Funktion von Graphics2D genutzt. 

Um auch bei größeren Entferungen von der Kamera und kleiner Schrift noch eine angemessene Lesbarkeit zu erreichen kann Mipmapping genutzt werden. 
Aufgrund von Problemen mit verschiedenen Grafikkarten, die im Rahmen des Projekts getestet wurden, ist dies standardmäßig jedoch nicht aktiviert.

Um Text darstellen zu können müssen beschriftbare Drawables den Trait *TextDisplayAddon* einmischen und die genutzte RenderStage muss das Plugin *TextDisplayRenderStagePlugin* sowie *TextureRenderStagePlugin* einbinden.

Der angezeigte Text kann im Drawable mit 

.. code-block:: scala

    drawable.text = "irgendein Text" 

verändert werden. Außerdem werden Einstellmöglichkeiten für die Schriftart, -größe und -stil (über java.awt.Font) und die Schriftfarbe (java.awt.Color) angeboten.

Der Text wird zentriert angezeigt und wird am Wortende umgebrochen, falls der horizontale Platz nicht ausreicht. Für alle Seiten des Objekts wird dieselbe Textur genutzt. Dies funktioniert problemlos, wenn ein Objekt gleichmäßig in alle 3 Richtungen skaliert wird. Die Schriftgröße wird als Mindestgröße interpretiert; falls ein Objekt eine Skalierung von > 1 aufweist wird die Größe der Schrift proportional mitskaliert. Bei einer Skalierung kleiner 1 wird der für die Schrift zur Verfügung stehende Platz verkleinert. 

[wie siehts jetzt wirklich aus?: Ungleichmäßigen Skalierungen verursachen jedoch ein Problem.] 




Unterstützung für deaktivierte, hevorgehobene und selektierte Elemente
----------------------------------------------------------------------

..  TODO

Für :ref:`visualisierungsvarianten` mussten verschiedene 

lassen sich einfach über entsprechende Properties aktivieren:

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

Betrachtungswinkelabhängige Darstellung von Texturen
----------------------------------------------------

..  TODO

Um dieses Problem abzumildern wird jedoch die Anzeige von der Blickrichtung des Benutzers (der Kamera) abhängig gemacht. Das hat zur Folge, dass die Information nur auf der dem Benutzer zugewandten Seite mit hoher Intensität dargestellt wird. Zur Berechnung wird der Winkel bzw. das Skalarprodukt zwischen Kameravektor und der Normalen der jeweiligen Objektfläche herangezogen. Dessen Wert bestimmt, zu welchem Anteil die Vordergrundfarbe (Schriftfarbe bzw. Texturfarbe) zur Hintergrundfarbe gemischt wird und welchen Einfluss sie damit auf den endgültig sichtbaren Farbton hat hat. 

Ab einem gewissen Winkel wird nur noch die Hintergrundfarbe angezeigt.


