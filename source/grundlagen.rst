**********
Grundlagen
**********

Prozessmodellierungssprachen
============================

Prozessmodellierung
-------------------

* Modellbildung allgemein

* Prozess, Kontrollfluss und so

Klassifikation von Modellierungssprachen
----------------------------------------

Modellierungssprachen lassen sich prinzipiell in textuelle und graphische Notationen unterscheiden. 

Textuelle Sprachen eignen sich besonders für die automatische Verarbeitung und Transformation in andere Sprachen.

In graphischen Sprachen können Beziehungen zwischen Elementen auf zwei Arten, graphbasiert oder die geometriebasiert dargestellt werden.

Bei der graphbasierten Variante werden grafische Elemente mittels Kanten verbunden. 

Im geometriebasierten Fall wird Zusammengehörigkeit durch "Enthaltensein" dargestellt; ein Element wird innerhalb eines anderen gezeichnet.

Es ist auch möglich, beide Ansätze in einer Modellierungssprache zu kombinieren (Hybrid).


.. _popm:

Perspektivenorientierte Prozessmodellierung
===========================================

Funktionale Perspektive 
    hier werden die funktionalen Einheiten, allgemein "Prozess" genannt angegeben.

Verhaltensbezogene Perspektive 
    dies wird auch als "Kontrollfluss" bezeichnet und gibt die zeitlichen bzw. logische Abfolge der funktionalen Einheiten an, also die Ausführungsreihenfolge an sich an. 

Organisationsbezogene Perspektive 
    jeder Prozess lässt sich einer ausführenden Entität, zum Beispiel eine Rolle oder eine konkreten Person zuordnen, die für die Ausführung verantwortlich ist.

Datenbezogene Perspektive 
    Prozessmodelle sind ohne ausgetauschte und erstellte / modifizierte Daten quasi undenkbar. Datenflüsse legen oft die Abhängigkeiten zwischen Prozessen fest.

Operationsbezogene Perspektive 
    Zur Ausführung von Prozessen sind immer verschiedene Betriebsmittel erforderlich.

Dies soll explizit keine vollständige Aufzählung, sondern nur eine Darstellung oft relevanter Aspekte sein. Die Objekte der Prozessmodellierung sind sehr vielgestaltig und die Anforderungen können sehr unterschiedlich sein. Das bedingt eine flexible Modellierungssprache. So können nach Bedarf weitere Perspektiven hinzugefügt werden.

In diesem Zusammenhang soll auch erwähnt werden, dass es oft notwendig ist, die Granularität von Prozessen dynamisch zu verändern, je nachdem, welche Informationen im konkreten Fall gefragt sind. Prozesse können daher komposit (oder auch komplex genannt) sein und weitere, Subprozesse enthalten, die in einem grobgranularen Diagramm nicht darstellt werden. Bei Bedarf können diese Unterprozesse separat in einem Diagramm betrachten werden (Umsetzung im Tool).

.. _metamodellierung:

Metamodellierung
================

Die schon erwähnte, nötige Flexibilität von Prozessmodellen (eigentlich Modellierungssprachen allgemein) erfordert oft, dass die Modellierungssprache selbst verändert werden kann. 

Dies wird allgemein mit dem Begriff Metamodellierung bezeichnet. Das Metamodell stellt wiederum ein Modell für Modelle dar, die oft als Instanzen des Metamodells bezeichnet werden.


Ein Beispiel für eine Metamodellierungsumgebung ist das OMME, Open Meta Modelling Environment, das an demselben Lehrstuhl wie diese Arbeit entstanden ist in :cite:`volz_werkzeugunterstuetzung_2011` beschrieben wurde. 

Ein Hauptbeitrag dieser Arbeit ist dasLinguistic Meta Model (LMM), dass der Idee der orthogonale Klassifikation folgt. 

Von den speziellen Modellierungskontrukten dieser Arbeit ist besonders die Spezialisierung von Instanzen für die Prozessmodellierung und auch für diese Arbeit interessant. Diese erlaubt es, von schon vorhandenen Konzepten neue Verwendungen anzulegen, in denen Parameter des spezialisierten Original-Konzepts überschrieben bzw. ergänzt werden können. Wird keine Wertzuweisung in der Verwendung vorgenommen gelten die Werte des orginalen Konzepts.

Mit diesem Modellierungsmuster lässt sich das "Typ-Verwendungs-Konzept" [Jabl?!] in der (perspektivenorientierten) Prozessmodellierung realisieren. Dies bedeutet, dass von Modellelementen, beispielsweise für einen Prozess erst ein Typ als Instanz angelegt werden muss, von welchem dann beliebig viele Verwendungen erzeugt werden können. Diese Verwendungen werden dann in einem konkreten Prozessmodell genutzt. Diese Paradigma wird auch in dieser Arbeit umgesetzt.

Ein Bestandteil von OMME ist das Model Designer Framework, mit dessen Hilfe grafische Editoren für beliebige Modellierungssprachen definiert werden können.

Der Editor wird selbst als Metamodell beschrieben, wobei das Graphical Meta Model die grafischen Modellelemente definiert und das  Editor Meta Model die Verbindung zu einem Metamodell der Domäne herstellt. Diese Arbeit folgt einem ähnlichen Muster, um auf Änderungen an der Prozessmodellierungssprache zu reagieren und die Adaption von neuen Domänenmodellen zu ermöglichen.

Mit Hilfe von MDF wurde beispielsweise neben einem Editor für ER-Modelle ein Prozessmodellierungswerkzeug basierend auf dem POPM-Konzept erstellt (IPM2). Dieser Editor diente ebensfalls als Vorbild für diese Arbeit (und auch fürs ganze Projekt).
