.. _render-bibliothek:

*****************
Render-Bibliothek
*****************

Für die im vorherigen Kapitel beschriebene Renderkomponente wurde eine Anbindung an eine Grafikschnittstelle benötigt, mit der sich die folgenden Anforderungen realisieren lassen:

    #. Nutzung der Fähigkeiten moderner Grafikhardware, unter Anderem um eine hohe Ausführungsgeschwindigkeit zu erlauben.
    #. Möglichkeit, spezielle Effekte mit Hilfe direkter Programmierung der Grafikhardware flexibel implementieren zu können (Shaderprogrammierung).
    #. Die Grafikausgabe sollte auf verbreiteten Plattformen funktionieren, mindestens auf Windows und Linux.
    #. Bereitstellung von in der Computergrafik üblichen Abstraktionen wie einer Kamera, Lichtquellen und Grafikobjekten, die die Manipulation von Visualisierungsparametern zur Laufzeit erlauben.
    #. Unterstützung von transluzenten Objekten
    #. Darstellung von Schrift und Texturen auf 3D-Objekten, möglichst skalierbar
    #. "Bild-in-Bild"-Techniken ("off-screen rendering"), also das Zeichnen von 3D-Teil-Szenen in 2D-Bilder, die im Grafikspeicher abgelegt sind und so in eine größere 3D-Szene eingebettet werden können.

Anforderung (5) wird im Projekt für die Darstellung von :ref:`deaktivierten Modellelementen <deaktivierung>` sowie :ref:`Modellierungsflächen<modellierungsflaechen>` gebraucht. 
Anforderung (7) ist auf den Wunsch zurückzuführen, komplexe Menüs wie das Eightpen-Menü :cite:`buchi` erstellen zu können, die selbst wieder 3D-Objekte enthalten. 

Die Anforderungen (1, 2, 3) werden – auf niedriger Ebene – nur von OpenGL in einer relativ aktuellen Version ab 3.0 erfüllt. 
Da "modernes" :ref:`OpenGL <opengl>` im Prinzip nur eine direkte Schnittstelle zur Grafikhardware ist, ist die Programmierung wegen fehlender Abstraktion und Wiederverwendbarkeit relativ aufwändig.

Im Gegensatz dazu stehen zahlreiche Frameworks, die verschiedene Aufgabengebiete und Abstraktionsebenen abdecken und auf Low-Level-Schnittstellen wie OpenGL aufsetzen.
Diese könnten die Anforderungen (4–7) selbst abdecken oder zumindest dafür benutzt werden, diese zu implementieren.
Solche Systeme werden oft als "Game-Engines" bezeichnet, die in erster Linie für die Erstellung von 3D-Computerspielen gedacht sind, sich aber auch durchaus für andere Visualisierungsaufgaben einsetzen lassen\ [#f3]_.

Für die Java-Plattform sind mit JME3 und Ardor3D zwei relativ aktuelle, in Java implementierte 3D-Frameworks verfügbar. 
JME3 lag zu Beginn der vorliegenden Projektes nur in einer Alpha-Version vor und schied deshalb für diese Arbeit aus.
Allgemein ist es bei solchen eher komplexen Frameworks schwer einzuschätzen, ob alle benötigten Grafikeffekte ohne viel Aufwand oder sogar nur mit Änderungen an den Frameworks selbst realisiert werden können. 
Insbesondere war dies aufgrund der relativ spärlichen (Wiki-) Dokumentation von Ardor3D der Fall.

Diese Frameworks bieten außerdem eher "zu viele" Fähigkeiten an, die über die reine Grafikausgabe hinausgehen, wie beispielsweise eine Physik-Unterstützung oder die Einbindung von Eingabegeräten. 
Oft werden verschiedenste Aufgaben wie die Grafikrepräsentation und die räumliche Strukturierung der Szene in einen zentralen "Szenengraphen" integriert, mit dem eine Anwendung interagiert.
Dies ist für die vorliegende Anwendung nicht sinnvoll, da sie auf :ref:`Simulator X<simulatorx>` basiert, welcher genau diese "Vermischung" von Aufgaben verhindern soll und selbst schon eine Komponenten-Infrastruktur für die Realisierung von 3D-Anwendungen bereitstellt.

Für die Integration in Simulator X über die Renderkomponente war deshalb eher eine "leichtgewichtige" Lösung wünschenswert, welche nur die Grafikausgabe auf eine einfach benutzbare und erweiterbare Weise realisiert.

Übersicht
=========

Im Rahmen dieser Arbeit wurde somit eine an die Erfordernisse des Projekts angepasste Render-Bibliothek auf Basis von OpenGL erstellt, die allerdings auch für weitere Projekte verwendet werden kann.
Andere Anwendungen könnten ebenfalls auf Simulator X-Basis mit der neuen :ref:`renderkomponente` realisiert werden oder die Render-Bibliothek direkt nutzen.

Als Anbindung an OpenGL wird :ref:`LWJGL<opengl>` genutzt. 
Es werden nur OpenGL-Funktionen genutzt, die in der Version 3.3 standardmäßig verfügbar sind. 
Das heißt, dass auf im OpenGL-Standard "deprecated" markierte Funktionalität vollständig verzichtet wurde, welche in Zukunft komplett entfernt werden oder zu Geschwindigkeitsproblemen führen könnte. 
Damit ist zu erwarten, dass die Render-Bibliothek auch mit den aktuellsten und zukünftigen OpenGL-Versionen sowie neuer Grafikhardware – zumindest in den nächsten Jahren – kompatibel sein wird.

Die Render-Bibliothek orientiert sich in den Grundkonzepten an der für C++ verfügbaren *Visualization Library* :cite:`www:vislib`.
Im Gegensatz zu vielen, umfassenden 3D-Engines sollten dem Benutzer der Bibliothek möglichst wenig Einschränkungen bei der Gestaltung seines Programms auferlegt werden.
Bei der Konzeption der Render-Bibliothek stand die Wiederverwendbarkeit von einzelnen Bestandteilen im Vordergrund, die sich wieder zu höheren Abstraktionen zusammensetzen lassen.

Solche Abstraktionen werden – wenn sie allgemein verwendbar sind – direkt von der Render-Bibliothek angeboten, können aber auch speziell für eine bestimmte Anwendung erstellt werden.
Durch das Prinzip soll der Programmierer von oft wiederkehrenden Aufgaben entlastet werden, aber trotzdem die vollen Möglichkeiten von OpenGL nutzen können, wenn nötig.

Höhere Abstraktionen sollen auch von Programmieren ohne tiefgreifende Computergrafik- und OpenGL-Kenntnisse genutzt werden können.
Ein :ref:`Beispiel<beispiel-neue-modellfigur>` dafür ist die Möglichkeit, auf einfachem Wege ein neues Grafikobjekt für die Darstellung von Modellelementen zu erstellen.

Die Library lässt sich grob in zwei Schichten, eine **Low-Level-API** und einer **Higher-Level-API** aufteilen, die im Folgenden vorgestellt werden.

Low-Level-API
=============

Das als Grundlage genutzte LWJGL bietet nur eine sehr dünne Abstraktionschicht oberhalb von OpenGL, die vor allem dazu dient, OpenGL-Datentypen auf die Java VM abzubilden und umgekehrt.
Die von LWJGL angebotenen Funktionen entsprechend weitestgehend denen, die durch den OpenGL-Standard vorgegeben und aus Programmiersprachen wie C bekannt sind.

Die Low-Level-API\ [#f6]_ sorgt nun für die objektorientierte Kapselung von OpenGL-Basiselementen und verschiedene Vereinfachungen für Standardfälle.
Diese Schicht ermöglicht es, sehr nahe an den Konzepten von OpenGL zu entwickeln, ohne bei Routineaufgaben selbst viel OpenGL-Code schreiben zu müssen. 
Klassen- und Methodennamen orientieren sich, wie bei der Visualization Library, vorwiegend an den gekapselten OpenGL-Funktionen.

Hier soll nur eine kurze Übersicht über die Funktionalitäten gegeben werden, da diese für das Verständnis dieser Arbeit weniger wichtig sind und sehr OpenGL-spezifisch sind. 
    
Vertex Buffer Object (VBO)-Klassen für verschiedene Datentypen 
    Vereinfachen die Verwaltung des Grafikspeichers, beispielsweise den Transfer von Daten dorthin.

Uniform, UniformBlock- und VertexAttribute-Klassen
    Daten lassen sich so bequem zum Shaderprogramm auf der Grafikkarte übertragen. Die UniformBlock- und VertexAttribute-Klassen bauen auf der VBO-Abstraktion auf.

Beispiel für eine Uniform-Verwendung:

.. code-block:: scala

    val color = ConstVec4(1, 1, 1, 1)
    val colorUniform = GLUniform4f("color")
    colorUniform.set(color)

Shader- und ShaderProgram-Klasse
    Übernehmen das Kompilieren und Linken von Shadern sowie die Verwaltung von Uniforms und UniformBlocks.

Renderbuffer und Framebuffer-Klassen (FBO)
    Abstraktionen für das Offscreen-Rendering (Anforderung 5)

Zeichenbefehle
    Kapseln die Zeichenfunktionen, welche OpenGL anweisen ein Objekt zu zeichnen. Es werden die Funktionen DrawArrays und DrawElements unterstützt.

Sonstige Abstraktionen: Textur- und Textursampler-Klassen, Viewport und Hintergrundfarbe (glClearColor), OpenGL-Einstellungen (wie glEnable oder glDepthFunc), VertexArrayObjects

Higher-Level-API
================

Diese Schicht stellt im Wesentlichen Schnittstellen und häufig benötigte Implementierungen für die Aufgaben bereit, die grafischen Objekte und den eigentlichen Rendervorgang zu beschreiben, der jene Objekte schließlich "auf den Bildschirm bringt". 
Zur Implementierung werden die von der Low-Level-API bereitgestellten OpenGL-Abstraktionen genutzt.

In dieser Bibliothek wird ein solcher Rendervorgang durch sogenannte **RenderStages** beschrieben.
Objekte, die von solchen RenderStages angezeigt werden können werden als **Drawable** bezeichnet. 

.. _drawable:

Drawable
---------

Zu zeichnende Objekte werden durch eine Klasse beschrieben, welche von einer Basisklasse ``Drawable`` abgeleitet ist.
Solche Drawable-Klassen müssen eine Beschreibung der Geometrie (Trait ``Mesh``), der Position und Größe (``Transformation``) und der Darstellungsweise (``Effect``) enthalten.
Die Implementierung ist dabei sehr flexibel möglich und kann an die Anforderungen des konkret dargestellten Objekts und der Anwendung angepasst werden. 

In den Traits sind nur Methoden vorgegeben, welche die von einem "Renderer" benötigten Daten liefern müssen:

* ``Mesh`` stellt dem Renderer die Zeichenbefehle sowie Vertex-Attribute bereit, üblicherweise sind das Vertexkoordinaten, Normalen und Texturkoordinaten.
* ``Transformation`` liefert die Transformationsmatrix des Grafikobjekts.
* ``Effect`` ist für die Bereitstellung von Shader-Beschreibungen und zugehörigen Uniforms zuständig.

Ein Renderer kann selbst implementiert werden oder es kann eine ``RenderStage`` (nächster Abschnitt) dafür konfiguriert und genutzt werden.

Drawables stellen im Normalfall eine Schnittstelle für die Anwendung bereit, über die sich Attribute des Grafik-Objektes auslesen und setzen lassen.
So könnte eine Transformation, die für ein bewegliches Objekt eingesetzt wird, einen Setter bereitstellen, der das Verändern der aktuellen Position erlaubt.

Die Render-Bibliothek stellt eine Reihe von Implementierungen dieser Traits zur Verfügung. 
Diese sind zwar auf die Bedürfnisse des i>PM3D-Projekts abgestimmt, aber möglichst allgemein gehalten und damit wiederverwendbar.

Sinnvollerweise werden Drawables erstellt, indem Traits zusammengemischt werden, die die genannten Basis-Traits ``Mesh``, ``Transformation`` und ``Effect`` implementieren.
So kann mit diesem Konzept beispielsweise ein Würfel definiert werden, indem eine entsprechende ``Mesh``-Implementierung erstellt wird.
Durch die Verwendung von unterschiedlichen ``Effect``-Traits können auf einfachem Wege verschieden dargestellte Varianten eines Objekts erstellt werden.

:num:`Abbildung #drawable-classdiag` zeigt dieses Drawable-Konzept an einem Beispiel. Es wird nur eine Auswahl der Methoden dargestellt.


.. _drawable-classdiag:

.. figure:: _static/diags/drawable-classdiag.eps
    :width: 16.5cm

    Zusammensetzung eines farbigen Würfels aus den Basis-Traits

Effects selbst können relativ kompliziert aufgebaut sein. Es ist sinnvoll, diese wieder aus verschiedenen Traits zusammenzusetzen, die Teilfunktionalitäten implementieren.
Solche Traits sind in der Render-Bibliothek mit der Endung ``-Addon`` versehen. 
Beispielsweise existiert ein ``PhongLightingAddon`` für die Bereitstellung von Lichtparametern und ein ``TextDisplayAddon``, welches die Anzeige von Schrift auf den Objekten implementiert.

:num:`Abbildung #effect-classdiag` zeigt ein Beispiel für einen ``Effect``, der aus zwei ``Addons`` zusammengesetzt wird. 
Addons stellen oft Uniforms (``material-`` und ``lightUniforms`` im Beispiel) zur Verfügung, die im Effect kombiniert und von der ``uniforms``-Methode zurückgegeben werden.
Mittels der Methoden diffuse und specular kann die Anwendung die Reflexionseigenschaften eines Objekts verändern.

.. _effect-classdiag:

.. figure:: _static/diags/effect-classdiag.eps
    :width: 16.5cm

    Zusammengesetzter PhongMaterialEffect

Ressourcen, die potenziell von vielen verschiedenen Drawables geteilt werden können werden im Drawable nur durch eine abstrakte Beschreibung dargestellt. 
Texturen werden über eine ``TextureDefinition`` beschrieben; Shaderquelldateien über eine ``ShaderDefinition``. 

.. _renderstage:

RenderStage
-----------

``RenderStages`` sind für das Zeichnen der grafischen Objekte zuständig. Die Anwendung übergibt einer ``RenderStage`` einmal pro Frame\ [#f1]_ alle zu zeichnenden ``Drawables``. 
Diese werden in der bereitgestellten Implementierung der RenderStage zuerst sortiert und anschließend gezeichnet. 
Eine Sortierung wird durchgeführt, um transluzente Objekte (Anforderung 7) in der richtigen Reihenfolge zu zeichnen sowie um unnötige Zeichenoperationen und OpenGL-Zustandswechsel zu vermeiden.
Durch Angabe einer Render-Priorität in den Drawables kann manuell eine bestimmte Reihenfolge erzwungen werden, wenn dies für spezielle Zeichenaufgaben nötig ist.

Von der ``RenderStage`` werden zu den von Drawables definierten Texture- und ShaderDefinitions Objekte der Low-Level-API nach Bedarf erzeugt.
Diese werden für das Zeichnen von mehreren Drawables wiederverwendet, um Grafikspeicher und Zeit zu sparen.

Abgegrenzte Funktionalitäten können in ein ``RenderStagePlugin`` ausgelagert werden. 
So stellt die Render-Bibliothek unter anderem Plugins für die Verwaltung von Texturen und die Umsetzung von Lichtquellen bereit.

:num:`Abbildung #renderstage-classdiag` zeigt eine zusammengesetzte ``RenderStage``.

.. _renderstage-classdiag:

.. figure:: _static/diags/renderstage-classdiag.eps
    :width: 16cm

    RenderStage mit eingemischten Plugin-Traits


Weitere Abstraktionen
---------------------

Licht 
^^^^^^

Die Render-Bibliothek unterstützt das Phong-Beleuchtungsmodell, welches pixelgenau ausgewertet wird. 
Für die Anwendung werden Klassen bereitgestellt, die die von "altem" OpenGL bekannten "Lichtquellen" bereitstellen und sich an deren Schnittstelle orientieren. 
Lichtquellen können entweder entfernungsabhängig (``PositionalLight``) oder -unabhängig sein (``DirectionalLight``).

Implementiert wird die Beleuchtung auf Scala-Seite durch das Zusammenspiel des ``PhongLightingRenderStagePlugins`` mit dem Effect-Addon ``PhongLightingAddon``. 
Die eigentlichen Lichtberechnungen wurden in GLSL-Shaderfunktionen implementiert, die von verschiedenen Fragment-Shadern genutzt werden können.

Kamera
^^^^^^

Die Klasse ``Camera`` repräsentiert eine bewegliche und rotierbare Kamera, die einer ``RenderStage`` zugewiesen werden kann und damit die Perspektive des Betrachters festlegt.
Es werden die von OpenGL bekannten Funktionen (als Methoden von ``Camera``) angeboten, die eine perspektivische (``glFrustum``, ``gluPerspective``, ``gluLookAt``) oder orthogonale Projektion (``glOrtho``) konfigurieren.
Außerdem stellt die Klasse verschiedene Methoden bereit, die für Umrechnungen von Bildschirm- in 3D-Raumkoordinaten und umgekehrt genutzt werden können (analog zu den OpenGL-Funktionen ``glProject`` und ``gluUnProject``).

Diese werden im Projekt von Eingabegeräten genutzt, die mit 2D-Daten arbeiten und diese beispielsweise für die Auswahl von 3D-Objekten entsprechend umrechnen müssen.
Aufgrund der von Simulator X geforderten Komponentenaufteilung werden die Methoden von den Nutzern nicht direkt aufgerufen, sondern von der :ref:`renderkomponente` gekapselt. 
Nutzer müssen analog zu den Methoden definierte Nachrichten verwenden, die über das Kommunikationssystem von Simulator X verschickt werden.

COLLADA2Scala-Compiler
======================

Das Laden von Modellen direkt aus COLLADA-XML-Dateien ist relativ zeitaufwändig. 
Außerdem unterstützt der genutzte COLLADA-Loader :cite:`uli` bisher noch nicht die Wiederverwendung der geladenen Modelldaten. 
So wird für jede Instanz eines solchen 3D-Modells zusätzlicher Grafikspeicher belegt. 
Ein weiteres Problem ist, dass der Loader "fertige" ``Drawables`` liefert, die nicht für die Darstellung von Modellelementen (Knoten und Kanten) genutzt werden können. 

Aufgrund dessen wurde ein "Compiler" entwickelt, der mit Hilfe des COLLADA-Loaders ein Modell lädt und daraus eine Repräsentation der in dem Modell definierten Geometrie in Scala-Code erzeugt. 
Die so erzeugte Scala-Quelldatei enthält ein Trait, das :ref:`Mesh<drawable>`) implementiert. 

Optional kann direkt eine .jar-Datei erstellt werden.

Am Ende des Kapitels wird im Anwendungsbeispiel die Nutzung des COLLADA2Scala-Compilers demonstriert.


Spezielle Erweiterungen für i>PM3D
==================================

In diesem Abschnitt werden abschließend die Erweiterungen vorgestellt, die speziell für die Realisierung der Prozessvisualisierung bereitgestellt werden (:ref:`Anforderung (f) <anforderungen>`).
Hier wird auch gezeigt, wie die oben beschriebenen Ebenen der Render-Bibliothek und die GLSL-Shader zusammenwirken.
Außerdem soll verdeutlicht werden, wie :ref:`Drawables <drawable>` als Schnittstelle zwischen grafischer Darstellung und Anwendung dienen.


.. _erweiterung-interaction:

Unterstützung für deaktivierte, hevorgehobene und selektierte Elemente
----------------------------------------------------------------------

Für die :ref:`visualisierungsvarianten` wurde eine Fragment-Shaderfunktion erstellt, welche die Farbe eines Objektes abhängig von den aktivierten Visualisierungsvarianten verändern kann.
Ein Shader, der diese Funktion nutzt, definiert Uniforms, mit welchen die Varianten ausgewählt werden können.

Auf Scala-Seite werden diese Uniforms vom ``SelectionHighlightAddon`` verwaltet, welches auch eine Schnittstelle für die Anwendung bereitstellt. 

Die Varianten lassen sich über im Addon definierte Setter aktivieren:

.. code-block:: scala

    drawable.disabled = false
    drawable.highlighted = false
    drawable.selectionState = DrawableSelectionState.Selected

Durch den Aufruf eines solchen Setters wird die zugehörige Uniform geändert und die Änderung somit zum Shaderprogramm weitergegeben, nachdem etwaige Konvertierungen durchgeführt wurden.

Zusätzlich können noch folgende Parameter gesetzt werden:

* ``borderWidth``: Breite des Selektionsrahmens.
* ``highlightFactor``: Wert, mit dem die berechnete Farbe multipliziert wird um Hervorhebung darzustellen. Bei dunklen Grundfarben wird stattdessen mit 1 / ``highlightFactor`` multipliziert.

"Deaktiviert" wird durch einen Grauton dargestellt, der wie folgt aus den Komponenten der Grundfarbe berechnet wird: ``grauwert = (rot + blau + grün) * 0.2``. 

Außerdem wird das Objekt transluzent gezeichnet.
Der Selektionsrahmen wird im deaktivierten Zustand abhängig von der resultierenden Helligkeit von "grauwert" entweder hellgrau oder dunkelgrau dargestellt.

Die Shaderfunktion zeichnet den "Selektionsrahmen" abhängig von den (2D)-Texturkoordinaten, die üblicherweise von 0 bis 1 reichen. 
Auf jeder Seite wird ein Bereich mit der Breite "borderWidth" als Rahmen in der Komplementärfarbe zum Hintergrund gezeichnet.

So wird durch die Texturkoordinaten die Form des Rahmens definiert; für die in der Arbeit verwendeten Objekte war dies ausreichend. 
Jedoch könnten sich bei anderen Figuren Probleme ergeben, da die Texturkoordinaten auch für die Ausrichtung der Textur oder der Schrift genutzt werden.
Für solche Objekte könnte allerdings leicht ein zusätzliches Vertex-Attribut definiert werden, welches die Koordinaten für die Positionierung des Rahmens liefert.\ [#f5]_


.. _schrift-rendering:

Darstellung von Text
--------------------

Für die Beschriftung von Modellknoten wurde eine gut lesbare und trotzdem einfach umsetzbare Technik für das Rendering von Schrift benötigt.
Hierfür wurde die 2D-API (``java.awt``) der Java-Klassenbibliothek zur Hilfe genommen. 
Zur Verwendung mit OpenGL wird die Schrift in eine Textur geschrieben, die dann auf die Objekte aufgebracht werden kann.
Um die Darstellungsqualität zu erhöhen, wird die Antialiasing-Funktion von Graphics2D genutzt. 

Zur Darstellung von Text müssen Drawables den Trait ``TextDisplayAddon`` einmischen und die genutzte ``RenderStage`` muss die Plugins ``TextDisplayRenderStagePlugin`` sowie ``TextureRenderStagePlugin`` einbinden.
Im RenderStagePlugin wird bei jeder Änderung des Textes oder Schrifteinstellungen die Schrift-Textur neu erstellt, so dass im nächsten Frame der neue Text angezeigt wird.

Der Text kann im Drawable mit 

.. code-block:: scala

    drawable.text = "irgendein Text" 

verändert werden. Außerdem werden Einstellmöglichkeiten für die Schriftart, -größe und -stil (``java.awt.Font``) und die Schriftfarbe (``java.awt.Color``) angeboten.

Der Text wird zentriert angezeigt und am Wortende umgebrochen, falls der horizontale Platz nicht ausreicht. 
Die "Schriftgröße" wird als Mindestgröße interpretiert; falls ein Objekt eine Skalierung größer eins aufweist, wird die Größe der Schrift proportional mitskaliert. 
Bei einer Skalierung kleiner eins wird der für die Schrift zur Verfügung stehende Platz verkleinert. 

Die Skalieroperationen werden von einer Shaderfunktion realisiert.

Um auch bei größeren Entferungen von der Kamera und kleiner Schrift noch eine angemessene Lesbarkeit zu erreichen kann Mipmapping genutzt werden, das auch von der Render-Bibliothek unterstützt wird. 
Aufgrund von Problemen mit verschiedenen Grafikkarten, die für das Projekt getestet wurden, ist dies standardmäßig jedoch nicht aktiviert.

SVarSupport - Einbindung der Modell-Drawables in i>PM3D
-------------------------------------------------------

Visualisierungsparameter der Modellelemente werden über die ``ModelComponent`` bereitgestellte :ref:`SVars<modellanbindung-svars>` gesetzt. 
Den SVar-Wert zu verändern hat alleine noch keinen Effekt; die Wertänderungen müssen an die Drawables weitergeleitet werden, welche die Anbindung an die Grafikschnittstelle realisieren.

Die Verbindung der SVars mit den Attributen der Drawables erfolgt über Traits, die das Trait ``SVarSupport`` implementieren. 
Solche ``SVarSupports`` werden in Modell-Drawables eingemischt, wie im Anwendungsbeispiel im folgenden Abschnitt gezeigt wird.

Diese Traits stellen eine Methode, ``connectSVars`` bereit, die von der Renderkomponente aufgerufen wird nachdem diese ein Drawable erzeugt hat.
So werden in dieser Methode für alle vom Trait unterstützten SVars *Observe-Handler* registriert, die bei jeder Änderung des SVar-Wertes aufgerufen werden.
Üblicherweise leiten diese Funktionen die neuen Werte an Setter des Drawables weiter, wie sie in den vorherigen Abschnitten gezeigt wurden.

Für SVars, deren Typ erst zur Laufzeit bekannt ist, kann der Methode eine "Ersetzungsliste" übergeben werden.
Eine solche Ersetzung ist beispielsweise für die Darstellung von Text auf Modellknoten nötig. 
Im :ref:`Editor-Metamodell<ebl-figures>` wird festgelegt, welches *Domain*-Attribut als Text dargestellt werden soll.
Die Modellkomponente liest den Namen des Attributs aus und definiert eine Ersetzung der ``Text``-SVar durch die entsprechend benannte :ref:`Domain-Model-SVar<modellanbindung-svars>`.
Beispielsweise wird für einen Prozessknoten die ``Text``-SVar durch die ``model.function``-SVar ersetzt. 
Für letztere SVar wird so ein *Observe-Handler* registriert, der dem Setter für den aktuellen Text des Drawables jede Änderung an ``model.function`` weitergibt.
Aufgrund dessen wird bei jeder Änderung an dieser SVar (bspw. durch eine Eingabe in einem Menü) der sichtbare Text auf dem Grafikobjekt angepasst.

Das gleiche Prinzip wird für Visualisierungsparameter (bspw. Farben oder die Schriftart) aus dem Editor-Modell angewendet.
:num:`Abbildung #farbe-setzen-aktivitaet` zeigt beispielhaft, wie sich eine Änderung an der Hintergrundfarbe eines Prozessknotens Benutzer auswirkt und welche Schritte vom Editor-Menü bis zur Grafikkarte durchlaufen werden.

Beispiele für SVarSupports
^^^^^^^^^^^^^^^^^^^^^^^^^^

Für die im vorherigen Unterabschnitt beschriebene Textdarstellung wird das Trait ``TextDisplaySVarSupport`` angeboten.
Im Normalfall wird dieses zusammen mit dem ``BackgroundSVarSupport`` genutzt, welches das Setzen der Hintergrundfarbe übernimmt. 
Das Trait ``SelectionHighlightSVarSupport`` stellt die Anbindung der :ref:`Visualisierungsvarianten<erweiterung-interaction>` bereit.


.. _farbe-setzen-aktivitaet:

.. figure:: _static/diags/farbe-setzen-aktivitaet.eps
    :width: 17cm

    Ablauf bei Änderung der Hintergrundfarbe eines Prozesses durch den Benutzer


.. raw:: latex

    \pagebreak

.. _beispiel-neue-modellfigur:

Anwendungsbeispiel: Erstellen von neuen Modell-Figuren
======================================================

Zum Abschluss wird gezeigt, wie sich ein neues Grafikobjekt erstellen lässt, das für die Visualisierung eines Knotens eingesetzt werden soll.
Dies ist die Fortsetzung des :ref:`Anwendungsbeispiels<beispiel-neues-element>` für das Hinzufügen eines neuen Modellelements zum Metamodell.

Die Geometrie des Objekts kann zum Einen manuell erstellt werden, indem das Trait ``Mesh`` implementiert wird. 
Als Vorlage kann eines der mitgelieferten Meshes, wie ``mmpe.renderer.mesh.UnitCube`` genutzt werden.

Einfacher ist die Nutzung des COLLADA2Scala-Compilers, der wie folgt aufgerufen werden kann (Linux):

.. code-block:: bash
    
   $ collada2scala pyramid.dae test.Pyramid pyramid.jar

Damit wird aus einer COLLADA-Datei pyramid.dae eine pyramid.jar erstellt, die im Package test einen Mesh ``Pyramid`` enthält.

Im nächsten Schritt wird die neue Figur zum object ``mmpe.model.BaseDrawables`` hinzugefügt.

Es soll eine Pyramide erstellt werden, auf deren Seiten Text angezeigt werden kann.
Dafür kann beispielsweise die ``TextBox`` als Vorlage genommen und wie folgt abgeändert werden:

.. code-block:: scala
    :linenos:
    :emphasize-lines: 1,2
    
    class TextPyramid extends EmptyDrawable("textPyramid")
      with Pyramid
      with SiXTransformation
      with SelectableAndTextEffect
      with SelectionHighlightSVarSupport
      with TextDisplaySVarSupport
      with BackgroundSVarSupport

Geändert wurde nur die ``Mesh``-Implementierung in der 2. Zeile sowie der Name des Objekts in der 1. Zeile.

Abschließend wird in ``mmpe.model.ModelDrawableFactory`` zur Fallunterscheidung in der Methode createDrawables ("figureFqn match ...") ein weiterer Fall hinzugefügt:

.. code-block:: scala

      case "test.TextPyramid" =>
        val drawable = new TextPyramid
        Seq(drawable)

Nun kann die texturierte Pyramide im Editor-Metamodell genutzt werden.
Das hier angegebene "test.TextPyramid" entspricht dem ``scalaType``, wie er im Metamodell gesetzt muss werden muss um diese Figur zu referenzieren.


Anmerkungen
-----------

Am ersten Code-Beispiel ist zu sehen, wie ein Drawable für ein Modellelement prinzipiell definiert wird. 
Die Zeilen 2 bis 4 geben die von :ref:`Drawable<drawable>` geforderte Implementierung an.
Es wird von der Renderkomponente vorausgesetzt, dass ``SiXTransformation`` für alle 3D-Drawables genutzt wird.
``SelectableAndTextureEffect`` wird für alle texturierten Figuren genutzt, die die :ref:`Visualisierungsvarianten<erweiterung-interaction>` unterstützen.
Analog dazu ist ``SelectableAndTextEffect`` für die Textdarstellung definiert, welcher das :ref:`TextDisplayAddon<schrift-rendering>` nutzt.

In den letzten drei Zeilen werden die ``SVarSupports`` eingemischt, welche die Verbindung zur Anwendung herstellen.

.. [#f6] Zu finden im Package mmpe.renderer.gl

.. [#f7] Package mmpe.renderer

.. [#f1] Wie in "Frames Per Second" (FPS). Damit ist ein "Einzelbild" gemeint.

.. [#f2] Der "Flaschenhals" der Anwendung ist eher die Physikkomponente. Die Ursachen wurden nicht näher untersucht, da immerhin mehrere hundert Modellelemente auf aktuellen Systemen noch relativ schnell dargestellt werden können.

.. [#f3] Gezeigt wird dies von :cite:`alvergren_3d_2009`; :ref:`siehe <krolovitsch>`. Dort sei die C++-Game-Engine Panda3D (über eine Python-Anbindung) genutzt worden, um schnell einen Prototypen für einen 3D-Zustandsdiagramm-Editor zu erstellen.

.. [#f5] Dies ist auch ein Beispiel, dass die Flexibilität von modernem OpenGL (und der Render-Bibliothek) zeigt, die im Gegensatz zu alten OpenGL-Versionen beliebige Vertex-Attribute unterstützen.
