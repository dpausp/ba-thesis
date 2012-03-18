*****************
Render-Komponente
*****************

In diesem Kapitel wird die Renderkomponente vorgestellt, die die Grafikfunktionen\ [#f4]_ für I>PM3D bereitstellt.
Anhang A beschreibt die :ref:`render-bibliothek`, die von dieser Komponente für die Grafikausgabe genutzt wird.
Die 

Dadurch wird die die von Simulator X bereitgestellte Komponente für die grafische Darstellung ersetzt, deren Fähigkeiten nicht ausreichten, um die hier vorgestellte Visualisierung auf einfachem Wege zu implementieren.

Die eigentlichen Render-Aufgaben werden an einen Actor (*MMPERenderActor*) delegiert, der von der Renderkomponente (Klasse *MMPERConnector*) gestartet wird.\ [#f2]_
Nachrichten, die Grafikfunktionen betreffen werden von anderen Komponenten an die Renderkomponente geschickt und an den RenderActor weitergeleitet. 

RenderActor
-----------

Für den RenderActor musste ein eigenen Scheduler für Scala-Actors erstellt werden, da der Standard-Scheduler die Actor-Arbeitspakete in beliebig wechselnden Threads ausführen kann. 
Dies ist für die Ausführung von OpenGL-Funktionen problematisch. Deswegen wird die Ausführung der Render-Aufgaben auf einen Thread beschränkt. 

Der RenderActor definiert zwei Render-Ebenen. Die zuerst gerenderte Ebene umfasst alle 3D-Objekte wie den Prozessgraphen und Szenenobjekte. 
Darüber wird eine Ebene gezeichnet, die 2D-Elemente wie Cursor der Eingabegeräte oder das Eightpen-Menü :cite:`buchi` enthält.

Beide Ebenen werden durch jeweils eine separate :ref:`render-stage` gezeichnet.

Es gibt die Möglichkeit, den RenderActor durch Plugins zu erweitern, die an verschiedenen vordefinierten Erweiterungspunkten im Render-Prozess eingreifen können. 
Plugins können neue Grafikobjekte erzeugen und zu einer der beiden RenderStage hinzufügen oder selbst OpenGL-Funktionen ausführen.

Dies wird im Projekt für die Darstellung der Nifty-GUI-Menüs :cite:`uli` und des Eightpen-Menüs :cite:`buchi` genutzt.

OpenGL-Versionsproblematik
--------------------------

Die Library Nifty-GUI bringt ihre eigene Render-Implementierung auf Basis von OpenGL 1.x mit, die auch Funktionen nutzt, die in OpenGL 3.3 als "veraltet" ("deprecated") eingestuft sind.
Von der Render-Bibliothek werden nur Funktionen genutzt, die in OpenGL 3.3 verfügbar sind.

Wegen Nifty-GUI muss der RenderActor OpenGL 3.3 im Kompatibilitätsmodus betreiben, der auch die "deprecated" Funktionen unterstützt. 
Es ist daher möglich, dass dies auf manchen Hardwareplattformen zu Geschwindigkeits- oder Darstellungsproblemen führt.

Für zukünftige Weiterentwicklungen des Projekts wäre es daher angebracht, eine eigene Menüimplementierung auf Basis der Renderbibliothek zu erstellen.\ [#f2]_

Projektspezifische Erweiterungen
--------------------------------

Die Renderkomponente unterstützt die von Simulator X bereitgestellten RenderAspects für die Definition von Lichtquellen-Entities (*PointLight*) sowie den Aspekt *ShapeFromFile*, der die Renderkomponente anweist, die grafische Repräsentation einer Entity aus einer COLLADA-Modelldatei zu laden.

ShapeFromFactory
^^^^^^^^^^^^^^^^

Zusätzlich wurde es ermöglicht, die grafische Repräsentation von einer Factory-Klasse oder -Actor erzeugen zu lassen.
Dafür ist der RenderAspect *ShapeFromFactory* definiert.
Dies wird im Projekt für die Erstellung der grafischen Repräsentationen der Modellelemente – also der Knoten und Kanten des Prozessmodells – genutzt.

Modellierungsflächen
^^^^^^^^^^^^^^^^^^^^

Für die Darstellung von :ref:`modellierungsflaechen` wurde ein *PlaneAspect* bereitgestellt, der SVars für die Konfiguration von Transluzenz, Farbe, Linenbreite und -dichte und das Deaktivieren der Linien definiert.

2D-Grafikobjekte
^^^^^^^^^^^^^^^^

Der Aspect *Image2D* wird dafür genutzt, bewegliche und skalierbare 2D-Grafikobjekte darzustellen. 
Bei der Erstellung muss ein Pfad zu einer Bilddatei angegeben werden. Position und Größe sind über SVars modifizierbar. 
Diese Werte können entweder über die Bildschirm-Pixelkoordinaten oder über normalisierte Koordinaten (von -1 bis +1 in x und y-Richtung) angegeben werden. 
Welches System genutzt wird, wird über den Konstruktor mit *coordsAreNormalized* angegeben.

User-Entity
^^^^^^^^^^^

Simulator X stellt eine User-Entity bereit, über deren SVars *HeadTransform* und *ViewPlatform* die Position des Benutzers in der 3D-Szene bestimmt wird.
Diese wird von der Renderkomponente erzeugt, die zusätzliche SVars definiert, über die Viewport- (*ViewportSettings*) und Render-Frustum-Einstellungen (*FrustumSettings*) abgefragt werden können.\ [#f3]_


.. [#I] Zu finden im Package mmpe.renderer.gl


.. [#f1] Dieser Aufbau ergibt sich aus der Idee, für die Darstellung der Szene mehrere Bildschirme nutzen zu können, wie es unter Anderem für ein CAVE-System nötig wäre. Dazu könnten der Renderkomponente mehrere RenderActors zugeordnet werden. Dies war vorgesehen, wird jedoch nicht überall in der Implementierung umgesetzt und daher nicht unterstützt.

.. [#f2] Oder darauf zu hoffen, dass sich bei Nifty-GUI etwas ändert...

.. [#f3] Die Werte lassen im Prinzip sich auch verändern, nur wird dies von der Implementierung noch nicht vollständig unterstützt.

.. [#f4] Die Implementierung umfasst auch die Übersetzung von Tastatur- und Mausdaten, die von LWJGL geliefert werden, in Simulator X - Events. Für dieses Kapitel sind aber nur die Grafikfunktionen relevant.
