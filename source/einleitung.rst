**********
Einleitung
**********

In der Computergrafik gab es in den letzten Jahrzehnten eine bemerkenswerte Entwicklung der Möglichkeiten von Hardware und Software.
Interaktive 3D-Grafikanwendungen, die früher nur mit sehr großen Aufwand und finanziellem Einsatz realisierbar waren, sind heute für handelsübliche PCs verfügbar. 
Besonders deutlich wird diese Entwicklung im Bereich der Computerspiele, die es heute dem Spieler erlauben, in eine nahezu fotorealistische Darstellung der realen Welt einzutauchen und mit 3D-Objekten zu interagieren.
Außer mit den seit Jahrzehnten verbreiteten Eingabegeräten Tastatur und Maus lassen sich Spiele mittlerweile auch mit neuartigen Eingabegeräten – wie der Nintendo Wii oder Microsoft Kinect – bedienen. 
So lassen sich Bewegungen des Benutzers direkt in den dreidimensionalen Raum übertragen, womit eine natürliche Interaktion mit der virtuellen Welt ermöglicht wird. 

Auch in "ernsthaften" Anwendungen werden die Möglichkeiten der 3D-Computergrafik genutzt. Beispielsweise werden in der Bioinformatik Werkzeuge wie *Pymol* :cite:`pymol`  eingesetzt, welche eine Visualisierung von dreidimensionalen Strukturen ermöglichen, welche entscheidend für die Funktion von Biomolekülen sind. Durch Interaktion per Tastatur und Maus lässt sich das Molekül aus verschiedenen Perspektiven betrachten.
Das vollständige Eintauchen in eine 3D-Szene und die Interaktion durch Körperbewegungen oder auch Sprache wird von Werkzeugen zur virtuellen Konstruktion gezeigt ("virtuelle Werkstatt") :cite:`lato`. 
So lässt sich beispielsweise ein rein virtuelles Auto in einer realistischen Darstellung betrachten und sogar "anfassen".

In diesen beiden Fällen sind die Vorteile einer dreidimensionalen Darstellung offensichtlich, da hier Objekte aus der realen Welt abgebildet werden, die prinzipiell dreidimensional sind.

Die vorliegende Arbeit beschäftigt sich jedoch vorrangig mit der Modellierung von Prozessen, welche zum Ziel hat, (Geschäfts-)Abläufe und zugehörige Informationen in einer abstrahierten Form darzustellen. 
Prozessmodelle dienen unter anderem zur Kommunikation zwischen am Prozess beteiligten Personen ("Stakeholder"), für die die Darstellung leicht verständlich und informativ sein sollte.
Hierbei ist es weniger deutlich, welche Vorteile eine dreidimensionale Visualisierung hätte und wie diese Darstellung überhaupt aussehen könnte.
Prozessmodellierungswerkzeuge, wie bespielsweise ARIS oder das später gezeigte i>PM2 :cite:`roth_konzeption_2011` werden, nutzen bisher ausschließlich 2D-Darstellungen. 
Das Bedienkonzept dieser Anwendungen, folgt den Standards der seit drei bis vier Jahrzehnten üblichen Desktopprogramme, welche mit Tastatur und Maus bedient werden und eine zweidimensionale grafische Benutzeroberfläche anbieten (auch "WIMP" genannt[#f1]_).

Der Einsatz der dritten Dimension für die Repräsentation von Prozessen wurde jedoch schon vereinzelt untersucht, beispielsweise wird von :cite:`betz` gezeigt, wie sich dies für die Visualisierung von Beziehungen zwischen mehreren Modellen oder zur Darstellung von hierarchischen Modellen sinnvoll nutzen lässt. 
So könnten sich Vorteile zu einer 2D-Darstellung ergeben, welche beispielsweise weniger Möglichkeiten bietet, verschiedene Arten von Beziehungen zwischen Modellelementen in leicht verständlicher Form zu visualisieren :cite:`gil_three_1998`.
Arbeiten auf dem Gebiet der Softwaremodellierung, welche in dieser Arbeit vorgestellt werden, zeigen weitere Nutzungsmöglichkeiten, die sich möglicherweise auf die Prozessmodellierung übertragen lassen. 

Prozessmodellen enthalten oft auch Konzepte, die Entitäten aus der realen Welt vertreten, beispielsweise eine eingesetzte Maschine oder ausführende Personen. 
In machen Fällen könnte es sinnvoll sein, diese Objekte in ihrem realen Erscheinungsbild neben dem Prozessmodell darzustellen, um beispielsweise abstrakte Konzepte für Benutzer anschaulicher zu machen, wie von :cite:`brown_conceptual_2010` dargestellt wird.

Ein ProzessmodellierungsWerkzeug, das die Möglichkeiten der modernen 3D-Computergrafik ausnutzt oder gar neuartige (3D-)Eingabegeräte unterstützt, existiert bisher nicht :cite:`brown_conceptual_2010`.
Um die Effektivität von 3D-Visualisierung für die Prozessmodellierung zu beurteilen und verschiedene Darstellungsformen zu vergleichen wäre allerdings ein solches System sehr hilfreich.

Zielsetzung dieser Arbeit
=========================

Da es kaum Möglichkeiten gibt, die Effektivität von 3D-Prozessvisualisierungen, besonders in interaktiven Anwendungen, zu evaluieren wurde mit dem i>PM 3D-Projekt das Ziel verfolgt, einen Prototypen für ein 3D-Prozessmodellierungswerkzeug zu schaffen, welches auch neuartige Eingabegeräte nutzt und die Anbindung von weiteren Eingabemöglichkeiten einfach macht.



Repräsentation bezieht sich hier sowohl auf die Visualisierung an sich, als auch auf die interne Darstellung der Prozessmodelle und die physische Speicherung auf Datenträgern. Da die dauerhafte Ablage eines Prozesses in Textform und das Laden solcher Darstellungen für eine Evaluation von einem Prozessmodellierungswerkzeugs unerlässlich ist, stellt dies auch einen Beitrag dieser Arbeit dar.

Die genannten Punkte wurden in einer prototypischen Implementierung umgesetzt und ersten Versuchen unterzogen.

Aufbau
======

Zu Beginn werdeb die grundlegenden Begriffe aus dem Bereich der Prozessmodellierung, der Modellierungssprachen und der Metamodellierung vorgestellt. Danach werden verschiedene für diese Arbeit relevante Arbeiten aus dem Bereich der 3D-Visualisierung und Vorarbeiten zum Thema 3D-Prozessvisualisierung bzw -modellierung und verwandten Gebieten besprochen. 
Darauf aufbauend wird ein Konzept zur dreidimensionalen Visualisierung von Prozessmodellen sowie zur Anbindung der Visualisierungskomponente an das Gesamtsystem, das mit Hilfe von Simulator X realisiert wurde entwickelt. Danach werden einige Details aus der Implementierung des Prototypen dargestellt und ein kleines Anwendungsbeispiel gegeben. 
Die Arbeit schließt mit einem Ausblick auf die zahlreichen Erweiterungsmöglichkeiten und zukünftigen Forschungs- und Entwicklungsthemen, die sich aus diesen Arbeit und dem Projekt ergeben.


.. [#f1] WIMP steht für "windows, icons, menus, pointer
