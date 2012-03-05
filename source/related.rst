************
Related Work
************

3D-Softwarevisualisierung
=========================

Nahe verwandt mit der Prozessmodellierung ist die Modellierung von Software beziehungsweise visuelle Programmiersprachen. In diesem Bereich findet sich eine Reihe von Arbeiten, die sich mit der 3D-Visualisierung von Software und der Übertragung von typischen grafischen, zweidimensionalen Notationen wie den state machine diagrams der UML in den 3D-Raum beschäftigen.

Einen Überblick über verschiedene 3D-Software-Visualisierungsarbeiten gibt :cite:`teyseyre_overview_2009`. 

:cite:`dwyer_three_2001` weist auf die Probleme von 2D-Darstellungen hin, große und insbesondere hierarchisch aufgebaute Diagramme darzustellen. 3D-Darstellungen hätten hier Vorteile durch die Möglichkeiten, Ebenen des Diagramms als Flächen im 3D-Raum zu zeigen. Vorgeschlagen wird eine Anordnung von UML-Elementen im dreidimensionalen Raum mit Hilfe eines automatischen, kräftebasierten Layout-Algorithmus.

In :cite:`mcintosh_x3d-uml:_2008` werden state-machine-Diagramme der UML direkt in den 3D-Raum übertragen. 
Die Diagramme werden hierbei immer noch zweidimensional gezeichnet, jedoch auf Ebenen im 3D-Raum platziert. So lassen sich Beziehungen zwischen mehreren Diagrammen gut grafisch darstellen, jedoch leider die Lesbarkeit je nach Entfernung und Betrachtungswinkel doch recht stark. Die Möglichkeiten von 3D-Visualisierungen werden hier kaum genutzt.


Graphical Editing Framework 3D
==============================

Eines der wenigen frei verfügbaren Werkzeuge, mit denen abstrakte 3D-Modelle erstellt werden können ist GEF3D :cite:`von_pilgrim_gef3d:_2008`. Das Projekt basiert auf den Konzepten von GEF, dem Grafical Editing Framework der Eclipse Plattform und uberträgt diese in den dreidimensionalen Raum.



Zur Implementierung ist zu sagen, dass das Projekt in den letzten beiden Jahren relativ wenig Änderungen und Verbesserungen erfahren hat und das Rendering noch auf "altem" OpenGL basiert und damit die Möglichkeiten moderner Grafikhardware eher unzureichend nutzt.


Conceptual Modelling in 3D Virtual Worlds for Process Communication
===================================================================

In dieser Arbeit wird ein Prototyp eines 3D BPMN-Editors vorgestellt, der in der virtuellen Welt von Second Life implementiert wurde. Besonderen Wert wurde auf die Zusammenarbeit zwischen mehreren Modellierern und die Prozesskommunikation, auch unter Beteiligung von Personen, die keine Modellierungsexperten sind, gelegt. 
"Naive stakeholders" hätten oft Probleme, die abstrakte Welt der konzeptuellen Modellierung zu verstehen, weil der Bezug zu realen Gegenständen fehle. Durch Zuhilfennahme einer virtuellen Welt, in der abstrakte Prozessmodelle eingebettet sind solle dies abgemildert werden. 

In dieser Umgebung können Abbilder von realen Entitäten, die mit dem Prozess in Beziehung stehen und / oder mit diesem interagieren - beispielsweise verwendete Betriebsmittel oder ausführende Personen - dargestellt werden. Dies könne auch dazu dienen, den Ort und die räumliche Anordnung von Prozessschritten, beispielsweise durch ein Einbettung in einem virtuellen Gebäude, zu visualisieren. Möglich sei auch eine Simulation der Prozessausführung in der virtuellen Welt.
(siehe folgendes Beispiel).

Dadurch solle es den Beteiligten leichter möglich sein, festzustellen, ob das Modell die Realität richtig abbildet und ob eventuell Probleme bei der Umsetzung des Prozesses in der Realität auftreten könnten.

Die Prozesse selbst werden mit Hilfe eines 3D-Graphen dargestellt. Als Knoten werden in den 3D-Raum übertragene BPMN-Modellelemente genutzt während Kanten durch einfache Linien (mit Pfeilspitzen auf der Zielseite bei gerichtete Kanten) dargestellt werden. Auf den Knoten können wie in der BPMN üblich Informationen durch Texte oder statische Grafiken vermittelt werden. 

Die Objekte selbst sind 3D-Körper, jedoch scheinen die Informationen nur auf einer Seite dargestellt zu sein. Damit ergeben sich Probleme bei Rotationen der Modellelemente und Bewegungen um den Prozessgraphen herum. Je nach Perspektive ist es möglich, dass die Texte bzw. die Symbole nicht mehr sichtbar sind.

Auf den Objekten sind rote Kugeln als Anker für Verbindungen zu anderen Modellobjekten angeordnet.

Die Benutzer selbst werden als Avatar [wassn das?] gezeigt, welche die Interaktion des Benutzers mit dem Modell für andere Teilnehmer verdeutlichen.


Es gibt es die Möglichkeit, "Kommentarwände" einzurichten, auf denen beliebige Texte zur Kommunikation zwischen den Beteiligten dargestellt werden können. Daneben können auch andere Multimediainhalte wie Videos, Tonaufnahmen oder Statistiken zur Prozessausführung (über Web-Services) eingebettet werden.

Hier sind Screenshots aus einem Video zu sehen, das mit Hilfe des beschriebenen Systems erstellt wurde. Es zeigt einen Prozess an einem Flughafen. 


Im Bereich des Process- / Event-Mining anzusiedeln ist die Arbeit :cite:`suntinger_event_2008`, "The Event Tunnel". Hier wird eine 3D-Ansicht dafür genutzt, Ereignisse bei der Ausführung von Prozessen in hilfreicher Weise zu visualisieren. Die Ereignisse sind hierbei innerhalb eines 3D-"Tunnels" angeordnet, entlang dessen Achse die Zeitachse verläuft. So lassen sich Ereignisse leicht zeitlich einordnen und Zusammenhänge werden erkennbar.

3D Representation of Business Process Models
============================================

(:cite:`betz_3d_2008`)

Diese Arbeit stellt verschiedene Anwendungsmöglichkeiten für die 3D-Prozessvisualisierung vor.

Prozesse werden hier mit Petrinetzen modelliert.

Gezeigt werden Möglichkeiten, Zusammenhänge zwischen mehreren Modellen darzustellen. Dabei könnte es sich um Modelle verschiedenen Typs handeln, wie beispielsweise einem Prozess- und einem Organisationsmodell oder um Modelle gleichen Typs.

3D-Ansichten könnten dazu benutzt werden, Gemeinsamkeiten und Unterschiede zwischen verschiedenen Prozessversionen oder -varianten aufzuzeigen. Die Darstellung als 3D-Graph hilft hierbei, indem die Prozesse nebeneinander im Raum platziert und Verbindungen zwischen gleichen Modellelementen der gegenüberliegenden Modelle angezeigt werden können.

Außerdem könnten Verfeinerungen und Aggregierungen von Prozessteilen gut im dreidimensionalen Raum dargestellt werden, da man neben dem verfeinerndem Diagramm die grobgranulare Prozessansicht zeigen kann.

Das Konzept wurde in einem Prototypen implementiert.

Sonstiges
=========

[das eher ins Konzept-Kapitel unter Erweiterungen!]

Das ebenfalls für die Prozessmodellierung interessante Konzept der dynamischen Transparent von Modellobjekten, abhängig von deren Relevanz, wird von :cite:`elmqvist_dynamic_2009` vorgestellt. Es handelt sich hierbei um einen Lösungsansatz für das typische Problem der Verdeckung in der 3D-Visualisierung.

Die Grundidee ist hier, Objekte nach ihrer Wichtigkeit für die aktuelle Betrachtungssituation einzuteilen. Unwichtige, die Ansicht störende Objekte werden als "distractors", informationstragende Elemente als "targets" bezeichnet. Das Ziel ist nun, sicherzustellen, dass "targets" nie von "distractors" verdeckt werden können. Letztere werden, sobald sie wichtige Objekte verdecken transparent dargestellt, damit das relevante Element jederzeit erkannt werden kann. Dazu wird ein Algorithmus angegeben, der diesen Effekt in Echtzeit auf Sub-Objekt-Ebene berechnen kann.
