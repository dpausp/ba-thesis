.. _related:

****************************************
Verwandte Arbeiten zur 3D-Visualisierung
****************************************

Neben den (wenigen) Arbeiten, die sich explizit mit der dreidimensionalen Visualisierung und Modellierung von Prozessen beschäftigen, sollen hier auch solche vorgestellt werden, die sich allgemein oder in verwandten Gebieten wie der Softwaremodellierung mit 3D-Visualisierungen beschäftigen.

Die hier gezeigten Arbeiten sollen als Anregung oder Motivation für die in dieser Arbeit dargestellte 3D-Visualisierung von Prozessen dienen.
Außerdem sollen Ideen für zukünftige Erweiterungen des vorliegenden Projekts gesammelt und prinzipielle Vor- und Nachteile von 3D-Visualisierungen beleuchtet werden.

3D-Softwarevisualisierung
=========================

Nahe verwandt mit der Prozessmodellierung ist die Modellierung von Software. 
Nicht selten sind Softwaresysteme überaus umfangreich und es muss daher nach Möglichkeiten gesucht werden, eine Vielzahl von Informationen übersichtlich und klar zu visualisieren ohne den Betrachter zu überfordern. 

Bisher nutzen Werkzeuge zur Softwaremodellierung, die häufig auf der Unified Modeling Language (UML) aufsetzen nahezu ausschließlich 2D-Visualisierungen. 
Die UML lässt prinzipiell aber auch 3D-Repräsentationen zu :cite:`booch_unified_1999`.
Einen umfassenden Überblick über Arbeiten, die sich mit 3D-Softwarevisualisierung befassen, gibt :cite:`teyseyre_overview_2009`. 

.. _dywer:

Wilma: Ein 3D-Modellierungstool für UML-Diagramme
-------------------------------------------------

:cite:`dwyer_three_2001` weist auf die Probleme von Softwarevisualisierungstechniken hin, große und insbesondere hierarchisch aufgebaute Diagramme darzustellen. 
3D-Darstellungen hätten hier Vorteile durch die Möglichkeit, Hierarchieebenen des Diagramms als Flächen im 3D-Raum zu zeigen. 

Die Platzierung von UML-Elementen per Hand sei eine zeitraubende Aufgabe, die besonders im dreidimensionalen Raum wegen der schlechten Verfügbarkeit von 3D-Eingabegeräten zum Problem werde. 
Daher wird eine Anordnung der Diagrammelemente im 3D-Raum mit Hilfe eines automatischen, kräftebasierten Layout-Algorithmus vorgeschlagen.

Es wurde ein Prototyp ("Wilma")\ [#f1]_ realisiert, der Graphen in einer 3D-Darstellung zeichnen kann und Interaktionsmöglichkeiten mit den Graphelementen bietet.
Ein damit erstelltes 3D-Klassendiagramm ist in :num:`Abbildung #dywer-classdiag` zu sehen.

So werden Klassen durch 3D-Quader dargestellt, deren Seiten beschriftet werden können. 
Um die Lesbarkeit sicherzustellen, wird immer eine Seite des Würfels auf den Betrachter ausgerichtet.
Pakete werden durch transluzente Kugeln und Verbindungen zwischen Klassen durch gestreckte 3D-Zylinder dargestellt.


.. _dywer-classdiag:

.. figure:: _static/ext_pics/dywer_classdiag.png
    :height: 15cm

    3D-UML-Klassendiagramm aus :cite:`dwyer_three_2001`


In einer Studie zur Benutzbarkeit seien die 3D-Diagramme von Beteiligten als nützlich für das Verständnis des Modells eingestuft worden.
Es wird ein Benutzer zitiert, der die Möglichkeit, das Diagramm aus verschiedenen Richtungen betrachten zu können besonders positiv kommentiert.

Auch seien Benutzer gebeten worden, selbst ein 3D-Diagramm nach einer textuellen Vorlage zu modellieren, wobei die meisten wenig Probleme mit der Aufgabe gehabt hätten. 
Es wird jedoch vermutet, dass die 3D-Darstellung bei einigen Benutzern eine gewisse Eingewöhnungszeit voraussetzen könnte.
Probanden mit vorheriger Erfahrung aus 3D-Computerspielen hätten im Versuch die wenigsten Schwierigkeiten mit der Navigation im 3D-Raum gehabt. 

.. _mcintosh:

X3D-UML: Hierarchische 3D-UML-Zustandsdiagramme
-----------------------------------------------

In :cite:`mcintosh_x3d-uml:_2008` werden Zustandsdiagramme ("state diagrams") der UML in den 3D-Raum übertragen.

Zu Beginn seien von vier Unternehmen erhaltene Zustandsdiagramme untersucht worden, die mit dem Modellierungswerkzeug IBM Rational Rose erstellt wurden. 
Daraus habe sich ergeben, dass die Modelle oft hierarchisch aus Unterzuständen aufgebaut seien. 
In RationalRose würden diese Unterdiagramme jedoch in separaten Tabs dargestellt, was dazu führe, dass Betrachter ständig zwischen einzelnen Diagrammen hin- und herwechseln müssten.
Das erschwere das Erkennen von Zusammenhängen und groben Strukturen, was von befragten Benutzern bemängelt worden sei.
Die Einschränkungen durch die Tab-Ansicht würden auf verschiedenem Wege "umgangen", etwa indem separate Handskizzen angefertigt würden. 
Andere Benutzer würden "in die Luft starren", um sich die Zusammenhänge und Auswirkungen von Änderungen besser vorstellen zu können.

Daher sei es die wichtigste Anforderung an eine 3D-Repräsentation, hier Abhilfe zu schaffen und hierarchische Zustandsdiagramme besser abzubilden.
Es wird eine Darstellung vorgeschlagen, welche die Zustandsdiagramme selbst immer noch zweidimensional zeichnet, diese jedoch auf ebenen Flächen im 3D-Raum platziert. 
So würden sich Beziehungen zwischen mehreren Diagrammen gut grafisch darstellen lassen. 
Wie sich in :num:`Abbildung #mcintosh-sm` erkennen lässt, werden Beziehungen zwischen Super- und Subzuständen durch transluzente, graues Dreiecke dargestellt.

Solche Diagramme seien Benutzern mit Erfahrung in Rational Rose vorgelegt worden, welche sich insgesamt positiv zur Nützlichkeit jener 3D-Diagramme geäußert hätten. 
Von den Benutzern seien verschiedene Erweiterungen vorgeschlagen worden; unter anderem eine Filtermöglichkeit, mit der sich uninteressante Details verbergen lassen, Einschränkungen der Navigation um ungünstige Sichten auf das Modell zu vermeiden (bspw. direkt von der Seite, so dass die 2D-Elemente nicht erkennbar sind) sowie Funktionen, um schnell zwischen verschiedenen Betrachtungsperspektiven wechseln zu können. 

.. _mcintosh-sm:

.. figure:: _static/ext_pics/mcintosh_sm.png
    :width: 16.5cm

    Hierarchisch aufgebautes 3D-UML-Zustandsdiagramm aus :cite:`mcintosh_x3d-uml:_2008`


.. raw:: latex

    \pagebreak


.. _krolovitsch:

3D-Visualisierung von großen, hierarchischen UML-Zustandsdiagrammen
-------------------------------------------------------------------

3D-Visualisierungen von (großen) UML-Zustandsdiagrammen werden auch von :cite:`krolovitsch_3d_2009` und, darauf aufbauend, :cite:`alvergren_3d_2009` untersucht. 
Zustandsdiagramme werden, wie in :cite:`mcintosh_x3d-uml:_2008` auf Flächen im 3D-Raum gezeichnet, wobei hier die Zustände selbst als 3D-Objekte dargestellt sind, um den visuellen Eindruck zu verbessern, wie in :num:`Abbildung #krolovitsch-sm` zu sehen ist. 

:num:`Abbildung #krolovitsch-sm-nodes` zeigt, wie in komplexen Diagrammen komplette Diagrammteile ausgeblendet und durch einen blauen Würfel ersetzt werden können, um momentan unwichtige Details zu verbergen und die Übersichtlichkeit zu erhöhen. 

.. _krolovitsch-sm:

.. figure:: _static/ext_pics/krolovitsch_sm.png
    :width: 15.5cm

    3D-Zustandsdiagramm aus :cite:`krolovitsch_3d_2009`


.. _krolovitsch-sm-nodes:

.. figure:: _static/ext_pics/krolovitsch_sm_nodes.png
    :width: 15.5cm

    Zustandsdiagramm mit ausgeblendeten Diagrammteilen (dargestellt durch blaue Würfel) aus :cite:`krolovitsch_3d_2009`

.. _gil:

Dreidimensionale Darstellung zur besseren Visualisierung von Beziehungen
------------------------------------------------------------------------

:cite:`gil_three_1998` merkt an, dass durch 3D-Visualisierungen die Ausdrucksstärke von (graphbasierten) grafischen Notationen deutlich erhöht werden könne. 
Besonders vorteilhaft seien 3D-Visualisierungen von Graphen, wenn es darum ginge, eine Vielzahl von unterschiedlichen Beziehungs- bzw. Verbindungstypen darzustellen. 
Im 2D-Bereich habe man nur relativ eingeschränkte Möglichkeiten, unterschiedliche Verbindungstypen durch Farbe, unterschiedliche Linentypen oder durch Konnektoren – Symbole an den Enden der Linien – voneinander abzugrenzen. 
Um diese Probleme im 2D-Raum zu umgehen würden oft unterschiedliche Graphen bzw. Diagrammtypen genutzt. 
Dabei besäßen Knoten in unterschiedlichen Diagrammtypen oft die gleiche Bedeutung während Verbindungen eine komplett andere Semantik hätten. 
Problematisch sei die Repräsentation von Zusammenhängen zwischen unterschiedlichen Diagrammtypen, was allgemein einen großen Schwachpunkt von Modellierungssprachen darstelle.

Zur Visualisierung von Verbindungen lasse sich die dritte Dimension, also die z-Richtung sinnvoll nutzen. 
Verbindungen in der x-y-Ebene hätten eine andere Bedeutung als die, die aus der Ebene heraus in z-Richtung verlaufen. So würden sich mehrere Diagrammtypen in eine Darstellung integrieren lassen.

Die dritte Dimension ließe sich auch als Zeitachse interpretieren. 
So sei es möglich, in 3D-Sequenzdiagrammen die Zustände des Systems zu bestimmten Zeitpunkten auf parallelen Flächen darzustellen, zu denen die Zeitachse senkrecht steht wie in :num:`Abbildung #gil-sequencediag` gezeigt wird.

.. _gil-sequencediag:

.. figure:: _static/ext_pics/gil_sequencediag.png
    :height: 7cm

    3D-UML-Sequenzdiagramm; Ausschnitt aus :cite:`gil_three_1998`


.. _gogolla:

Nutzung von Tiefeneindruck und Animation zur Visualisierung von UML-Diagrammen
------------------------------------------------------------------------------

In :cite:`gogolla_towards_1999` wird ebenfalls die 3D-Darstellung von UML-Diagrammen, speziell Klassen-, Objekt- und Sequenzdiagrammen untersucht. 
Die dritte Dimension könne beispielsweise dafür genutzt werden, als "uninteressant" eingestufte Elemente in den Hintergrund zu schieben und damit Elemente im Vordergrund besonders hervorzuheben.

In :num:`Abbildung #gogolla-classdiag-a` und :num:`Abbildung #gogolla-classdiag-b` wird das Prinzip am Beispiel eines Klassendiagramms verdeutlicht.
Bei letzerer Abbildung ist zu sehen, dass bei Klassen, die nah am Betrachter sind, mehr Information dargestellt wird als bei den in größerer Entfernung dargestellten Klassen, bei denen nur der Name als Text zu erkennen ist.
Zusätzlich wird die Nutzung von Animationen vorgeschlagen, um Übergänge zwischen verschiedenen Visualisierungsperspektiven – wie zwischen den beiden gezeigten Abbildungen – anschaulicher zu machen.

.. _gogolla-classdiag-a:

.. figure:: _static/ext_pics/gogolla_classdiag_a.png
    :height: 9cm

    3D-UML-Klassendiagramm aus :cite:`gogolla_towards_1999`


.. _gogolla-classdiag-b:

.. figure:: _static/ext_pics/gogolla_classdiag_b.png
    :height: 9cm

    Diagramm mit nach hinten verschobenen, "unwichtigen" Klassen aus :cite:`gogolla_towards_1999`


.. _gef3d:

Graphical Editing Framework 3D
------------------------------

Bei GEF3D handelt es sich um ein Framework zur Erstellung von Modell-Editoren :cite:`von_pilgrim_gef3d:_2008`.
Das Projekt basiert auf den Konzepten des Grafical Editing Framework der Eclipse Plattform und überträgt diese in den dreidimensionalen Raum.

Mit GEF3D ist es möglich, 3D-Editoren für Eclipse zu erstellen und schon vorhandene, GEF-basierte 2D-Editoren darin einzubetten indem 2D-Elemente auf Flächen im dreidimensionalen Raum gezeichnet würden. 
:num:`Abbildung #gef3d-twt3d` zeigt ein Beispiel für die Darstellung von mehreren Diagrammtypen in einer Ansicht und Verbindungen zwischen Elementen verschiedener Diagramme.

In :num:`Abbildung #gef3d-ecore` ist ein mit GEF3D implementierter Ecore-Editor zu sehen. 
Diese Darstellungsform mit 2D-Elementen, die im 3D-Raum platziert werden können wird als "2.5D"-Darstellung bezeichnet. 
Elemente könnten, wie in der Abbildung zu sehen ist, auf Flächen oder auch frei im 3D-Raum platziert werden :cite:`www:gef3ddevblog`.

Die grafische Ausgabe von GEF3D baut direkt auf OpenGL auf; um 2D-Grafiken und Text zu zeichnen werde Vektorgrafik genutzt, was zu einer besseren Darstellungsqualität im Vergleich zu texturbasiertem 2D-Rendering führe\ [#f4]_\ .

.. _gef3d-twt3d:

.. figure:: _static/ext_pics/772px-ScreenshotTVT3D.jpg
    :height: 10cm

    Kombination mehrerer 2D-Editoren in einer 3D-Ansicht von :cite:`www:gef3d`


.. _gef3d-ecore:

.. figure:: _static/ext_pics/gef3d-ecore-rev436.png
    :height: 10cm

    3D-Ecore-Editor von :cite:`www:gef3ddevblog`


.. raw:: latex



    \pagebreak

3D-Prozessvisualisierung
========================

Arbeiten, die sich speziell mit 3D-Visualisierungen im Kontext des Prozessmanagements (Prozessmodellierung, -simulation) beschäftigen, werden hier vorgestellt. 
Außerdem wird gezeigt, wie sich abstrakte Modelle in eine virtuelle Umgebung integrieren lassen.


.. _betz:

3D-Repräsentation von Prozessmodellen
-------------------------------------

Von :cite:`betz_3d_2008` wird die Visualisierung von Prozessen mittels dreidimensional dargestellter Petrinetze vorgestellt. 
Es werden verschiedene Szenarien gezeigt, in denen 3D-Visualisierungen gewinnbringend genutzt werden könnten. 

Es wird das Problem angesprochen, dass für die Modellierung von Prozessen oft verschiedene Diagrammtypen nötig seien, zwischen denen in üblichen 2D-Werkzeugen zeitraubend gewechselt werden müsse. 
Mehrere Diagrammtypen in eine 3D-Ansicht zu integrieren könne hier Abhilfe schaffen. 

Als Beispiel wird :num:`Abbildung #betz-org-process` eine Kombination eines Organisationsmodells mit einem Prozessmodell gezeigt. 
Neben den Beziehungen zwischen Aktivitäten im Prozessmodell und den Rollen des Organisationsmodells sei es gleichzeitig möglich, Beziehungen im Organisationsmodell, wie die Generalisierung von Rollen oder die Zuordnung von Aktoren zu Rollen zu visualisieren.

.. _betz-org-process:

.. figure:: _static/ext_pics/betz_org_process.png
    :height: 10cm

    Darstellung von Beziehungen zwischen Prozess- und Organisationsmodell aus :cite:`betz_3d_2008` 

Ein weiteres Anwendungsszenario für 3D-Visualisierungen sei es, Ähnlichkeiten zwischen verschiedenen Prozessmodellen aufzuzeigen. 

Im 3D-Raum sei es einfach möglich, zu vergleichende Prozesse nebeneinander auf parallelen Ebenen im Raum zu platzieren.
Verbindungen zwischen Modellelementen der gegenüber gestellten Prozessmodelle könnten dafür genutzt werden, mit verschiedenen Metriken berechnete Ähnlichkeitswerte anzuzeigen. 
Wie in :num:`Abbildung #betz-vergleich-pm` zu sehen ist werden die Werte sowohl durch die Beschriftung als auch durch die Dicke der Verbindungslinien visualisiert. 


.. _betz-vergleich-pm:

.. figure:: _static/ext_pics/betz_vergleich_pm.png
    :height: 8cm

    Visualisierung von Ähnlichkeiten zwischen Prozessmodellen aus :cite:`betz_3d_2008` 

Außerdem könnten hierarchische Prozessdiagramme gut im dreidimensionalen Raum dargestellt werden. 
Der Benutzer könne mehrere Verfeinerungsstufen des Modells in einer Ansicht sehen, wie in :num:`Abbildung #betz-prozess-verfeinerung` gezeigt wird. 


.. _betz-prozess-verfeinerung:

.. figure:: _static/ext_pics/betz_prozess_verfeinerung.png
    :height: 8cm

    Vier Verfeinerungsstufen eines Prozessmodells aus :cite:`betz_3d_2008` 


.. _schoenhage:

3D-Visualisierung für die Prozesssimulation
-------------------------------------------

In :cite:`schonhage_3d_2000` wird ein Prototyp einer interaktiven 3D-Umgebung vorgestellt, der dafür genutzt werden könne, Simulationen von Prozessen zu kontrollieren und dabei anfallende Daten zu visualisieren.
Der Prozess selbst wird, wie in :num:`Abbildung #schoenhage-graph` gezeigt, als 3D-Graph dargestellt, wobei Subgraphen durch den Benutzer nach Bedarf auf- und zugeklappt werden könnten. 
Datenflüsse würden durch animierte Kugeln angezeigt, die sich entlang der Kanten von einem Aktivitätsknoten zum nächsten bewegen würden.
Der Anwender könne durch die Auswahl von Knoten und dem Drücken einer "drill-down-Schaltfläche" eine Visualisierung zugehöriger Prozessdaten öffnen – hier im Beispiel 3D-Histogramme – wie in :num:`Abbildung #schoenhage-all` zu sehen ist. Im Beispiel zeigen die 3D-Histogramme eine Häufigkeitsverteilung (horizontal) von "Wartezeiten" im Laufe von vier Wochen.
Es sei möglich, Ansichten auf den Prozessgraphen zu speichern, um später wieder schnell zu diesen zurückspringen zu können.

.. _schoenhage-graph:

.. figure:: _static/ext_pics/schoenhage_process.png
    :height: 9cm

    Prozessgraph mit "Datenflusskugeln" aus :cite:`schonhage_3d_2000`


.. _schoenhage-all:

.. figure:: _static/ext_pics/schoenhage_all.png
    :height: 12cm

    Darstellung eines Prozesses mit assoziierten Daten (3D-Histogramme) aus :cite:`schonhage_3d_2000`


.. _ross-brown:

Modellierung von Prozessen in interaktiven, virtuellen 3D-Umgebungen
---------------------------------------------------------------------

In :cite:`brown_conceptual_2010` wird ein Prototyp eines BPMN-Editors vorgestellt, der Prozesse innerhalb eine virtuellen 3D-Umgebung darstellt
Besonderer Wert sei auf die Zusammenarbeit zwischen mehreren Modellierern und die Prozesskommunikation – auch unter Beteiligung von Personen, die keine Modellierungsexperten sind – gelegt worden. 
"Naive stakeholders" hätten oft Probleme, die abstrakte Welt der konzeptuellen Modellierung zu verstehen, weil der Bezug zu realen Gegenständen fehle. 
Unter Zuhilfennahme einer "virtuellen Welt" (virtual reality), in welche abstrakte Prozessmodelle eingebettet sind, solle dies abgemildert werden. 

In dieser Umgebung könnten Abbilder von realen Entitäten, die mit dem Prozess in Beziehung stehen oder mit diesem interagieren – beispielsweise verwendete Betriebsmittel oder ausführende Personen – dargestellt werden. 
Dies könne auch dazu dienen, den Ort und die räumliche Anordnung von Prozessschritten, beispielsweise durch die Einbettung in ein virtuelles Gebäude, zu visualisieren. 
Möglich sei auch eine Simulation der Prozessausführung in der virtuellen Welt.
Dadurch solle es den Beteiligten leichter möglich sein, festzustellen, ob das Modell die Realität richtig abbilde und ob eventuell Probleme bei der Umsetzung des Prozesses in der Realität auftreten könnten.

Wie in :num:`Abbildung #brown-process` zu sehen ist, werden Prozesse als 3D-Graph dargestellt, wobei als Knoten auf 3D-Objekte übertragene BPMN-Modellelemente genutzt werden.
Auf den Knoten können Informationen durch Texte oder statische Grafiken vermittelt werden. 

.. _brown-process:

.. figure:: _static/ext_pics/brown_prozessgraph.png

    BPMN-Prozessgraph in virtueller Welt aus :cite:`brown_conceptual_2010` 


Informationen auf den Objekten scheinen nur auf einer Seite dargestellt zu sein. Das ist problematisch, falls Modellelemente gedreht und Bewegungen um den Prozessgraphen herum ausgeführt werden. 
Je nach Perspektive wäre es möglich, dass die Texte bzw. die Symbole nicht mehr sichtbar sind.
:num:`Abbildung #brown-process` zeigt auch, dass die gegenseitige Verdeckung von Modellelementen ebenfalls zu Schwierigkeiten bei der Lesbarkeit der Informationen führt.
Die Benutzer selbst werden, wie in :num:`Abbildung #brown-nodes` zu sehen ist, als Avatar gezeigt, welcher die Interaktion der Benutzer mit dem Modell für andere Teilnehmer zeigen soll.

.. _brown-nodes:

.. figure:: _static/ext_pics/brown_nodes.png
    :height: 6cm

    Benutzer-Avatar vor 3D-BPMN-Elementen aus :cite:`brown_conceptual_2010` 

Es gebe die Möglichkeit, "Kommentarwände" zur Anzeige von Texten für die Kommunikation zwischen den Beteiligten aufzustellen.
Daneben könnten auch andere Multimedia-Inhalte wie Videos, Tonaufnahmen oder Statistiken zur Prozessausführung (über Web-Services) eingebettet werden.
Dies ist in :num:`Abbildung #brown-datadisplay` zu sehen.

.. _brown-datadisplay:

.. figure:: _static/ext_pics/brown_datadisplay.png
    :height: 6cm

    Kommentarwände und Multimedia-Inhalte in der virtuellen Welt aus :cite:`brown_conceptual_2010` 


Verbesserung der dreidimensionalen Darstellung von Graphen
==========================================================

3D-Szenen werden auf üblichen Arbeitsplatz-Bildschirmen zu einer zweidimensionalen Projektion reduziert :cite:`akenine-moller_real-time_2008`. 
Dies bedeutet, dass die Vorteile einer dreidimensionalen Darstellung, welche sich aus der Tiefenwirkung ergeben, nicht vollständig zur Geltung kommen. 
Um dies zu umgehen lassen sich Techniken wie die Stereoskopie, Bewegungsparallaxe\ [#f2]_ oder voll immersive virtuelle Umgebungen (oft als CAVE bezeichnet\ [#f3]_) einsetzen.

.. _ware-graphs:

Nutzung von 3D-Effekten für einen verbesserten Tiefeneindruck
-------------------------------------------------------------

In :cite:`ware_visualizing_2008` wird an Probanden untersucht, wie groß die Vorteile einer stereoskopischen 3D-Darstellung von umfangreichen Graphen im Vergleich zu einer 2D-Darstellung sind. 
Als Maß für die "Lesbarkeit" wird hier das Abschneiden bei der Aufgabe, die Pfadlänge zwischen zwei markierten Knoten zu erkennen, genutzt. 

Stereoskopische 3D-Darstellung sei besonders hilfreich, um dem Betrachter einen realistischen Tiefeneindruck zu vermitteln und damit das Erkennen von Verbindungen zu erleichtern. 
Eine weitere Maßnahme, um den Tiefeneindruck zu verbessern, sei es, den Graphen ständig zu rotieren und damit die Bewegungsparallaxe zu nutzen\ [#f2]_.
Es zeigte sich, dass die Probanden – bei gleicher Fehlerrate – Verbindungen in 3D-Graphen erkennen hätten können, welche um eine Größenordung größer gewesen seien als die entsprechenden 2D-Graphen.

Dabei sei eine Anzeige mit einer sehr hohen Auflösung verwendet worden, die nahe an das Auflösungsvermögen des menschlichen Sehsystems herankomme. 
Eine frühere Untersuchung mit ähnlicher Konzeption :cite:`ware_evaluating_1996` zeigte deutlich kleinere Vorteile für die stereoskopische 3D-Darstellung. 
Dies wird in der späteren Arbeit auf den Umstand zurückgeführt, dass hierbei Anzeigen mit einer viel niedrigeren Auflösung verwendet worden seien. 

.. _halpin-social-net:

3D-Visualisierung von Graphen in voll immersiven virtuellen Umgebungen
----------------------------------------------------------------------

Neben der Anzeige von 3D-Graphvisualisierungen auf handelsüblichen Arbeitsplatz-Rechnern könnten dafür auch immersive 3D-Umgebungen (fully immersive virtual reality) genutzt werden. 

So zeigt :cite:`halpin_exploring_2008` die Visualisierung von sozialen Netzwerken in einer CAVE-Umgebung. 
Benutzer könnten so direkt mit der Graphdarstellung der Daten in einer natürlichen Art und Weise interagieren und einen realitätsnahen räumlichen Eindruck von der virtuellen Welt bekommen. 

Der Graph würde zu Beginn in einer "2D-Darstellung" in einer Ebene vor dem Benutzer angezeigt, wie in der :num:`Abbildung #halpin-extrude` (unten) zu sehen ist. 
Links ist zu sehen, wie durch das "Berühren" mit einem virtuellen Werkzeug (grauer Quader) die mit dem Knoten assoziierten Daten angezeigt werden können.

Wenn sich ein Benutzer speziell für die Verbindungen eines bestimmten Knoten interessiere, sei es möglich aus dieser Darstellung den gewünschten Knoten zu "extrudieren", also zu sich heranzuziehen. 
Wie in :num:`Abbildung #halpin-extrude` (rechts) zu sehen ist werden dadurch die Verbindungen des Knotens hervorgehoben.

.. _halpin-extrude:

.. figure:: _static/ext_pics/halpin_extrude_mod.png
    :height: 11cm

    Visualisierung von semantischen Netzwerken aus :cite:`halpin_exploring_2008`


.. _related-zusammenfassung:

Zusammenfassung und Bewertung
=============================

Es wurden verschiedene Ansätze gezeigt, zu einer 3D-Visualisierung von Informationen zu gelangen und deren Vorteile zu nutzen. 
So lässt sich häufig der Ansatz beobachten, von einer bekannten 2D-Visualisierung auszugehen und diese in den 3D-Raum zu übertragen.
Dies war besonders bei den verschiedenen Arbeiten zu sehen, die sich mit 3D-UML beschäftigen.

Der vorliegende Abschnitt fasst die wichtigsten Nutzungmöglichkeiten der dritten Dimension zusammen, die in den gezeigten Arbeiten vorgeschlagen wurden. 
Außerdem wird versucht, eine Einschätzung zu geben, wie vorteilhaft die gezeigte (3D-)Visualisierung im Vergleich zu einer reinen 2D-Darstellung ist.
Die folgende Tabelle gibt einen Überblick über die vorgestellten Verwendungsmöglichkeiten und deren möglichen Nutzen.

|

.. include:: table1.rst

|

2,5D / 3D-Visualisierungen von Modellen
---------------------------------------

Eine naheliegende Möglichkeit ist es, schon bekannte 2D-Modellierungssprachen wieder zweidimensional auf Flächen im 3D-Raum zu platzieren. 
Dies wurde von :ref:`McIntosh<mcintosh>` für UML-Zustandsdiagramme, von :ref:`Gil und Kent<gil>` oder bei :ref:`GEF3D<gef3d>` gezeigt.
In der Tabelle werden solche Darstellungsformen als 2,5D-Ansatz bezeichnet.
Für die Implementierung bedeutet das, dass sich schon vorhandene 2D-Bibliotheken nutzen lassen, deren Grafikausgabe direkt auf die Flächen gezeichnet wird.
Für den Benutzer hat die Darstellung den Vorteil, dass sich die Darstellung der Modellelemente selbst nicht ändert und sich mehrere Modelle gleichzeitig darstellen lassen, indem die Ebenen zueinander versetzt werden. 

Wie von :ref:`Gil und Kent<gil>` erwähnt, lassen sich durch eine dreidimensionale Anordnung verschiedene Beziehungstypen besser unterscheiden als in reinen 2D-Ansichten. 
Modellhierarchien und Beziehungen zwischen verschiedenen Modellen lassen sich gut darstellen, indem beispielsweise Linien zwischen assoziierten Elementen oder zu Unterdiagrammen gezeichnet werden.
Diese Beziehungen lassen sich schon durch die Anordnung optisch leicht von denjenigen unterscheiden, die innerhalb eines (Teil-)Modells bestehen und in einer Ebene mit den Modellelementen liegen. 
In reinen 2D-Darstellungen ist diese Unterscheidung deutlich schwieriger und es muss üblicherweise auf unterschiedliche Farben oder Konnektoren zurückgegriffen werden. 
Vor allem bei einer großen Anzahl von Elementen kann dies leicht zu verwirrenden Darstellungen führen.

Problematisch ist dagegen bei 2,5D-Ansätzen, dass es bei "schrägen" Betrachtungswinkeln schwierig wird, Informationen abzulesen, was sich besonders bei Schriftdarstellung bemerkbar machen wird. 
Außerdem ist bei den bisher genannten Visualisierungsformen die Möglichkeit eingeschränkt, die dritte Dimension zur Vermittlung von zusätzlichen Informationen zu nutzen, da Elemente immer auf festen Ebenen platziert werden müssen.
Kontinuierliche Attribute der Modellelemente lassen sich so nicht darstellen.

Als Weiterentwicklung lässt sich die von :ref:`Krolovitsch und Nilsson<krolovitsch>` vorgestellte Visualisierung von Zustandsdiagrammen betrachten, die ebenfalls 2D-Flächen nutzt, jedoch die Elemente aus der Ebene herausragen lässt.
So wirkt die Darstellung etwas "plastischer" und Strukturen lassen sich besser erkennen. 
Die Höhe der Elemente lässt sich hier nutzen, um Werte von kontinuierlichen Modellattributen direkt zu visualisieren.

Interessant ist die dort gezeigte Möglichkeit, Subdiagramme temporär auszublenden und durch ein einzelnes Symbol zu ersetzen.
Dies wäre auch in der Prozessmodellierung hilfreich für die Darstellung von kompositen Prozessen.
So kann beispielsweise durch einen Doppelklick auf einen Prozessknoten ein weiteres Modell in der 3D-Szene angezeigt werden ohne ein neues Fenster zu öffnen, wie es in 2D-Werkzeugen praktiziert wird.

:ref:`Betz et al.<betz>` zeigten für den Bereich der Prozessmodellierung die schon für die Softwaremodellierung genannten Nutzungsmöglichkeiten des 3D-Raums, also die hierarchische Darstellung von Prozessdiagrammen und die Visualisierung von Beziehungen zwischen unterschiedlichen Modellarten.

Von :ref:`Gogolla et al.<gogolla>` wurden UML-Diagramme mit "echten", frei plazierbaren 3D-Objekten gezeigt. 
Die dritte Dimension ("Tiefe") lässt sich dazu nutzen, schon durch Verbindungen festgelegte Zusammenhänge zu verdeutlichen oder um Attribute der Modellelemente zu visualisieren.

3D-Objekte wie Quader haben den Vorteil, dass sich Information – oft in Textform — auf mehreren Seiten darstellen lässt. 
Wie von :ref:`Dwyer<dywer>` vorgeschlagen, gibt es die Möglichkeit, diese Objekte so zu drehen, dass dem Benutzer immer eine Seite zugewandt und damit gut lesbar ist.
Damit lassen sich solche Modelle besser aus unterschiedlichen Perspektiven betrachten als die vorgenannten 2,5D-Darstellungen.
Der Wechsel der Perspektive kann hilfreich sein, um unterschiedliche Aspekte der 3D-Szene gezielt betrachten zu können. 
Beispielsweise können so Beziehungen zwischen Elementen auf parallelen Ebenen herausgestellt werden, indem die Szene "von der Seite" betrachtet wird.

.. _informations-integration:

Integration von weiteren Informationen in 3D-Visualisierungen
-------------------------------------------------------------

Neben dem abstrakten Prozessmodell an sich lassen sich auch weitere Informationen dreidimensional darstellen. 
So zeigten :ref:`Schönhage et. al.<schoenhage>`, wie sich aus einer Simulation des Prozesses gewonnene Daten neben dem Prozessmodell anzeigen lassen.
Dies ist aber prinzipiell mit reinen 2D-Darstellungen ebenfalls möglich. 
Um hier einen klaren Vorteil der 3D-Visualisierung erkennen zu können, müssen sich die Daten selbst sinnvoll dreidimensional darstellen lassen.
Ein Beispiel dafür sind die 3D-Histogramme, wie sie von Schönhage gezeigt wurden.

:ref:`Brown<ross-brown>` bettet die abstrahierte Darstellung des Prozesses in eine virtuelle Umgebung ein, welche den tatsächlichen Ausführungsort eines Prozesses räumlich abbilden kann. 
Dies lässt sich als deutlicher Vorteil für die Nutzung von 3D-Visualisierungen im Vergleich zu 2D-Darstellungen festhalten.

Beispielsweise lassen sich Laufwege von am Prozess beteiligten Personen oder andere Vorgänge wie der Transport von Werkstücken (animiert) darstellen, um mögliche Probleme bei der Ausführung und Optimierungsmöglichkeiten aufzuzeigen. 
So kann festgestellt werden, ob sich gewisse Wege verkürzen oder vermeiden lassen, indem Reihenfolge oder der Ausführungsort von Prozessschritten verändert werden.
Durch die Integration von Abbildern realer Objekte in die virtuelle Welt können abstrakte Konzepte des Prozessmodells veranschaulicht oder um weitere Informationen ergänzt werden. 
Sinnvoll ist dies beispielsweise, um Veränderungen an einem Werkstück im Laufe eines Produktionsprozesses zu visualisieren, indem die Zwischenstufen dreidimensional neben den Prozessschritten abgebildet werden.

Eine andere denkbare Anwendung wäre eine Visualisierung der Platzverhältnisse in einer Ausführungsumgebung. 
Wenn die Umgebung sowie sich darin befindliche Objekte relativ zueinander im richtigen Größenverhältnis und in der tatsächlichen Form dargestellt sind, könnte schon bei einer Betrachtung des Prozesses in der virtuellen Welt bemerkt werden, dass vorgesehene Ablageplätze in einem Lager oder Transportbehälter für ein Werkstück zu klein dimensioniert sind.

:num:`Abbildung #brown-airport`\ [#f6]_ zeigt einen BPMN-Prozess in einer 3D-Umgebung, die mit Hilfe des von Brown vorgestellten Editors erstellt wurde und einen Flughafen darstellen soll. 
Dies ließe sich prinzipiell auch mit einer 2D-Darstellung realisieren, indem die Szene von oben gezeigt wird. 
Dadurch wird aber die Übersichtlichkeit eingeschränkt; die Möglichkeit, den Prozessgraphen im dreidimensionalen Raum in einer Ebene über dem Boden zu zeigen, erweist sich hier als Vorteil.
Ebenso bietet es sich an, im 3D-Raum die Betrachtungsperspektive nach Bedarf zu verändern. 
Ist die Blickrichtung des Betrachters annähernd parallel zum Untergrund wie in der Abbildung, lassen sich auch weit entfernte "Stationen" des Prozesses erkennen.
Ein Blick von oben auf die Szene aus größerer Entfernung gibt dagegen einen groben Überblick über die gesamte Prozessstruktur.

.. _brown-airport:

.. figure:: _static/ext_pics/brown_airport.png
    :width: 16.5cm

    Visualisierung eines BPMN-Prozesses in einer virtuellen Umgebung

Effizienz und Akzeptanz von 3D-Darstellungen?
---------------------------------------------

Die wichtige Frage, ob und in welchen Situationen 3D-Visualisierungen Vorteile gegenüber 2D-Darstellungen haben, die über eine reine Verbesserung des Erscheinungsbilds hinausgehen,  kann von den gezeigten Arbeiten sicher nicht vollständig beantwortet werden.
Es wurden immerhin einige Hinweise zur Effizienz gegeben, indem beispielsweise Benutzerstudien durchgeführt wurden, welche Vorteile für 3D-Darstellungen in der Softwaremodellierung andeuten, jedoch auch Probleme aufzeigen :cite:`dwyer_three_2001` :cite:`mcintosh_x3d-uml:_2008` :cite:`halpin_exploring_2008`.
Untersuchungen zur Effizienz, die sich speziell auf die Prozessmodellierung beziehen, ließen sich nicht finden.

Inwieweit 3D-Darstellungen für die Prozessmodellierung nützlich sind, hängt sicher auch davon ab, wie komplex die Modelle sind. 
Einfache Modelle lassen sich auch mit den Hilfsmitteln der 2D-Visualisierung adäquat darstellen. 
Prinzipiell ist eine 2D-Darstellung von Modellen wohl für die meisten Benutzer etwas intuitiver zu verstehen, da diese schon länger verbreitet und beispielsweise aus UML- oder BPMN-Werkzeugen bekannt ist.
Wie erwähnt, werden die Vorteile von 3D-Darstellungen dann deutlich, wenn es darum geht, viele Verbindungstypen darzustellen oder hierarchische Modelle in einer Szene darzustellen. 
Ob 3D-Visualisierungen effizient sind und ob sich deren höhere Komplexität – sowohl für den Benutzer als auch für die Implementierung – auszahlt muss daher sicher abhängig vom konkreten Anwendungsfall bewertet werden.
 
Bei der Betrachtung der Effizienz muss auch berücksichtigt werden, dass die Erfahrung der Benutzer mit 3D-Darstellungen aus anderen Bereichen – beispielsweise aus Computerspielen oder 3D-CAD-Werkzeugen – eine Rolle spielt
:cite:`dwyer_three_2001` :cite:`ware_visualizing_2008` :cite:`schonhage_3d_2000`.
Es stellt sich die Frage, wie unerfahrene Benutzer an 3D-Werkzeuge für die Prozessmodellierung herangeführt werden könnten, um diese effizient nutzen zu können.

In eine ähnliche Richtung geht die Fragestellung, inwieweit 3D-Werkzeuge überhaupt von Benutzern akzeptiert werden. 
:cite:`schonhage_3d_2000` bemerkte, dass 3D-Visualisierungen oft als reines "Spielzeug" angesehen würden, die keinen wirklichen Nutzen gegenüber 2D-Darstellungen brächten, sondern bestenfalls nur "schöner" aussähen.
Daher ist es wichtig, 3D-Visualisierungen für den Benutzer möglichst hilfreich und intuitiv zu gestalten, so dass deren Vorteile klar erkennbar sind.
Um eine hohe Akzeptanz zu erreichen müssten aber auch technische Probleme wie die schlechte Verfügbarkeit von 3D-tauglichen Eingabegeräten oder zu langsame Hardware\ [#f5]_ gelöst werden.
So ergeben sich große Herausforderungen, sowohl auf der konzeptuellen Ebene als auch auf der Seite der Implementierung von 3D-Visualisierungen und -Modellierungswerkzeugen.

Verwendbare Vorarbeiten und Schlussfolgerungen für die Arbeit
-------------------------------------------------------------

In den vorgestellten Arbeiten wurden einige Prototypen für 3D-Modellierungswerkzeuge entwickelt. 
Allerdings war nur von :cite:`dwyer_three_2001` eine freie Version im Internet auffindbar. 
Diese ist allerdings technisch auf einem ziemlich alten Stand und lässt in Sachen Bedienung eher zu wünschen übrig.

Frei verfügbare Softwareprojekte, die schon ein flexibles 3D-Prozessmodellierungswerkzeug realisieren ließen sich nicht finden. 
Als Grundlage für ein solches Werkzeug könnte möglicherweise GEF3D dienen, was jedoch nicht weiter verfolgt wurde. 
Negativ könnte bei GEF3D gesehen werden, dass in letzter Zeit relativ wenige Änderungen an der Codebasis erfolgten und insgesamt eher wenig Aktivität festzustellen ist.

Ein Blick in den Quellcode zeigte, dass das Projekt noch auf "alter" OpenGL-Funktionalität aufbaut und damit die Möglichkeiten moderne Grafikhardware nicht nutzt.
Bei der vorliegenden Arbeit stand es aber im Vordergrund, eine möglichst flexible und "zukunftsorientierte" Grundlage für ein (anpassbares) Prozessmodellierungswerkzeug zu legen, wozu auch eine Grafikausgabe auf dem aktuellen Stand der Technik gehört.

Aus den hier vorgestellten Arbeiten ließen sich jedoch einige Konzepte ableiten, die in i>PM 3D realisiert wurden. 
Es ist für die Arbeit sinnvoll, einen möglichst allgemeinen Visualisierungsansatz zu wählen, der es erlaubt, verschiedene Nutzungsmöglichkeiten der dritten Dimension umzusetzen und diese für verschiedene Anwendungsfälle im Prototypen zu evaluieren.
Daher soll der Prototyp grundsätzlich eine 3D-Graphvisualisierung von Prozessen unterstützen, wie es beispielsweise von :ref:`Brown<ross-brown>` gezeigt wurde. (:ref:`Anforderung (a)<anforderungen>`).
Die Knoten des Graphen werden selbst dreidimensional dargestellt und sind frei in der 3D-Szene platzierbar. Dies stellt prinzipiell den flexibelsten Ansatz dar.
Eine 2,5D oder gar 2D-Darstellung lässt sich als Sonderfall einer 3D-Darstellung betrachten. 
So kann die Darstellung oder die Platzierbarkeit von Elementen – ausgehend vom gewählten 3D-Ansatz – wieder eingeschränkt werden, falls sich dies für eine Anwendung als vorteilhaft herausstellen sollte. 

Abstrakte Prozessmodelle in eine virtuelle Welt einzubetten und so räumliche Information zu nutzen stellt einen vielversprechenden Ansatz dar, aus dreidimensionalen Darstellungen einen großen Vorteil zu ziehen.
Dadurch wird *Anforderung (b)* motiviert, beliebige 3D-Objekte in die Szene einfügen zu können, um damit abstrakte Modellelemente zu illustrieren oder virtuelle Ausführungsumgebungen visualisieren zu können.


.. [#f1] Quellcode und ausführbare Dateien des (weiterentwickelten) Prototyps "WilmaScope" (auf Basis von Java3D) können unter http://wilma.sourceforge.net/ heruntergeladen werden.

.. [#f2] Näheres zu Wahrnehmung von Tiefe siehe :cite:`wickens_three_1989`, :cite:`wp:bewegungsparallaxe` oder :cite:`wp:stereoskopie`.

.. [#f3] Näheres zu CAVE-Systemen siehe :cite:`cruz-neira_surround-screen_1993` oder :cite:`wpe:cave`.

.. [#f4] Ein verbreiteter Ansatz, um 2D-Grafiken und Text in OpenGL darzustellen ist es, diese erst in eine Textur zu zeichnen und diese auf 3D-Objekte aufzubringen. Dies wird auch in dieser Arbeit verwendet.

.. [#f5] Die besagte Arbeit ist 2000 entstanden. Sicherlich ist die Geschwindigkeit heutzutage ein kleineres Problem, aber es lässt sich nicht vernachlässigen. Gerade bei aufwändigen Grafikeffekten oder der Verarbeitung von komplexen Eingabedaten kann man leicht an die Grenzen der Rechenleistung stoßen.

.. [#f6] Quelle: www.youtube.com/watch?v=aUBmvykDhB0. Das Video ist auch auf der beigelegten :ref:`DVD <anhang-dvd>` zu finden.
