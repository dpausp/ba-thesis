**************************************
Einordnung in das Gesamtprojekt I>PM3D
**************************************

Diese Arbeit und die dazugehörige Implementierung sind im Rahmen des I>PM3D-Projekts entstanden. Das Ziel des Projektes ist es, einen Prototypen eines 3D-Prozessmodellierungswerkzeugs zu erstellen, der prinzipielle Vor-und Nachteile von 3D-Editoren zeigen und als Grundlage für weitere Arbeiten in dieser Richtung dienen soll. I>PM3D ist als Anwendung auf Basis von :ref:`simulator_x` konzipiert.

Eine zentrale Aufgabe des Prototypen ist die grafische Darstellung von Prozessmodellen. Elemente aus dem Prozessmodell sollen durch in einer für den Benutzer leicht verständlichen Art und Weise visualisiert werden, die die Möglichkeiten des dreidimensionalen Raums nutzt. Die Darstellung soll dabei an die aus 2D-Prozessmodellierungswerkzeugen bekannten grafischen Notationen angelehnt sein. 

Prozessmodelle in I>PM3D werden in einer graphbasierten Form, also mittels Knoten und damit verbundenen Kanten dargestellt. Zusätzlich zu den eigentlichen Prozessmodellelementen soll es noch eine Möglichkeit geben, beliebige 3D-Objekte anzuzeigen. Dies kann beispielsweise dafür genutzt werden, um abstrakte Konzepte mit Abbildern von realen Objekten zu illustrieren.

Dem Benutzer wird das 3D-Diagramm in einer interaktiven Umgebung präsentiert, die das Erstellen von neuen Elementen sowie das Bearbeiten und Löschen schon vorhandener Elemente erlaubt.

Wie in der Computergrafik üblich wird das Prinzip einer virtuellen Kamera benutzt, durch die der Benutzer die Szene beobachtet, oft "Egoperspektive" genannt. 
Durch Verschieben und Rotieren der Kamera kann sich der Benutzer in der virtuellen Umgebung "bewegen" und die dargestellten Prozessdiagramme aus verschiedenen Perspektiven betrachten. 

Die Knoten und Szenenobjekte sind frei drehbar um alle drei Achsen und können prinzipiell frei in der 3D-Szene platziert werden.

Das allgemeine Konzept zur Visualisierung der Prozessdiagramme, sowie dessen Implementierung auf Basis einer selbst erstellten OpenGL-Render-Bibliothek wird in der vorliegenden Arbeit im Kapitel :ref:`konzept-visualisierung` beziehungsweise :ref:`implementierung` vorgestellt.

Ein Prozesseditor muss außerdem die Möglichkeit haben, bestehende Modelle in einer physischen Repräsentation — hier als textuelle Darstellung in einer Datei – zu laden, das Modell beziehungsweise dessen Elemente zu bearbeiten und wieder zu speichern. Außerdem sollen neue Modelle erstellt werden können. 

Das Konzept für die Einbindung dieser Modell-Funktionen in den Prototypen und die Implementierung zu erstellen ist ebenfalls Gegenstand dieser Arbeit und wird in den Kapiteln :ref:`konzept-modellanbindung` beziehungsweise :ref:`implementierung` behandelt. In der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptuelle-uebersicht` wird dieser Abschnitt als "Modellanbindung" bezeichnet.

Die verschiedenen Funktionen des Editors wie das Erstellen von Modellelementen und das Laden von Modellen sollen sich über Menüs aktivieren lassen, die über der 3D-Szene gezeichnet werden und die an das Bedienkonzept verbreiteter 2D-Anwendungen mit grafischer Oberfläche angelehnt sind. 
Dabei sollte aber auch berücksichtigt werden, dass sich die Menüelemente auch mit Geräten wie der Kinect bedienen lassen, die etwas ungenauere Positionsangaben als eine übliche PC-Maus liefern.

Für das Erstellen von neuen Knoten und Szenenobjekten soll ein Menü, im Folgenden "Palette" genannt, bereitgestellt werden, in dem die zur Verfügung stehenden Objekte durch einen Klick auf eine Schaltfläche erzeugt werden können.

Attribute der Modellelemente, die entweder die Visualisierung selbst oder das damit verbundene Element aus dem Prozessmodell betreffen sollen über ein Menü angezeigt und bearbeitet werden könnnen.

Eine wichtige Anforderung an den Protoypen ist, dass verschiedene Arten von Eingabegeräten unterstützt, neue Geräte einfach angebunden und – soweit sinnvoll – nebeneinander benutzt werden können. 
Die Anbindung der Eingabegeräte wird in der Bachelorarbeit von Uli Holtmann :cite:`uli`, die ebenfalls im Rahmen von I>PM3D entstanden ist, behandelt. 
Die Arbeit beschäftigt sich außerdem mit der Realisierung der vorher genannten Menüs, die von den Eingabegeräten bedient werden.

In der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptuelle-uebersicht` werden diese beiden Systembestandteile als "Eingabeschicht" beziehungsweise "Menüs" bezeichnet.

Neben den für Desktoprechner üblichen Eingabegeräten Tastatur und Maus sollen auch neuartige Eingabegeräte untersucht und für das Projekt nutzbar gemacht werden, die sich besonders für die Bedienung von 3D-Anwendungen eignen könnten.
Dabei sollte es sich um Geräte handeln die auch an einem handelsüblichen, aktuellen Arbeitsplatzrechner angeschlossen werden können und relativ "preiswert" sind. 
Dieses Abschnitt des Projektes wird von der durch Sebastian Buchholz angefertigten Bachelorarbeit :cite:`buchi` abgedeckt, welche sich speziell mit der Anbindung der Microsoft Kinect und der Nintendo WiiMote befasst. Neben der direkten Nutzung dieser Geräte als "Mausersatz" [#f1]_ werden auch mit den Geräten ausgeführte Gesten und ein spezielles Menü für die Kinect als Eingabemethode untersucht.

Diese Beiträge werden in der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptuelle-uebersicht` unter "Eingabegeräte" zusammengefasst. 

Für I>PM3D wurde die von :ref:`Simulator X` bereitgestellte Anbindung an eine Physikengine für spezielle Aufgaben erweitert, was ebenfalls von :cite:`buchi` beschrieben wird. Die Physikengine wird für die Selektion von Modellobjekten, für die Realisierung von "Gravitationsebenen", und die Erkennung von Kollisionen zwischen Modellobjekten eingesetzt.

.. _[#f1]: Dies bedeutet in diesem Zusammenhang, dass die Geräte einen Cursor ("Mauszeiger") steuern, der die aktuelle Position in einer zweidimensionalen Ebene anzeigt. Bei einem "Klick" wird eine Aktion auf dem darunter befindlichen Objekt ausgelöst.
