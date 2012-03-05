**********
Einleitung
**********

Prozessmanagement ist heute in vielen Unternehmen und anderen Einrichtungen im Einsatz um Geschäftsprozesse zu planen, zu dokumentieren und Möglichkeiten zu finden, um Abläufe zu optimieren. Visualisierungstechniken spielen hierbei eine große Rolle, da Prozesse nicht nur von Rechnern, sondern auch von Menschen verstanden werden sollen. Prozessmodelle werden zur Kommunikation von Sachverhalten eingesetzt, wobei die Beteiligten oft keine ausgebildeten Experten für Prozessmodellierung oder Programmierer sind. Das macht eine besonders intuitive und verständliche Art der Darstellung nötig. 

Für diesen Einsatzzweck gibt es eine Reihe von Spezifikationen wie beispielsweise BPMN oder EPK, die Prozesse in einer recht abstrakten Form mit Hilfe von einfachen geometrischen Figuren in 2D-Diagrammen darstellen. Für Nicht-Experten kann diese abstrakte, weit von realen Objekten entfernte Darstellung ein Problem sein. Mehr gegenständliche Präsentation könnte hier helfen.

Implementiert werden die genannten Modellierungssprachen durch verschiedene Programme, die den Standards der seit drei bis vier Jahrzehnten üblichen Desktopprogramme folgen und mit Maus und Tastatur bedient und mit üblichen GUI-Elementen dargestellt werden. WIMP stands for "windows, icons, menus, pointer.

Prozessmodelle müssen, wenn sie effektiv sein sollen von Details abstrahieren und sich auf das Wesentliche konzentrieren. Trotzdem enthalten reale Modelle eine recht große Anzahl an Elementen, was schnell den verfügbaren Platz auf dem Bildschirm belegt und beispielsweise Zoom oder seitliches Scrollen notwenig macht. Zweidimensionale Darstellungen sind hier in ihren Möglichkeiten eingeschränkt, vor allem wenn es darum geht, größere Zusammenhänge zu visualisieren und Kontext herzustellen; beispielsweise zwischen verschiedenen Modellen.

Davon getrennt gab es in der Computergrafik in den letzten (beiden) Jahrzehnten eine bemerkenswerte Entwicklung der Möglichkeiten von Hardware und Software in Sachen dreidimensionaler Darstellung. 
3D-Grafikanwendungen, die früher nur mit sehr großen Aufwand und finanziellem Einsatz realisierbar waren, sind heute selbst in Echtzeit für handelsübliche PCs verfügbar. Auffällig und für jeden sichtbar ist dieser Fortschritt vor allem in der Computerspielebranche. 

Neben den klassischen Desktop-Anwendungen gibt es mittlerweile auch Techniken, um in die virtuelle Welt vollständig einzutauchen und auf natürliche Weise mit nicht-realen Objekten zu interagieren. Diese immersiven 3D-Anwendungen, oft als "CAVE" bezeichnet werden neben Spielanwendungen auch für die gegenständliche Modellierung in der Industrie, zum Beispiel zum virtuellen Konstruieren eingesetzt ("virtuelle Werkstatt").

Wäre so etwas auch für die abstrakte, konzeptuelle Modellierung, zum Beispiel für die Prozessmodellierung sinnvoll?

Bisher wurden die Fortschritte in der 3D-Computervisualisierung für die Prozessmodellierung und ähnliche Gebiete noch wenig beachtet. Programme, die alle Möglichkeiten virtueller 3D-Welten für die Prozessmodellierung nutzbar machen fehlen bislang komplett. 

Hierfür gibt es sicher eine Reihe von Gründen, die von fehlenden technischen Möglichkeiten (sowohl bei Hardware als auch bei Softwareentwicklungstechniken) bis hin zu Problemen bei der Vermittlung an Benutzer gehen. 3D-Anwendungen werden hin und wieder als Spielzeug gesehen, die für ernsthafte Arbeiten nicht zu gebrauchen sind, was sicher auch von technischen Unzulänglichkeiten früherer 3D-Systeme und der heutigen Verbreitung von Computerspielen herrührt.


Zielsetzung dieser Arbeit
=========================

Da es kaum Möglichkeiten gibt, die Effektivität von 3D-Prozessvisualisierungen, besonders in interaktiven Anwendungen, zu evaluieren soll diese Arbeit einen Beitrag leisten, indem ein Konzept entwickelt wird, wie Prozesse im dreidimensionalen Raum repräsentiert werden können. Außerdem ist das Ziel, die Integration in ein im Aufbau befindliches 3D-Simulatorsystem, Simulator X zu leisten um den Aufbau einer interaktiven Prozessvisualisierungs- und -modellierungsumgebung möglich zu machen.

Repräsentation bezieht sich hier sowohl auf die Visualisierung an sich, als auch auf die interne Darstellung der Prozessmodelle und die physische Speicherung auf Datenträgern. Da die dauerhafte Ablage eines Prozesses in Textform und das Laden solcher Darstellungen für eine Evaluation von einem Prozessmodellierungswerkzeugs unerlässlich ist, stellt dies auch einen Beitrag dieser Arbeit dar.

Die genannten Punkte wurden in einer prototypischen Implementierung umgesetzt und ersten Versuchen unterzogen.

Aufbau
======

Zu Beginn werdeb die grundlegenden Begriffe aus dem Bereich der Prozessmodellierung, der Modellierungssprachen und der Metamodellierung vorgestellt. Danach werden verschiedene für diese Arbeit relevante Arbeiten aus dem Bereich der 3D-Visualisierung und Vorarbeiten zum Thema 3D-Prozessvisualisierung bzw -modellierung und verwandten Gebieten besprochen. 
Darauf aufbauend wird ein Konzept zur dreidimensionalen Visualisierung von Prozessmodellen sowie zur Anbindung der Visualisierungskomponente an das Gesamtsystem, das mit Hilfe von Simulator X realisiert wurde entwickelt. Danach werden einige Details aus der Implementierung des Prototypen dargestellt und ein kleines Anwendungsbeispiel gegeben. 
Die Arbeit schließt mit einem Ausblick auf die zahlreichen Erweiterungsmöglichkeiten und zukünftigen Forschungs- und Entwicklungsthemen, die sich aus diesen Arbeit und dem Projekt ergeben.
