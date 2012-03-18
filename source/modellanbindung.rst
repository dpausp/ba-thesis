.. _modellanbindung:

***************
Modellanbindung
***************

In diesem Kapitel werden die Anforderungen an die Modellanbindung und deren Realisierung besprochen. 

Auf eine vollständige Erläuterung der Implementierung wird in dieser Arbeit aus Platzgründen verzichtet. 
Jedoch werden in diesem Kapitel einige Hinweise zur Implementierung gegeben, soweit diese für das Verständnis hilfreich sind.

Details zu Implementierungsfragen und Verweise auf Pakete des Projekts werden in Fußnoten angegeben, die mit römischen Ziffern bezeichnet sind.

Insbesondere soll hier verdeutlicht werden, wie andere Teile des Systems diese Modellanbindung nutzen.

Da das Ziel des Projekts I>PM3D ist, einen interaktiven Prozessmodelleditor zu erstellen ergeben sich die folgenden funktionalen Anforderungen:

#. Erstellen und Löschen von Modellelementen
#. Bewegen, Rotieren und Skalieren von Modellelementen
#. Verändern von Prozessmodellattributen
#. Modifizieren der Visualisierungsparameter von Modellelementen; beispielsweise Farbe oder Schriftart
#. Laden und Speichern von Prozessmodellen und deren visuelle Repräsentation

Da das Projekt auf der Grundlage von Simulator X entwickelt wurde folgt das hier vorgestellte Konzept der Architektur von Simulator-X-Anwendungen.

Modell-Funktionalitäten - ModelComponent
========================================

Wie in :ref:`simulatorx` erläutert bestehen Simulator-X-Anwendungen aus einer Reihe von Komponenten, die eine bestimmte Funktionalität dem restlichen System bereitstellen.

Die ModelComponent stellt folglich dem System alle Funktionen zur Verfügung, die im Zusammenhang mit der Manipulation von Modellen stehen. 

So wird der Zugriff auf die Modelle wird vollständig von der ModelComponent gekapselt; es gibt für andere Systembestandteile keine Möglichkeit, direkt darauf zuzugreifen.
Abgesehen von der erhöhten Übersichtlichkeit und Wartbarkeit wird dies auch durch die actor-basierte und damit parallele Architektur von Simulator X vorgegeben.

Die Funktionen werden zum Teil als sogenannte *Commands* bereitgestellt, die über das Kommunikationssystem von :ref:`simulatorx` an die ModelComponent geschickt werden.
In der momentanen Umsetzung werden diese Commands ausschließlich durch die Editorkomponente genutzt, die von :cite:`uli` beschrieben wird.

Es existieren Commands für die folgenden Funktionen [#I]_\ :

* Laden von Metamodellen
* Laden, Speichern, Erstellen, Schließen und Löschen von Usage-Modellen
* Erstellen und Löschen von Knoten und Szenenobjekten
* Erstellen einer Kante zwischen zwei Knoten

Alle anderen Manipulationsmöglichkeiten, das sind diejenigen, die nur Parameter einzelner Modellelemente betreffen werden über Modell-Entitäten bereitgestellt.

Im Folgenden wird das Laden und Speichern von Modellen sowie die dafür genutzte Funktionen beschrieben, soweit diese an keiner anderen Stelle in dieser Arbeit vorgestellt wurden.

Modell-Persistenz
-----------------

Eine Anforderung an den Protoypen ist es, neue Modelle erstellen, diese abzuspeichern und wieder laden zu können. 
Wie in :ref:`modellhierarchie` angesprochen werden in I>PM3D Modelle eingesetzt, die in der Sprache LMMLight verfasst werden.

Diese Modelle werden in Dateien in einer textuellen Darstellung abgelegt und daraus wieder geladen.

Für das Laden wird der im Rahmen dieser Arbeit entstandene LMMLight-Parser\ [#II]_ genutzt, der mit Hilfe der vorher vorgestellten :ref:`parser-kombinatoren` implementiert wurde.
Der Parser liefert einen Syntaxbaum der textuellen Eingabe, der aus "unveränderlichen" (immutable) Objekten aufgebaut ist.

Um die Modelle in der Anwendung verändern zu können werden diese in eine andere Struktur überführt. 
Der so erzeugte Objektbaum ist an die an die EMF-Repräsentation zur Laufzeit angelehnt, wie sie in OMME von XText erzeugt wird.

Die Wurzel wird durch eine Instanz von MModel gebildet, der sich MLevels unterordnen, die wiederum MPackages enthalten usw. 

Der Vorteil zur Nutzung von XText ist, dass es sich hier Objekte, die die Vorteile von Scala nutzen und daher in einer Scala-Umgebung bequem genutzt werden können. 
Besonders deutlich wird das bei den von Scala bereitgestellten Collections, die deutlich mehr Funktionalität bieten als die von Java oder EMF bereitgestellten.

Die **ResourceUtils**\ [#II]_ stellen Methoden für das Laden von Modellen aus Dateien bereit, die dem Aufrufer eine Referenz auf eine MModel-Instanz geben.

Laden von Metamodellen
^^^^^^^^^^^^^^^^^^^^^^

Wie in :ref:`modellhierarchie` beschrieben wurde, werden Metamodelle für die Spezifikation der verwendeten Modellierungssprache und deren Repräsentation eingesetzt. 
Diese sollen prinzipiell austauschbar sein. Dazu wird von der ModelComponent die Funktion bereitgestellt, Metamodelle zur Laufzeit zu laden.

Um das Laden der Modelle anzustoßen ist folgendes Command definiert:

.. code-block:: scala

    final case class LoadMetaModels(domainModelPath: String, editorModelPath: String, loadAsResource: Boolean) extends Command

LoadAsResource gibt an, ob die Pfade als Java-Resource-Path zu einer Metamodell-Datei interpretiert ("true") oder direkt im Dateisystem gesucht werden sollen ("false").

Es wird zur Vereinfachung der Implementierung davon ausgegangen, dass die Metamodelle der Domäne und des Editors immer paarweise geladen werden. 
Mehrere Repräsentationen zu einer Domäne zu laden ist damit beispielsweise noch nicht möglich.

.. TODO vielleicht mal testen!

Die ModelComponent lässt prinzipiell das Laden von mehreren Metamodell-Paaren zu. Jedoch wird dies von der Editorkomponente noch nicht unterstützt.

.. TODO vielleicht mal testen!

Nachdem Metamodelle geladen werden, werden von der ModelComponent Informationen aus den Modellen ausgelesen, die für die Editorkomponente relevant sind.


Laden und Schließen von Usage-Modellen und Umgang mit mehreren Modellen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Usage-Modelle umfassen den aktuellen Zustand eines Prozessmodells und dessen Repräsentation im Editor. 
Ein konkretes "Prozesmodell" wird geladen, indem das zugehörige Domain- und Editor-Usage-Model geladen werden.

Das Command *LoadUsageModels* ist analog zum Command LoadMetaModels definiert, wie im Abschnitt darüber beschrieben.

Es können von der Anwendung zur Laufzeit mehrere Usage-Modelle (zu denselben Metamodellen) geladen werden. 
In der ModelComponent ist jeweils ein Usage-Model-Paar als "aktiv" gekennzeichnet.
Commands wie das Erstellen von Knoten beziehen sich immer auf das aktive Usage-Model. Welches Modell "aktiv" ist kann über das Command *SetActiveUsageModel* geändert werden.

Modelle können über *CloseUsageModel* wieder geschlossen werden, wobei alle seit dem letzten Speichern erfolgten Änderungen verloren gehen.

Der Umgang mit mehreren Modellen wird auch von der Editorkomponente unterstützt.

Speichern von Usage-Modellen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

"Speichern" bedeutet hier, dass die Änderungen an Modellelementen in das Usage-Model zurückgeschrieben werden und das Modell anschließend in textueller Form persistiert wird.
Analog zum Lade-Command *LoadUsageModels* werden bei *SaveUsageModels* zwei Dateinamen für Domänen- und Editormodell angegeben. Java-Resource-Pfade sind hier nicht erlaubt.

Vereinfachung des Umgangs mit Modellen
--------------------------------------

Um den Zugriff auf die Modelle zu vereinfachen und öfter vorkommende Aufgaben auszulagern wurde eine Reihe von Adaptern\ [#III]_ für die in der Scala-Repräsentation der Modelle genutzten Klassen implementiert.
Beispielsweise gibt es einen MConceptAdapter, dessen Methoden beispielsweise den schnellen Zugriff auf alle zuweisbaren Attribute (*assignableAttributes*), das Setzen von Werten (*setValue*) oder die Abfrage von Concept-Relationen (*instanceOf*) erlauben.

[#f6]_

Für alle Adapter werden :ref:`implicit` angeboten, die die gekapselten Objekte direkt um die Methoden "erweitern", die in den Adaptern definiert sind.


Modell-Entitäten
================

Objekte, mit denen verschiedene Teile des Systems interagieren werden in ref:`simulatorx` durch Entities beschrieben. 

Es ist daher zweckmäßig, für jedes Modellelement, also für Knoten und Verbindungen sowie für Szenenobjekte eine zugehörige Entity zu erstellen.
*ModelEntities* werden von der ModelComponent erzeugt, wenn über ein Command die Erstellung von neuen Elementen angefordert wird oder ein Modell geladen wird. 
Näheres zum Ablauf wird im Abschnitt :ref:`lebenszyklus` dargelegt.

Aspekte
-------

Wie aus :ref:`simulatorx` bekannt sind Entity-Definitionen aus Aspekten aufgebaut, die einzelnen Komponenten zugeordnet sind. 
Die für ModelEntites genutzten Aspects werden hier aufgeführt.

Grafik
^^^^^^

Die :ref:`Renderkomponente` stellt verschiedene RenderAspects bereit, die der Renderkomponente alle nötigen Informationen mitteilen, um ein Visualisierungsobjekt [#f1]_ zur entsprechenden Entity anzulegen.

Für Knoten und Kanten wird der *ShapeFromFactory*-Aspect genutzt, der besagt, dass das Objekt nicht von der Renderkomponente, sondern – in diesem Fall – von der ModelComponent erstellt wird. 
Näheres hierzu wird weiter unten im Abschnitt :ref:`lebenszyklus` dargestellt.

Szenenobjekte, für die es bisher nur die Möglichkeit gibt, diese aus COLLADA-Modelldateien zu laden werden von der Renderkomponente erzeugt. 
In der Entity-Beschreibung wird dafür der *ShapeFromFile*-Aspect angegeben.

Physik
^^^^^^

Knoten und Szenenobjekte sollen in die physikalische Simulation aufgenommen werden, um Kollisionen zu erkennen und eine Auswahl der Elemente zu ermöglichen. 

Dafür stellt die Physikkomponente verschiedene Aspects bereit, die besagen, dass eine bestimmte Geometrie für die entsprechende Entity genutzt werden soll.
Da bisher nur annähernd quaderförmige Geometrien für die Visualisierung von Knoten genutzt werden, wird hier für alle Knoten der *PhysBox*-Aspect verwendet.

Kanten definieren keinen Physik-Aspect und besitzen daher keine physikalische Darstellung.

Dies ist nicht nötig, da die Auswahl von Kanten nicht unterstützt werden soll und Kollisionen mit Verbindungen eher als hinderlich gesehen wurde.
Außerdem könnte eine große Anzahl von Verbindungen schnell zu Geschwindigkeitsproblemen der Simulation führen.

.. kann vielleicht weg, wenn buchi was dazu schreibt oder in den Übersichtsartikel

Modell
^^^^^^

Für die drei Elementtypen Knoten, Kanten und Szenenobjekte gibt es jeweils einen Aspect, der von *ModelAspect* abgeleitet ist.

ModelAspects sind der *ModelComponent* zugeordnet und enthalten für Nutzer der ModelEntity relevante Informationen. 

Für alle Elemente, die von ModelEntities repräsentiert werden wird ein vollqualifizierter Name (*modelTypes.Fqn*) vergeben, der das Element eindeutig innerhalb des Systems identifiziert.
Dieser Name wird in Commands verwendet, die sich auf bestimmte Elemente beziehen, wie beispielsweise das Verbinden oder Löschen von Knoten.

Bei Knoten und Kanten wird dafür die FQN des entsprechenden Modellelementes aus dem Domänenmodell genutzt. Szenenobjekte werden über die FQN des Editor-Usage-Concepts identifiziert.\ [#f2]_

Außerdem wird ein Identifikationsstring (modelTypes.CreatorId*) mitgeliefert, der vom Ersteller eines Elements definiert wird. 
Mit "Ersteller" ist hier der Absender des entsprechenden Commands oder die ModelComponent selbst gemeint. 

Diese ID kann von diesem dafür benutzt werden, neu erstellte Entities in internen Datenstrukturen richtig zuzuordnen.

.. _modellanbindung-svars:

SVars
-----

Über die Zustandsvariablen (SVars) der Modell-Entitäten ist es für Aktoren im System möglich, die Parameter eines Modellobjekts zu verändern.

Die von einer ModelEntity angebotenen SVars lassen sich in drei Gruppen einteilen. 
SVars können direkt Attribute aus den beiden zugrunde liegenden (Meta)-Modellen abbilden oder statisch von der ModelComponent definiert sein.

#. *Domain-Model-SVars* 
   Solche SVars werden zu Attributen erzeugt, die im Domänen-Metamodell definiert sind und denen in Concepts im Usage-Model Werte zugewiesen werden können [#f3]_\ . 
   Sie stellen somit die Schnittstelle dar, über die Modellattribute wie die Funktion eines Prozesses oder der Name eines Konnektors verändert werden können.
   Unterstützt werden alle literalen Datentypen; den SVars werden die passenden Scala-Datentypen zugewiesen.

#. *Editor-Model-SVars* 
   Diese SVars werden nach Bedarf aus den Attributen des Editor-Metamodells erstellt. 
   Sie erlauben es, die Visualisierung der Elemente anzupassen, wie sie im Editormodell beschrieben wird.\ [#f4]_
   Neben literalen Attributen werden hier auch Concept-Attribute unterstützt. Diese werden für die meisten hier genannten SVars benötigt.

   Welche Editor-Attribute unterstützt werden wird von der ModelComponent festgelegt.\ [#f5]_ 
   
   Das sind im Einzelnen:

    * Hintergrundfarbe *backgroundColor*
    * Schrift *font*
    * Schriftfarbe *fontColor*
    * Texturpfad *texture*
    * Liniendicke *thickness*
    * Spekulare Farbe *specularColor*

#. *Editor-SVars*
   Dies sind SVars, die keine direkte Entsprechung im Modell haben und deren Werte daher auch nicht persistiert werden. 
   Sie sind automatisch für alle Modellelemente definiert oder werden durch Modellattribute "aktiviert". 
   Dabei handelt es sich um:

   * SVars für die Auswahl von :ref:`visualisierungsvarianten`: 

     * Deaktivierung (*disabled*), 
     * Hervorhebung (*highlighted*)
     * Selektion (*selected*)

   * Parameter für die Visualisierungsvarianten 
     
     * Breite des Selektionsrahmens (*borderWidth*)
     * Hevorhebungsfaktor (*highlightFactor*)
     * Transluzenzfaktor bei deaktivierten Elementen (*deactivatedAlpha*)
    
   Alle hier genannten SVars werden von der ModelComponent aktiviert, wenn im Modell das Attribut *interactionAllowed* auf "true" gesetzt ist.
   

Alle SVars müssen eindeutig durch einen SVar-Typ beschrieben werden, der ein Symbol zur Identifizierung und einen Scala-Datentyp umfasst. Die Symbole für Editor-SVars beginnen mit 'editor', die Symbole für Domänenmodell-SVars werden mit 'model' gekennzeichnet. Daran wird der Attributname aus dem Modell oder im Falle der statischen Editor-SVars die oben genannten Bezeichner angehängt, abgetrennt durch einen Punkt.

Beispiele für SVar-Bezeichner aus den vorher genannten SVar-Kategorien: 

#. ``model.function``
#. ``editor.color``
#. ``editor.disabled``

.. _lebenszyklus:

Übersicht über den Lebenszyklus von Model-Entitäten
===================================================

SVarSupport
-----------

.. TODO

Um den Umgang mit den Drawables für Modellelemente zu vereinfachen wurden verschiedene traits erstellt, die das abstrakte trait SVarSupport implementieren. 

Damit lassen sich SVars direkt mit Attributen der Drawables verbinden.

In den SVarSupports werden in der Methode connectSVars für passende SVar-Typen Observe-Handler registriert. 

Im einfachsten Fall bestehen diese Handler nur aus einem Setter, der direkt Attribute des Drawables setzt sobald sich der Wert der SVar ändert.



.. [#f2] Dass hier die FQNs aus dem Modell genutzt werden hat keine besondere Bedeutung und ist nur ein "Implementierungsdetail", auf das man sich nicht verlassen solle.

.. [#f3] Die Regeln für die Zuweisbarkeit ...

.. [#f4] Es wäre auch erlaubt, Attribute zu integrieren, die nicht direkt die Visualisierung betreffen, aber das Editor-Verhalten modifizieren. Dies wird bisher aber nicht genutzt.

.. [#f5] Es war nicht möglich, die Implementierung (auf einfachem Wege) so flexibel zu gestalten wie bei Domain-Model-SVars, was leider dazu führt, dass man keine Attribute hinzufügen kann ohne die ModelComponent anzupassen.

.. [#f6] Gewisse Ähnlichkeiten mit anderen Projekten sind rein zufällig ;-)

.. [#I] Zu finden im Scala-Package mmpe.lmmlight.parser

.. [#II] Alle Commands sind in Scala-Package mmpe.model.commands definiert.

.. [#III] Die Adapter sind im Package mmpe.lmmlight zu finden.
