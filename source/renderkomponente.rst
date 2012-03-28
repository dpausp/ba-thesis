.. _renderkomponente:

*****************
Render-Komponente
*****************

In diesem Kapitel wird die Renderkomponente vorgestellt, die die Grafikfunktionen\ [#f4]_ für I>PM3D bereitstellt.
Von dieser Komponente wird die im Rahmen dieser Arbeit entstandene :ref:`render-bibliothek` an Simulator X angebunden.

Damit wird die von Simulator X bereitgestellte Komponente für die grafische Darstellung ersetzt, deren Fähigkeiten nicht ausreichten, um die hier vorgestellte Visualisierung auf einfachem Wege zu implementieren.

Die eigentlichen Render-Aufgaben werden an einen Actor (``MMPERenderActor``) delegiert, der von der Renderkomponente (Klasse ``MMPERConnector``) gestartet wird.\ [#f1]_
Nachrichten, die Grafikfunktionen betreffen werden von anderen Komponenten an die Renderkomponente geschickt und an den RenderActor weitergeleitet. 

RenderActor
-----------

Für den RenderActor musste ein eigener Scheduler für Scala-Actors erstellt werden, da der Standard-Scheduler die Actor-Arbeitspakete in beliebig wechselnden Threads ausführen kann. 
Dies ist für die Ausführung von OpenGL-Funktionen problematisch. Deswegen wird die Ausführung der Render-Aufgaben auf einen Thread beschränkt. 

Der RenderActor definiert zwei Render-Ebenen. Die zuerst gezeichnete Ebene umfasst alle 3D-Objekte wie den Prozessgraphen und Szenenobjekte. 
Darüber wird eine Ebene gezeichnet, die 2D-Elemente wie Cursor der Eingabegeräte oder das Eightpen-Menü :cite:`buchi` enthält.

Beide Ebenen werden durch jeweils eine :ref:`renderstage` gezeichnet.

Es gibt die Möglichkeit, den RenderActor durch Plugins zu erweitern, die an verschiedenen vordefinierten Erweiterungspunkten im Render-Prozess eingreifen können. 
Plugins können neue Grafikobjekte erzeugen und zu einer der beiden *RenderStages* hinzufügen oder selbst OpenGL-Funktionen ausführen.

Dies wird im Projekt für die Darstellung der Nifty-GUI-Menüs :cite:`uli` und des Eightpen-Menüs :cite:`buchi` genutzt.

OpenGL-Versionsproblematik
--------------------------

Die Library Nifty-GUI bringt eine eigene Render-Implementierung auf Basis von OpenGL 1.x mit, welche auch Funktionen nutzt, die in OpenGL 3.3 als "veraltet" ("deprecated") eingestuft sind.
Von der Render-Bibliothek werden dagegen nur Funktionen genutzt, die in OpenGL 3.3 verfügbar sind.

Wegen Nifty-GUI muss der RenderActor OpenGL 3.3 im Kompatibilitätsmodus betreiben, der auch die "deprecated"-Funktionen unterstützt. 
Es ist möglich, dass dies auf manchen Hardwareplattformen zu Geschwindigkeits- oder Darstellungsproblemen führt.

Projektspezifische Erweiterungen
--------------------------------

Die Renderkomponente unterstützt die von Simulator X bereitgestellten RenderAspects für die Definition von Lichtquellen-Entities (``PointLight``) sowie den Aspect ``ShapeFromFile``, der die Renderkomponente anweist, die grafische Repräsentation einer Entity aus einer COLLADA-Modelldatei zu laden.

Im Folgenden werden die Funktionalitäten aufgelistet, welche von der ursprünglichen Renderkomponente nicht unterstützt werden.

ShapeFromFactory
^^^^^^^^^^^^^^^^

Mir der hier entwickelten Renderkomponente ist es möglich, die grafische Repräsentation einer Entity von einer Factory-Klasse oder -Actor erzeugen zu lassen. 
Damit lassen sich in der Anwendung beliebige Grafikobjekte nutzen, die mit Hilfe der :ref:`render-bibliothek`: erstellt wurden.

Hierfür ist der RenderAspect ``ShapeFromFactory`` definiert.
Dies wird im Projekt für die Erstellung der Grafikobjekte für Modellelemente – also der Knoten und Kanten des Prozessmodells – genutzt.

Modellierungsflächen
^^^^^^^^^^^^^^^^^^^^

Für die Darstellung von :ref:`modellierungsflaechen` wurde ein ``PlaneAspect`` bereitgestellt, der SVars für die Konfiguration von Transluzenz, Farbe, Linenbreite und -dichte und das Deaktivieren der Linien definiert.

2D-Grafikobjekte
^^^^^^^^^^^^^^^^

Der Aspect ``Image2D`` wird dafür genutzt, bewegliche und skalierbare 2D-Grafikobjekte darzustellen. 
Bei der Erstellung muss ein Pfad zu einer Bilddatei angegeben werden. Position und Größe sind über SVars modifizierbar. 
Diese Werte können entweder über die Bildschirm-Pixelkoordinaten oder über normalisierte Koordinaten (von -1 bis +1 in x und y-Richtung) angegeben werden. 
Welches Koordinatensystem genutzt wird, wird über den Konstruktor in ``coordsAreNormalized`` angegeben.

User-Entity
^^^^^^^^^^^

Simulator X stellt eine ``User``-Entity bereit, über deren SVars ``HeadTransform`` und ``ViewPlatform`` die Position des Benutzers in der 3D-Szene bestimmt wird.
Diese wird von der Renderkomponente erzeugt, die zusätzliche SVars definiert, über die Viewport\ [#f5]_ - (Aspect ``ViewportSettings``) und Render-Frustum-Einstellungen \ [#f6]_ (Aspect ``FrustumSettings``) abgefragt werden können\ [#f3]_.


.. [#f1] Dieser Aufbau ergibt sich aus der Idee, für die Darstellung der Szene mehrere Bildschirme nutzen zu können, wie es unter Anderem für ein CAVE-System nötig wäre. Dazu könnten der Renderkomponente mehrere RenderActors zugeordnet werden. Dies war vorgesehen, wird jedoch nicht überall in der Implementierung umgesetzt und daher nicht unterstützt.

.. [#f3] Die Werte lassen im Prinzip sich auch verändern, nur wird dies von der Implementierung noch nicht vollständig unterstützt. 

.. [#f4] Die Implementierung umfasst auch die Übersetzung von Tastatur- und Mausdaten, die von LWJGL geliefert werden, in Simulator X - Events. Für diese Arbeit sind aber nur die Grafikfunktionen relevant.

.. [#f5] Größe und Nullpunkt der Zeichenfläche für OpenGL, angegeben in Pixel.

.. [#f6] Diese Einstellungen legen die perspektivische Projektion fest. :cite:`www:frustum`
