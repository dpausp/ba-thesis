.. _ipm3d:

**************************************
Einordnung in das Gesamtprojekt i>PM3D
**************************************

Diese Arbeit und die dazugehörige Implementierung sind im Rahmen des i>PM3D-Projekts entstanden. Das Ziel des Projekts ist es, einen Prototypen eines grafischen 3D-Prozessmodellierungswerkzeugs zu erstellen, der prinzipielle Vor-und Nachteile von 3D-Editoren zeigen und als Grundlage für weitere Arbeiten in dieser Richtung dienen soll. 
Das vorliegende Kapitel gibt eine Übersicht über das i>PM3D-Projekt\ [#f1]_.

.. _ipm3d-uebersicht:

Übersicht über die Projektbestandteile
======================================

In der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptionelle-uebersicht` wird der konzeptionelle Aufbau des Projektes veranschaulicht. 
Bestandteile, mit denen sich die vorliegende Arbeit befasst, sind darin farbig hervorgehoben (rechts).
In das i>PM 3D-Projekt sind ebenfalls die Bachelorarbeiten von Sebastian Buchholz :cite:`buchi` und Uli Holtmann :cite:`uli` einzuordnen, die die übrigen Bestandteile von i>PM3D beschreiben. 

.. _ipm3d-konzeptionelle-uebersicht:

.. figure:: _static/diags/ipm3d-uebersicht.eps
    :width: 15cm 

    Übersicht über die Bestandteile von i>PM3D

Im Folgenden werden die einzelnen Projektbestandteile kurz vorgestellt und miteinander in Beziehung gesetzt.


.. _ipm3d-visualisierung:

Visualisierung
--------------

Eine zentrale Fragestellung bei der Realisierung eines grafischen Prozessmodellierungswerkzeugs ist es, auf welche Weise Prozessmodelle visualisiert werden sollen.

Elemente aus dem Prozessmodell sollen in einer für den Benutzer leicht verständlichen Art und Weise angezeigt werden, die die Möglichkeiten des dreidimensionalen Raums nutzt. 
Die Darstellung soll dabei an die aus 2D-Prozessmodellierungswerkzeugen bekannten grafischen Notationen angelehnt sein. 
Prozessmodelle in i>PM3D werden in einer graphbasierten Form, also durch Knoten und damit verbundenen Kanten dargestellt. 
Zusätzlich zu den eigentlichen Prozessmodellelementen gibt es die Möglichkeit, beliebige 3D-Objekte anzuzeigen. Dies kann beispielsweise dafür genutzt werden, um abstrakte Konzepte mit Abbildern von realen Objekten zu illustrieren.

Wie in der Computergrafik üblich wird das Prinzip einer virtuellen Kamera benutzt, durch die der Benutzer die Szene beobachtet ("Egoperspektive").
Durch Verschieben und Rotieren der Kamera kann sich der Benutzer in der virtuellen Umgebung "bewegen" und die dargestellten Prozessdiagramme aus verschiedenen Perspektiven betrachten. 
Knoten und Szenenobjekte sind frei drehbar um alle drei Achsen und unter Beibehaltung der Seitenverhältnisse skalierbar. Sie können prinzipiell frei in der 3D-Szene platziert werden.
Die Visualisierung von Modellen in i>PM3D wird in der vorliegenden Arbeit näher :ref:`vorgestellt<visualisierung>`.

Modellanbindung
---------------

Ein Modellierungswerkzeug muss die Fähigkeit haben, bestehende Modelle aus einer physischen Repräsentation zu laden, das Modell beziehungsweise dessen Elemente zu bearbeiten und wieder zu speichern. 
Außerdem sollen neue Modelle erstellt werden können. 
In i>PM 3D kann die grafische Modellierungssprache (in einem gewissen Rahmen) ohne Änderungen am Programmcode modifiziert werden, da die abstrakten Modellelemente und deren (visuelle) Repräsentation jeweils durch ein zur Laufzeit geladenes Metamodell beschrieben werden. 
Die in einem Prozessmodell verwendeten abstrakten Modellelemente (bspw. ein "Prozessknoten") sowie deren aktuelle Repräsentation im Modelleditor (bspw. das "Aussehen" und die Position eines Prozessknotens) werden in separaten Modellen abgelegt.
In der :num:`Abbildung #ipm3d-konzeptionelle-uebersicht` werden diese Modelle als "Prozess-Modell" bzw. als "Editor-Modell" bezeichnet.

Der grundsätzliche Aufbau und die Anpassbarkeit der Modell-Hierarchie wird in :ref:`dieser Arbeit<modellhierarchie>` besprochen. 
Außerdem werden die verwendeten Metamodelle im Detail :ref:`beschrieben<metamodelle>` und ein Beispiel gezeigt, wie sich neue Modellelemente ergänzen lassen.

Es ist ebenfalls Gegenstand dieser Arbeit, die Einbindung der Modell-Funktionen in den Prototypen zu realisieren.
In der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptionelle-uebersicht` wird dieser Teil des Projekts als :ref:`"Modellanbindung"<modellanbindung>` bezeichnet.

.. _ipm3d-gui:

GUI
---

Dem Benutzer wird das 3D-Prozessdiagramm in einer interaktiven Umgebung präsentiert, die das Erstellen, Bearbeiten und Löschen von Elementen erlaubt.

Die verschiedenen Funktionen des Prozessmodellierungswerkzeugs wie das Erstellen von Modellelementen und das Laden von Modellen lassen sich durch grafische Menüs aktivieren, die über der 3D-Szene gezeichnet werden und die an das Bedienkonzept verbreiteter 2D-Anwendungen mit grafischer Oberfläche angelehnt sind. 
Für das Erstellen von neuen Knoten und Szenenobjekten wird ein Menü – auch als "Palette" bezeichnet – bereitgestellt, über welches die zur Verfügung stehenden Objekte durch einen Klick auf eine Schaltfläche erzeugt werden können.
Attribute der Modellelemente, die entweder die Visualisierung selbst oder das damit verbundene Element des Prozessmodells betreffen, können in einem in einem Menü angezeigt und bearbeitet werden.
Die Menüs werden in der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptionelle-uebersicht` als GUI zusammengefasst.

Eingabeaufbereitung und Editor
------------------------------

Eine wichtige Anforderung an den Prototypen ist, dass verschiedene Arten von Eingabegeräten unterstützt, neue Geräte einfach angebunden und – soweit sinnvoll – nebeneinander benutzt werden können. 
Die von den Eingabegeräten gelieferten Daten unterscheiden sich je nach Art des Geräts und der verwendeten Schnittstelle deutlich voneinander.
Daher ist es sinnvoll, von den Eingabegeräten und deren Schnittstellen zu abstrahieren. Dies wird erreicht, indem die Eingabedaten aller Geräte von einer Eingabeschicht aufbereitet und an eine vereinheitlichte Schnittstelle zur Bedienung der Anwendung weitergeleitet werden. Diese Schnittstelle zur Eingabeverarbeitung wird, zusammen mit dem GUI, in der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptionelle-uebersicht` als *Editor* bezeichnet.

Mit der Realisierung des *Editors* sowie mit der Aufbereitung der Daten, die von Tastatur und Maus geliefert werden befasst sich :cite:`uli`.

Neuartige Eingabegeräte
-----------------------

Neben den für Arbeitsplatzrechner üblichen Eingabegeräten Tastatur und Maus, soll der Editor auch mittels "neuartiger" Eingabegeräte bedienbar sein, die sich besonders für die Interaktion mit virtuellen 3D-Umgebungen eignen könnten.
Dabei sind besonders solche Geräte interessant, die auch an einem handelsüblichen, aktuellen Desktop-PC angeschlossen werden können und relativ "preiswert" sind. 
Die Bereitstellung von neuartigen Eingabegeräten und die Aufbereitung der Eingabedaten werden von der Arbeit :cite:`buchi` abgedeckt, welche sich speziell mit der Anbindung der Microsoft Kinect und der Nintendo WiiMote befasst. Neben der direkten Nutzung dieser Geräte als "Mausersatz" [#f2]_ werden auch mit den Geräten ausgeführte Gesten und ein spezielles Kinect-Menü als Eingabemethode untersucht und für das Projekt nutzbar gemacht.

Diese Beiträge sind in der Übersichtsgrafik :num:`Abbildung #ipm3d-konzeptionelle-uebersicht` unter "Eingabegeräte" und "Eingabeaufbereitung" zu finden. 


i>PM3D als Simulator X - Applikation
====================================

i>PM3D ist als Anwendung auf Basis von :ref:`simulatorx` konzipiert. 
:num:`Abbildung #ipm3d-simulatorx` zeigt, wie die Architektur des Projekts auf den von Simulator X bereitgestellten Funktionalitäten aufbaut. 
In den beiden folgenden Abschnitten wird zusammengefasst, welche Änderungen am Simulator-X-Basissystem vorgenommen worden sind und wie die im letzten Abschnitt dargestellten Projektteile im Kontext von *Simulator X* umgesetzt werden.

.. _ipm3d-simulatorx:

.. figure:: _static/diags/ipm3d-simulatorx.png

   Architektur von i>PM3D, aufbauend auf Simulator X

.. _mod-simx:

Modifikationen an Simulator X
-----------------------------

Für i>PM3D wurde die von :ref:`simulatorx` bereitgestellte Physik-Komponente für spezielle Aufgaben erweitert. Die Physikengine wird für die Selektion von Modellobjekten, für die Realisierung von "Gravitationsebenen", und die Erkennung von Kollisionen zwischen Modellobjekten eingesetzt. Den Einsatz Physikkomponente und die projektspezifischen Modifikationen beschreibt :cite:`buchi`.

Die ebenfalls mitgelieferte Renderkomponente, die für die grafische Ausgabe auf Basis von OpenGL zuständig ist, war für das Projekt allerdings nicht sinnvoll nutzbar. 
Daher wurde diese durch eine Anbindung an die im Rahmen dieser Arbeit entwickelte, flexible :ref:`render-bibliothek` ersetzt, welche die einfache Erstellung von neuen Modell-Figuren ermöglicht und die Möglichkeiten moderner OpenGL-Grafikprogrammierung nutzt.  
Die Anbindung an *Simulator X* wird durch die in :num:`Abbildung #ipm3d-simulatorx` gezeigte :ref:`renderkomponente` geleistet.

Modellkomponente und Modell-Entitäten
--------------------------------------

Die im vorherigen Abschnitt als *Modellanbindung* bezeichneten Funktionalitäten werden im Simulator X - Kontext durch die **Modellkomponente** realisiert, die dem Editor eine Schnittstelle zur Verfügung stellt über welche die genannten Aktionen ausgelöst werden können.
Die Modellelemente selbst zu bearbeiten, also deren Visualisierungsparameter und Prozessmodellattribute sowie die Position, Größe und Orientierung im Raum zu ändern, wird durch die von der Modellkomponente bereitgestellten **Modell-Entitäten** ermöglicht, welche durch den Editor manipuliert werden.

Dem Simulator X - Konzept folgend, beschreiben diese *Entities* außerdem, wie die dazugehörigen Objekte von der Physikkomponente behandelt und wie sie von der Renderkomponente angezeigt werden.
Näheres zur Modellkomponente und den Modell-Entitäten ist im Kapitel zur :ref:`modellanbindung` zu finden.

Editor-Komponente und Eingabekonnektoren
----------------------------------------

Die Bedienschnittstelle, in Abbildung 3.2 als Editor beschrieben, ist eine Simulator X Komponente. 
Wird eine Modell-Entity fertiggestellt, wird sie dem Editor übergeben, so dass dieser sie in seine eigenen Datenstrukturen einbringen und für den Benutzer ansprechbar machen kann.
Die Konnektoren der Eingabeaufbereitungsschicht setzen auf keiner Simulator X Komponente auf, benutzen dafür aber das Simulator X Event-System um die aufbereiteten Eingabedaten an die Bedienschnittstelle zu senden.
Eine ausführlichere Beschreibung der Editor-Komponente, Eingabekonnektoren sowie der Kommunikation untereinander und mit anderen Komponenten ist bei :cite:`uli` zu finden.


.. [#f1] Dieses Kapitel ist in Zusammenarbeit mit :cite:`buchi` und :cite:`uli` entstanden und in jenen Arbeiten in ähnlicher Form zu finden.

.. [#f2] Dies bedeutet in diesem Zusammenhang, dass die Geräte einen Cursor ("Mauszeiger") steuern, der die aktuelle Position in einer zweidimensionalen Ebene anzeigt. Bei einem "Klick" wird eine Aktion auf dem darunter befindlichen Objekt ausgelöst.


