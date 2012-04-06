**********
Einleitung
**********

In der Computergrafik zeigte sich in den letzten Jahrzehnten eine bemerkenswerte Entwicklung der Möglichkeiten von Hardware und Software.
Interaktive 3D-Grafikanwendungen, die früher nur mit sehr großem Aufwand und finanziellem Einsatz realisierbar waren, sind heute für handelsübliche PCs verfügbar. 
Besonders deutlich wird diese Entwicklung im Bereich der Computerspiele, die es heute dem Spieler erlauben, in eine nahezu fotorealistische Darstellung der realen Welt einzutauchen und mit 3D-Objekten zu interagieren :cite:`porcino_gaming_2004`.

Neben den seit Jahrzehnten verbreiteten Eingabegeräten Tastatur und Maus lassen sich Spiele mittlerweile auch mit neuartigen Eingabegeräten – wie der Nintendo Wii oder Microsoft Kinect – bedienen :cite:`sung_recent_2011`. 
So lassen sich Bewegungen des Benutzers direkt in den dreidimensionalen Raum übertragen, wodurch eine natürliche Interaktion mit der virtuellen Welt ermöglicht wird. 

Auch in "ernsthaften" Anwendungen werden die Möglichkeiten der 3D-Computergrafik genutzt. 
Beispielsweise werden in der Bioinformatik Werkzeuge wie *PyMol* :cite:`schrodinger_pymol_2010` eingesetzt, welche eine Visualisierung von dreidimensionalen Strukturen ermöglichen, welche entscheidend für die Funktion von Biomolekülen sind. 
Durch Interaktion per Tastatur und Maus lässt sich das Molekül aus verschiedenen Perspektiven betrachten.
Das vollständige Eintauchen in eine 3D-Szene und die Interaktion durch Körperbewegungen oder auch Sprache wird von Werkzeugen zur virtuellen Konstruktion gezeigt ("virtuelle Werkstatt") :cite:`frohlich_virtuelle_2009`. 
So lässt sich beispielsweise ein rein virtuelles Auto in einer realistischen Darstellung betrachten und sogar "anfassen". 
Solche Systeme bieten teilweise auch die Möglichkeit, dass mehrere Benutzer gleichzeitig mit dem System interagieren können.

In den genannten Anwendungsfällen sind die Vorteile einer dreidimensionalen Darstellung offensichtlich, da hier Objekte aus der realen Welt abgebildet werden, die grundsätzlich dreidimensional ist.

Motivation für die 3D-Modellierung von Prozessen
================================================

Die vorliegende Arbeit beschäftigt sich jedoch vorrangig mit der Modellierung von Prozessen, welche die Aufgabe hat, (Geschäfts-)Abläufe und zugehörige Informationen in einer abstrahierten Form darzustellen. 
Hierbei ist es weniger leicht festzustellen, welche Vorteile eine dreidimensionale Visualisierung hätte und wie diese Darstellung überhaupt aussehen könnte.

Prozessmodelle dienen unter anderem der Kommunikation zwischen den an der Entwicklung oder Ausführung eines Prozesses beteiligten Personen ("Stakeholder"), für welche die Darstellung leicht verständlich und informativ sein sollte :cite:`brown_conceptual_2010`.
Prozessmodellierungswerkzeuge, wie beispielsweise *ARIS (Express)* :cite:`scheer_aris_2000` oder das später in dieser Arbeit gezeigte *i>PM*\ :sup:`2` :cite:`roth_konzeption_2011`, nutzen bisher ausschließlich 2D-Darstellungen. 
Das Bedienkonzept dieser Anwendungen folgt den Standards der seit zwei bis drei Jahrzehnten üblichen Desktopprogramme, welche eine zweidimensionale grafische Benutzeroberfläche anbieten und mit Tastatur und Maus bedient werden ("WIMP"-Oberflächen\ [#f1]_).

Der Einsatz der dritten Dimension für die Repräsentation von Prozessen wurde jedoch schon vereinzelt untersucht. 
Beispielsweise wird von :cite:`betz_3d_2008` :ref:`gezeigt<betz>`, wie sich dies für die Visualisierung von Beziehungen zwischen mehreren Modellen oder zur Darstellung von hierarchischen Modellen sinnvoll nutzen lässt. 
So könnten sich Vorteile zu einer 2D-Darstellung ergeben, welche unter anderem weniger Möglichkeiten bietet, verschiedene Arten von Beziehungen zwischen Modellelementen in leicht verständlicher Form zu visualisieren :cite:`gil_three_1998`.
Arbeiten auf dem Gebiet der Softwaremodellierung, welche in dieser Arbeit vorgestellt werden, zeigen weitere Nutzungsmöglichkeiten, die sich möglicherweise auf die Prozessmodellierung übertragen lassen. 

Prozessmodelle enthalten oft auch Konzepte, die Entitäten aus der realen Welt vertreten, beispielsweise die in einem Prozessschritt verwendete Maschine oder eine ausführende Person. 
In machen Fällen könnte es sinnvoll sein, diese Objekte in ihrem realen Erscheinungsbild neben dem Prozessmodell darzustellen, um das abstrakte Modell für Benutzer anschaulicher zu machen, wie von :cite:`brown_conceptual_2010` dargestellt wird.

Ein Prozessmodellierungswerkzeug, welches die Möglichkeiten der modernen 3D-Computergrafik ausnutzt oder gar neuartige (3D-)Eingabegeräte unterstützt, existiert bisher nicht :cite:`brown_conceptual_2010`.
Um die Effektivität von 3D-Visualisierung für die Prozessmodellierung zu beurteilen und verschiedene Darstellungsformen zu vergleichen wäre ein solches System allerdings sehr hilfreich.

Zielsetzung und Aufbau dieser Arbeit
====================================

Da es kaum Möglichkeiten gibt, die Effektivität von 3D-Prozessvisualisierungen – besonders in interaktiven Anwendungen – zu evaluieren, wurde mit dem i>PM3D-Projekt ein Prototyp eines 3D-Prozessmodellierungswerkzeugs entwickelt, welches auch neuartige (3D-)Eingabegeräte nutzt und die Anbindung von weiteren Eingabemöglichkeiten einfach macht. 
Das Projekt basiert auf :ref:`simulatorx`, einer Plattform für eine modulare, komponentenbasierte Realisierung von Anwendungen aus dem Bereich der 3D-Computergrafik.

Ein detaillierter Überblick über das Gesamtprojekt wird später in :ref:`dieser Arbeit<ipm3d>` gegeben.

Die vorliegende Arbeit beschäftigt sich im Rahmen des Projekts mit der Konzeption und Realisierung der **Repräsentation** der Prozessmodelle im Modellierungswerkzeug.
Repräsentation bezieht sich hier sowohl auf die Visualisierung der Prozessmodelle als auch auf die interne Darstellung der Modelle und deren physische Speicherung (auf Datenträgern). 

Um das Experimentieren mit der Visualisierung einfach zu machen und die Anpassung der in einem Modell verwendeten Konstrukte zu ermöglichen, werden abstrakte Syntax der Modellierungssprache und deren konkrete grafische Repräsentation in getrennten **Metamodellen** beschrieben, wie es schon durch das in :cite:`roth_konzeption_2011` entwickelte (MDF) :ref:`Model Designer Framework<mdf>` für 2D-Modelleditoren umgesetzt wird. 
So lassen sich auch gänzlich neue Elemente und dazugehörige grafische Objekte hinzufügen.
Die Modell-Hierarchie von i>PM3D wird in :ref:`dieser Arbeit<modellhierarchie>` vorgestellt.

Prinzipiell lässt sich i>PM3D durch diese Anpassbarkeit nicht nur für die Modellierung von Prozessen sondern auch für ähnliche Anwendungsdomänen einsetzen. 
Der Fokus liegt hier allerdings speziell auf der Modellierung nach dem Prinzip der :ref:`perspektivenorientierten Prozessmodellierung<popm>` und dem damit assoziierten :ref:`tvk`.
So wird ein Metamodell für diese Domäne und deren Visualisierung nach einem graphbasierten Ansatz :ref:`bereitgestellt<metamodelle>`. 
Zusammen beschreiben diese Metamodelle einen **Prozessmodell-Editor**, der den Konzepten von vergleichbaren 2D-Modellierungswerkzeugen und der daraus bekannten Visualisierung folgt (siehe :ref:`visualisierung`).

Für den Zugriff auf die interne Repräsentation der Modelle muss eine Schnittstelle bereitgestellt werden, über die andere Komponenten der Anwendung Parameter zur Laufzeit verändern können, welche die grafische Repräsentation oder das Prozessmodellelement selbst (bspw. die Funktion eines Prozessknotens) betreffen.
Ebenfalls werden für ein Modellierungswerkzeug übliche Funktionen wie das Neuerstellen, Laden und Speichern von Modellen (aus einer textuellen Repräsentation) angeboten.
Diese sog. :ref:`Modellanbindung` nutzt hierbei die von Simulator X bereitgestellten Möglichkeiten zur Kommunikation zwischen den Komponenten der Anwendung.

Für die Implementierung der 3D-Visualisierung, insbesondere für das leichte Hinzufügen von neuen grafischen Modellobjekten und die Realisierung von speziell für einen Modelleditor benötigten :ref:`grafischen Effekte<visualisierung>` stand keine geeignete Plattform zur Verfügung. 
Daher wurde auf Basis von OpenGL eine :ref:`render-bibliothek` und eine darauf aufbauende :ref:`renderkomponente` für Simulator X erstellt, die auf die Anforderungen der Arbeit zugeschnitten, aber möglichst allgemein gehalten und erweiterbar sind.

.. _anforderungen:

Funktionale Anforderungen
=========================

Zusammengefasst werden in dieser Arbeit folgende funktionale Anforderungen an den i>PM3D Prototypen realisiert:

    (a) Beschreibung der verwendeten Modellierungssprache durch Metamodelle
    (b) Modellierung von Prozessen mit einer grafischen Modellierungssprache nach einem graphbasierten Ansatz
    (c) Möglichkeit, bestehende Modellkonstrukte und deren Visualisierung zu verändern sowie neue Modellelemente hinzuzufügen
    (d) Anbindung der Modelle an die Simulator X-Anwendung und Bereitstellung von Manipulationsmöglichkeiten an Modellelementen und deren Visualisierung
    (e) Erstellen, Laden und Speichern von Modellen in textueller Form
    (f) Bereitstellung von Grafikeffekten für einen Modelleditor: Darstellung von Text und 2D-Grafiken auf Modellfiguren; Visualisierung von selektierten, hervorgehobenen und deaktivierten Modellelementen
    (g) Anzeige von textuellen Attributen aus dem Prozessmodell auf den Modellfiguren


.. [#f1] WIMP steht für "Windows, Icons, Menus, Pointer. Grafische Benutzeroberflächen, die auf die Nutzung mit anderen Eingabegeräte als Tastatur und Maus ausgelegt sind, werden auch als "Post-WIMP-Interfaces" bezeichnet. :cite:`van_dam_post-wimp_1997`
