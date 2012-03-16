.. _ref_konzept_modellanbindung:

***************
Modellanbindung
***************

In diesem Kapitel werden die Anforderungen an die Modellanbindung und deren Realisierung besprochen. 

Auf eine vollständige Erläuterung der Implementierung wird in dieser Arbeit aus Platzgründen verzichtet. 
Jedoch werden in diesem Kapitel einige Hinweise zur Implementierung gegeben, soweit diese für das Verständnis hilfreich sind.

Details zu Implementierungsfragen und Verweise auf Pakete des Projekts werden in Fußnoten angegeben, die mit römischen Ziffern bezeichnet sind.

Insbesonders soll hier verdeutlicht werden, wie andere Teile des Systems diese Modellanbindung nutzen.

Da das Ziel des Projekts I>PM3D ist, einen interaktiven Prozessmodelleditor zu erstellen ergeben sich die folgenden funktionalen Anforderungen:

#. Erstellen und Löschen von Modellelementen
#. Bewegen, Rotieren und Skalieren von Modellelementen
#. Verändern von Prozessmodellattributen
#. Modifizieren der Visualisierungsparameter von Modellelementen; beispielsweise Farbe oder Schriftart
#. Laden und Speichern von Prozessmodellen und deren visuelle Repräsentation

Da das Projekt auf der Grundlage von Simulator X entwickelt wurde folgt das hier vorgestellte Konzept der Architektur von Simulator-X-Anwendungen.

ModelComponent
==============

Wie in :ref:`simulator_x` erläutert bestehen Simulator-X-Anwendungen aus einer Reihe von Komponenten, die eine bestimmte Funktionalität dem restlichen System bereitstellen.

Die ModelComponent stellt folglich dem System alle Funktionen zur Verfügung, die im Zusammenhang mit der Manipulation von Modellen stehen. 
Diese Funktionen werden als sogenannte *Commands* bereitgestellt. 

Es existieren Commands für die folgenden Funktionen [I]_\ :

* Laden von Metamodellen
* Laden, Speichern, Erstellen, Schließen und Löschen von Usage-Modellen
* Erstellen und Löschen von Knoten und Szenenobjekten
* Erstellen einer Kante zwischen zwei Knoten

Alle anderen Manipulationsmöglichkeiten, also diejenigen, die nur Parameter von einzelnen Modellelementen betreffen werden über Modell-Entitäten bereitgestellt.

Modell-Persistenz
-----------------

Eine Anforderung an den Protoypen ist es, neue Modelle erstellen, diese abzuspeichern und wieder laden zu können. 
Wie in :ref:`modellhierarchie` angesprochen werden in I>PM3D Modelle eingesetzt, die in der Sprache LMMLight verfasst werden.
Diese Modelle werden in Dateien in einer textuellen Darstellung abgelegt und daraus wieder geladen.

Für das Laden wird der in :ref:`implementierung-lmmlight` vorgestellte Parser genutzt.

Laden von Metamodellen
^^^^^^^^^^^^^^^^^^^^^^

Wie in :ref:`modellhierarchie` beschrieben wurde, werden Metamodelle für die Spezifikation der verwendeten Modellierungssprache und deren Repräsentation eingesetzt. 
Diese sollen prinzipiell austauschbar sein. Dazu wird von der ModelComponent die Funktion bereitgestellt, Metamodelle zur Laufzeit zu laden.

Dafür wird das Command benutzt, das wie folgt definiert ist:

.. code-block:: scala

    final case class LoadMetaModels(domainModelPath: String, editorModelPath: String, loadAsResource: Boolean) extends Command

LoadAsResource gibt an, ob die Pfade als Java-Resource-Path zu einer Metamodell-Datei interpretiert ("true") oder direkt im Dateisystem gesucht werden sollen ("false").

Es wird zur Vereinfachung der Implementierung davon ausgegangen, dass die Metamodelle der Domäne und des Editors immer paarweise geladen werden. 
Mehrere Repräsentationen zu einer Domäne zu laden ist damit beispielsweise noch nicht möglich.

.. TODO vielleicht mal testen!

Die ModelComponent lässt prinzipiell das Laden von mehreren Metamodell-Paaren zu. Jedoch wird dies von der Editorkomponente noch nicht unterstützt.

.. TODO vielleicht mal testen!

Nachdem Metamodelle geladen werden, werden von der ModelComponent Informationen aus den Modellen ausgelesen, die für die Editorkomponente relevant sind.


Laden von Usage-Modellen und Umgang mit mehreren Modellen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

Bei Knoten und Kanten wird dafür die FQN des entsprechenden Modellelementes aus dem Domänenmodell genutzt. Szenenobjekte werden über die FQN des Editor-Usage-Concepts identifiziert. [#f2]_

Außerdem wird ein Identifikationsstring (modelTypes.CreatorId*) mitgeliefert, der vom Ersteller eines Elements definiert wird. 
Mit "Ersteller" ist hier der Absender des entsprechenden Commands oder die ModelComponent selbst gemeint. 

Diese ID kann von diesem dafür benutzt werden, neu erstellte Entities in internen Datenstrukturen richtig zuzuordnen.

SVars
-----

Über die Zustandsvariablen (SVars) der Modell-Entitäten ist es für Aktoren im System möglich, die Parameter eines Modellobjekts zu verändern.

Die von einer ModelEntity angebotenen SVars lassen sich in drei Gruppen einteilen. 
SVars können direkt Attribute aus den beiden zugrunde liegenden (Meta)-Modellen abbilden oder statisch von der ModelComponent definiert sein.

#. *Domain-Model-SVars* 
   Solche SVars werden zu Attributen erzeugt, die im Domänen-Metamodell definiert sind und denen in Concepts im Usage-Model Werte zugewiesen werden können [#f2]_\ . 
   Sie stellen somit die Schnittstelle dar, über die Modellattribute wie die Funktion eines Prozesses oder der Name eines Konnektors verändert werden können.
   Unterstützt werden alle literalen Datentypen; den SVars werden die passenden Scala-Datentypen zugewiesen.

#. *Editor-Model-SVars* 
   Diese SVars werden nach Bedarf aus den Attributen des Editor-Metamodells erstellt.

   Diese SVars werden aus den im :ref:`emm-metamodell` für einen Typ definierten Attributen erstellt. 
   Welche Editor-Attribute unterstützt werden wird von der ModelComponent festgelegt. Das sind im Einzelnen:

    * Hintergrundfarbe *backgroundColor*
    * Schrift *font*
    * Schriftfarbe *fontColor*
    * Texturpfad *texture*
    * Liniendicke *thickness*
    * Spekulare Farbe *specularColor*

#. Statische definierte *Editor-SVars*: Die sind SVars, die für alle Modellelemente definiert sind.

   Dabei handelt es sich um:

   * SVars für die Auswahl von Visualisierungsvarianten (siehe :ref:`visualisierungsvarianten`): 

     * Deaktivierung (*disabled*), 
     * Hervorhebung (*highlighted*)
     * Selektion (*selected*)

   * Parameter für Visualisierungsvarianten 
     
     * Breite des Selektionsrahmens (*borderWidth*)
     * Hevorhebungsfaktor (*highlightFactor*)
     * Transluzenzfaktor bei deaktivierten Elementen (*deactivatedAlpha*)
   

Alle SVars müssen eindeutig durch einen SVar-Typ beschrieben werden, der ein Symbol zur Identifizierung und einen Scala-Datentyp umfasst. Die Symbole für Editor-SVars beginnen mit 'editor', die Symbole für Domänenmodell-SVars werden mit 'model' gekennzeichnet. Daran wird der Attributname aus dem Modell oder im Falle der statischen Editor-SVars die oben genannten Bezeichner angehängt, abgetrennt durch einen Punkt.V

Beispiele für SVar-Bezeichner: 

#. ``editor.disabled``
#. ``editor.color``
#. ``model.function``



.. _lebenszyklus:

Übersicht über den Lebenszyklus von Model-Entitäten
===================================================



.. [f1] Prinzipiell können dies in der Implementierung auch mehrere Objekte sein, jedoch ist diese vereinfachte Darstellung hier ausreichend.

.. [f2] Dass hier die FQNs aus dem Modell genutzt werden hat keine besondere Bedeutung und ist nur ein "Implementierungsdetail", auf das man sich nicht verlassen solle.

.. [f3] Näheres zur Zuweisbarkeit siehe :ref:`implementierung-lmmlight`.

.. [I] Alle Commands sind in Package mmpe.model.commands definiert.
