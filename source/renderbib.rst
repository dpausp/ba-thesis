.. _render-bibliothek:

*****************
Render-Bibliothek
*****************

Für die im vorherigen Kapitel beschriebene Renderkomponente wurde eine Anbindung an eine Grafikschnittstelle benötigt, mit der sich die folgenden Anforderungen realisieren lassen:

    #. Nutzung der Fähigkeiten moderner Grafikhardware, unter Anderem um eine hohe Ausführungsgeschwindigkeit zu erlauben
    #. Möglichkeit, spezielle Effekte mit Hilfe direkter Programmierung der Grafikhardware zu implementieren (Shaderprogrammierung)
    #. Die Grafikausgabe sollte auf verbreiteten Plattformen funktionieren, mindestens auf Windows und Linux.
    #. Bereitstellung von in der Computergrafik üblichen Abstraktionen wie einer Kamera, Lichtquellen und Grafikobjekten, die die Manipulation von Visualisierungsparametern zur Laufzeit erlauben
    #. Unterstützung von transluzenten Objekten
    #. Darstellung von Schrift und Texturen auf 3D-Objekten, möglichst skalierbar
    #. "Bild-in-Bild"-Techniken ("off-screen rendering"), also das Zeichnen von 3D-Teil-Szenen in 2D-Bilder, die im Grafikspeicher abgelegt sind und so in eine größere 3D-Szene eingebettet werden können.

Anforderung (5) wird im Projekt für die Darstellung von deaktivierten Modellelementen (:ref:`visualisierungsvarianten`) sowie Modellierungsflächen (:ref:`modellierungsflaechen`) genutzt.
Anforderung (7) ist auf den Wunsch zurückzuführen, komplexe Menüs wie das Eightpen-Menü :cite:`buchi` erstellen zu können, die selbst wieder 3D-Objekte enthalten. 

Die Anforderungen 1-3. werden – auf niedriger Ebene – nur von OpenGL in einer relativ aktuellen Version ab 3.0 erfüllt. 
Da "modernes" OpenGL (:ref:`opengl:`) im Prinzip nur eine direkte Schnittstelle zur Grafikhardware ist, ist die Programmierung wegen fehlender Abstraktion und Wiederverwendbarkeit relativ aufwändig.

Im Gegensatz dazu stehen zahlreiche Frameworks, die verschiedene Aufgabengebiete und Abstraktionsebenen abdecken und auf Low-Level-Schnittstellen wie OpenGL aufsetzen.
Diese könnten die Anforderungen 4-7. selbst abdecken oder zumindest dafür benutzt werden, diese zu implementieren.
Solche Systeme werden oft als "Game-Engines" bezeichnet, die in erster Linie für die Erstellung von 3D-Computerspielen gedacht sind, sich aber auch durchaus für andere Visualisierungsaufgaben einsetzen lassen. [f3]_

Für die Java-Plattform sind mit JME3 und Ardor3D zwei relativ aktuelle, in Java implementierten 3D-Frameworks verfügbar. 
JME3 lag zu Beginn der vorliegenden Projektes nur in einer Alpha-Version vor und schied deshalb für diese Arbeit aus.
Allgemein ist es bei solchen eher komplexen Frameworks schwer einzuschätzen, ob alle benötigten Grafikeffekte ohne viel Aufwand oder sogar nur mit Änderungen an den Frameworks selbst realisiert werden können. 
Besonders war dies aufgrund der relativ spärlichen (Wiki-) Dokumentation bei Ardor3D der Fall.

Diese Frameworks bieten eher "zu viele" Fähigkeiten an, die über die reine Grafikausgabe hinausgehen, wie beispielsweise eine Physik-Unterstützung oder die Einbindung von Eingabegeräten. 
Oft werden verschiedenste Aufgaben wie die Grafikrepräsentation und die räumliche Strukturierung der Szene in einen zentralen "Szenengraphen" integriert, mit dem eine Anwendung interagiert.
Dies ist für die vorliegende Anwendung nicht sinnvoll, da sie auf Simulator X (:ref:`simulator-x`) basiert, welcher genau diese "Vermischung" von Aufgaben verhindern soll und selbst schon eine Infrastruktur für die Realisierung von 3D-Anwendungen bereitstellt.

Für die Integration in Simulator X über die Renderkomponente war deshalb eher eine "leichtgewichtige" Lösung wünschenswert, die nur die Grafikausgabe auf eine einfach benutzbare und erweiterbare Weise realisiert.


Übersicht
=========

Im Rahmen dieser Arbeit wurde somit eine an die Erfordernisse des Projekts angepasste Render-Bibliothek auf Basis von OpenGL erstellt, die allerdings auch für weitere Projekte verwendet werden kann.
Andere Anwendungen könnten ebenfalls auf Simulator X mit der vorgestellten Renderkomponente realisiert werden oder die Render-Bibliothek direkt nutzen.


Als Grafikschnittstelle wird die für die Java Virtual Machine verfügbare OpenGL-Anbindung LWJGL (Lightweight Java Gaming Library) in der Version 2.8.x (siehe :ref:`opengl`) genutzt. 
Es werden nur OpenGL-Funktionen genutzt, die in der Version 3.3 standardmäßig verfügbar sind. 
Das heißt, dass auf im OpenGL-Standard "deprecated" markierte Funktionalität vollständig verzichtet wurde, welche in Zukunft komplett entfernt werden oder zu Geschwindigkeitsproblemen führen könnte. 
Damit ist zu erwarten, dass die Render-Bibliothek auch mit den aktuellesten und zukünftigen OpenGL-Versionen und neuer Grafikhardware – zumindest in den nächsten Jahren – kompatibel sein wird.

Die Render-Bibliothek orientiert sich in den Grundkonzepten an der in C++ verfügbaren "Visualization Library" :cite:`www-vislib`.

Im Gegensatz zu vielen, umfassenden 3D-Engines sollten dem Benutzer der Bibliothek möglichst wenig Einschränkungen bei der Gestaltung seines Programms auferlegt werden.
Bei der Konzeption der Render-Bibliothek stand die Wiederverwendbarkeit von einzelnen Bestandteilen im Vordergrund, die sich wieder zu höheren Abstraktionen zusammensetzen lassen.

Solche Abstraktionen werden – wenn sie allgemein verwendbar sind – direkt von der Renderbibliothek angeboten, können aber auch speziell für eine bestimmte Anwendung erstellt werden.
Durch das Prinzip soll der Programmierer von oft wiederkehrenden Aufgaben entlastet werden, aber trotzdem die vollen Möglichkeiten von OpenGL nutzen können, wenn nötig.

Höhere Abstraktionen sollen auch von Programmieren ohne tiefgreifende Computergrafik- und OpenGL-Kenntnisse genutzt werden können.
Ein Beispiel dafür ist die (wo?) gezeigte Möglichkeit, auf einfachem Wege ein neues Grafikobjekt für die Modellierung auf Basis der schon vorhandenen Figuren zu erstellen.


.. TODO ref?


Die Library lässt sich grob in zwei Schichten, eine **Low-Level-API** und einer **Higher-Level-API** aufteilen, die im Folgenden vorgestellt werden.

Low-Level-API
=============

Das als Grundlage genutzte LWJGL bietet nur eine sehr dünne Abstraktion oberhalb von OpenGL, die vor allem dazu dient, OpenGL-Datentypen auf die Java VM abzubilden und umgekehrt.
Die von LWJGL angebotenen Funktionen entsprechend weitestgehend denen, die durch den OpenGL-Standard vorgegeben und aus Programmiersprachen wie C bekannt sind.

Eine Low-Level-API\ [#I]_ sorgt nun für die objektorientierte Kapselung von OpenGL-Basiselementen und verschiedene Vereinfachungen für Standardfälle.
Diese API ermöglicht es, sehr nahe an den Konzepten von OpenGL zu entwickeln, ohne bei Routineaufgaben selbst viel OpenGL-Code schreiben zu müssen. 
Klassen- und Methodennamen orientieren sich, wie bei der Visualization Library, vorwiegend an den gekapselten OpenGL-Funktionen.

Hier soll nur eine kurze Übersicht über die Funktionalitäten gegeben werden, da diese für das Verständnis dieser Arbeit weniger wichtig sind und sehr OpenGL-spezifisch sind. 
    
Vertex Buffer Object (VBO)-Klassen für verschiedene Datentypen 
    Vereinfachen die Verwaltung des Grafikspeichers, beispielsweise den Transfer von Daten dorthin.

Uniform, UniformBlock- und VertexAttribute-Klassen
    Daten lassen sich so bequem zum Shaderprogramm auf der Grafikkarte übertragen. Die UniformBlock und VertexAttribute-Klassen bauen auf der VBO-Abstraktion auf.

Beispiel für eine Uniform-Verwendung:

.. code-block:: scala
    val color = ConstVec4(1, 1, 1, 1)
    val colorUniform = GLUniform4f("color")
    colorUniform.set(color)

Shader- und ShaderProgram-Klasse
    Übernehmen das Kompilieren und Linken von Shadern sowie die Verwaltung von Uniforms und UniformBlocks.

Abstraktionen für das Offscreen-Rendering (Anforderung 7)
    Renderbuffer und Framebuffer-Klassen (FBO)

Sonstige Abstraktionen: Zeichenbefehle (DrawArrays und DrawElements), Textur- und Textursampler-Klassen, Viewport und Hintergrundfarbe (glClearColor), OpenGL-Einstellungen (wie glEnable oder glDepthFunc)

Higher-Level-API
================

Diese Schicht stellt im Wesentlichen Schnittstellen und häufig benötigte Implementierungen für die Aufgabe bereit, die grafischen Objekte und den eigentlichen Rendervorgang zu beschreiben, der jene Objekte schließlich "auf den Bildschirm bringt". 
Zur Implementierung werden die von der Low-Level-API bereitgestellten OpenGL-Abstraktionen genutzt.

In dieser Bibliothek wird ein solcher Rendervorgang durch sogenannte *RenderStages* beschrieben.
Objekte, die von solchen RenderStages angezeigt werden können werden als *Drawable* bezeichnet. 

.. _drawable:

Drawable
---------

Zu zeichnende Objekte werden durch eine Klasse beschrieben, welche von der Basisklasse *Drawable* abgeleitet ist.
Solche Drawable-Klassen müssen eine Beschreibung der Geometrie (trait *Mesh*), der Position und Größe (trait *Transformation*) und der Darstellungsweise (trait *Effect*) enthalten.
Die Implementierung ist dabei sehr flexibel möglich und kann an die Anforderungen des konkret dargestellten Objekts und der Anwendung angepasst werden. 

In den traits sind nur Methoden vorgegeben, die die vom "Renderer" benötigten Daten liefern müssen.
Dieser Renderer kann im Prinzip selbst implementiert werden oder es kann eine *RenderStage* dafür konfiguriert und genutzt werden.

.. TODO Klassendiagramm?

Drawables stellen im Normalfall eine Schnittstelle für die Anwendung bereit, über die sich Attribute des Grafik-Objektes auslesen und setzen lassen.
So könnte eine Transformation, die für ein bewegliches Objekt eingesetzt wird, einen Setter bereitstellen, der das Verändern der aktuellen Position erlaubt.

Die Render-Bibliothek stellt eine Reihe von Implementierungen dieser Traits zur Verfügung. 
Diese sind zwar auf die Bedürfnisse des I>PM3D-Projekts abgestimmt, aber möglichst allgemein gehalten und damit wiederverwendbar.

Sinnvollerweise werden Drawables erstellt, indem traits zusammengemischt werden, die die genannten Basis-Traits Mesh, Transformation und Effect implementieren.
So kann mit diesem Konzept beispielsweise leicht ein Würfel definiert werden, indem eine entsprechende Mesh-Implementierung erstellt wird, die die Vertexkoordinaten und weitere Informationen wie Normalen und Texturkoordinaten liefert. 
Durch die Verwendung von unterschiedliche Effect-Traits können verschieden dargestellte Varianten erstellt werden, die beispielsweise eine Darstellung in einer einheitlichen Farbe oder mit einer Textur vorgeben.

Besonders Effects können relativ kompliziert aufgebaut sein. Es ist sinnvoll, diese wieder aus verschiedenen traits zusammenzusetzen, die Teilfunktionalitäten implementieren.
Solche Traits werden in der Renderbibliothek mit der Endung -Addon benannt; beispielsweise existiert ein PhongLightingAddon für die Bereitstellung von Lichtparametern und ein TextDisplayAddon, das die Anzeige von Schrift auf den Objekten implementiert.

.. TODO am besten durch ein Klassendiagramm ergänzen / ersetzen

Ressourcen, die potenziell von vielen verschiedenen Drawables geteilt werden können werden im Drawable nur durch eine abstrakte Beschreibung dargestellt. 
Texturen werden über eine *TextureDefinition* beschrieben, Shaderquelldateien über eine *ShaderDefinition*. 

.. _render-stage:

RenderStage
-----------

RenderStages sind für das Zeichnen der grafischen Objekte zuständig. Die Anwendung übergibt diesen RenderStages einmal pro Frame [#f1]_ alle zu zeichnenden Objekte. 
Diese werden in der bereitgestellten Implementierung der RenderStage zuerst sortiert und anschließend gezeichnet. 
Eine Sortierung wird durchgeführt, um transluzente Objekte (Anforderung 7) in der richtigen Reihenfolge zu zeichnen sowie um unnötige Zeichenoperationen und OpenGL-Zustandswechsel zu vermeiden.
Durch Angabe einer Render-Priorität in den Drawables kann manuell eine bestimmte Reihenfolge erzwungen werden, wenn dies für spezielle Zeichenaufgaben nötig ist.

Von der Renderstage werden zu den von Drawables definierten Texture- und ShaderDefinitions Objekte der Low-Level-API nach Bedarf erzeugt.
Diese werden für das Zeichnen von mehreren Drawables wiederverwendet, um Grafikspeicher und Zeit zu sparen.

Abgegrenzte Funktionalitäten können in **RenderStagePlugins** ausgelagert werden. 
So stellt die Renderbibliothek unter Anderem Plugins für die Verwaltung von Texturen und die Umsetzung von Lichtquellen bereit.

Weitere Abstraktionen
---------------------

Licht 
^^^^^^

Die Renderbibliothek unterstützt das Phong-Beleuchtungsmodell, wobei dieses pixelgenau ausgewertet wird. 
Für die Anwendung werden Klassen bereitgestellt, die die von "altem" OpenGL bekannten "Lichtquellen" bereitstellen und sich an deren Schnittstelle orientieren. 
Lichtquellen können entweder entfernungsabhängig (*PositionalLight*) oder -unabhängig sein (*DirectionalLight*).

Implementiert wird die Beleuchtung auf Scala-Seite durch das Zusammenspiel des *PhongLightingRenderStagePlugins* mit dem Effect-Addon *PhongLightingAddon*. 
Die eigentlichen Lichtberechnungen wurden in GLSL-Shaderfunktionen implementiert, die von verschiedenen Fragment-Shadern genutzt werden. [#f4]_

Kamera
^^^^^^



COLLADA2Scala-Compiler
======================

Da Laden von Modellen direkt aus COLLADA-XML-Dateien ist relativ zeitaufwändig. Außerdem unterstützt der genutzte COLLADA-Loader :cite:`uli` bisher noch nicht die Wiederverwendung der geladenen Geometriedaten. 
So wird für jede Instanz eines COLLADA-Modellobjekts zusätzlicher Grafikspeicher belegt. 

Um die Effizienz zu steigern und nachträgliche Modifikationen an den Modelldaten zu erlauben wurde ein eigenständiges Programm entwickelt, dass mit Hilfe des COLLADA-Loaders ein Modell lädt und daraus eine Repräsentation in Scala-Code erstellt. Die erzeugte Scala-Meshdatei lässt sich dann dafür nutzen, neue Modellobjekte zu konstruieren, wie es im Beispiel im nächsten Abschnitt gezeigt wird.

In I>PM3D genutzte Funktionalitäten
===================================

Übersicht
---------

Hier eine Übersicht über die in dieser Arbeit genutzte Funktionalität:

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


.. [#I] Zu finden im Package mmpe.renderer.gl

.. [#f1] Frame??!

.. [#f2] Der "Flaschenhals" der Anwendung ist eher die Physikkomponente. Die Ursachen wurden nicht näher untersucht, da immerhin mehrere hundert Modellelemente auf aktuellen Systemen noch relativ schnell dargestellt werden können.

.. [#f3] Gezeigt wird dies beispielsweise von :cite:`alvergren_3d_2009` (:ref:`krolovitsch`). Dort sei die C++-Game-Engine Panda3D (über eine Python-Anbindung) genutzt worden, um schnell einen Prototypen für einen 3D-Zustandsdiagramm-Editor zu erstellen.


