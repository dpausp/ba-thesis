.. _ref_konzept_modellanbindung:

***********************
Konzept Modellanbindung
***********************

Nachdem im vorherigen Kapitel beschrieben wurde, wie Prozessmodelle visualisiert werden können und welche grafischen Objekte dafür genutzt werden, soll im Folgenden das Konzept zur Anbindung an das Gesamtsystem vorgestellt werden.

Da das Ziel des Projekts I>PM3D ist, einen interaktiven Prozessmodelleditor zu erstellen ergeben sich die folgenden funktionalen Anforderungen:

#. Erstellen und Löschen von Modellelementen
#. Bewegen, Rotieren und Skalieren von Modellelementen
#. Verändern von Prozessmodellattributen
#. Modifizieren der Visualisierungsparameter von Modellelementen; beispielsweise Farbe oder Schriftart
#. Laden und Speichern von Prozessmodellen und deren visuelle Repräsentation

Da das Projekt auf der Grundlage von Simulator X entwickelt wurde folgt das hier vorgestellte Konzept der Architektur von Simulator-X-Anwendungen.

ModelComponent
==============

Wie in :ref:`simulator_x` erläutert bestehen Simulator-X-Anwendungen aus einer Reihe von Komponenten, die anderen Komponenten eine bestimmte Funktionalität bereitstellen. 

Die ModelComponent stellt folglich dem System alle Funktionen zur Verfügung, die im Zusammenhang mit der Manipulation von Modellen stehen. Diese Funktionen werden als sogenannte *Commands* bereitgestellt. 

Es existieren Commands für die folgenden Funktionalitäten:

* Laden von Metamodellen
* Laden, Speichern, Erstellen, Schließen und Löschen von Usage-Modellen
* Erstellen und Löschen von Knoten und Szenenobjekten
* Verbinden und Trennen von Knoten

Alle anderen Manipulationsmöglichkeiten, also diejenigen, die nur Parameter von einzelnen Modellelementen betreffen werden über Modell-Entitäten bereitgestellt.

Modell-Entitäten
================

Objekte in Simulator X werden durch Entities beschrieben. Es ist daher zweckmäßig, für jedes Modellelement, also Knoten und Verbindungen sowie für Szenenobjekte eine passende Entity zu erstellen.

Über die Zustandsvariablen dieser Modell-Entitäten ist es für Aktoren im System möglich, die Parameter eines Modellobjekts zu verändern.

Die von einer ModelEntity angebotenen SVars lassen sich in 3 Gruppen einteilen

#. Statische definierte Editor-SVars: Die sind SVars, die für alle Modellelemente definiert sind.

   Dabei handelt es sich um:

   * SVars für die Auswahl von Visualisierungsvarianten (siehe :ref:`visualisierungsvarianten`): 

     * Deaktivierung (*disabled*), 
     * Hervorhebung (*highlighted*)
     * Selektion (*selected*)

   * Parameter für Visualisierungsvarianten 
     
     * Breite des Selektionsrahmens (*borderWidth*)
     * Hevorhebungsfaktor (*highlightFactor*)
     * Transluzenzfaktor bei deaktivierten Elementen (*deactivatedAlpha*)
   
#. Dynamisch durch das Editor-Metamodell definierte Editor-SVars, die das Erscheinungsbild bestimmen: diese SVars werden aus den im Editor-Metamodell für einen Typ definierten Attributen erstellt. 
   Welche Editor-Attribute unterstützt werden wird von der ModelComponent festgelegt. Das sind im Einzelnen:

    * Hintergrundfarbe *backgroundColor*
    * Schrift *font*
    * Schriftfarbe *fontColor*
    * Texturpfad *texture*
    * Liniendicke *thickness*
    * Spekulare Farbe *specularColor*

#. Dynamisch durch das Prozess-Metamodell definierte Modell-SVars: analog zu Editor-SVars handelt es sich hier um die (literalen) Attribute aus dem Domänen-Metamodell. Es werden alle Attribute angeboten, die laut LMM-Semantik im zugehörigen Konzept *assignable* sind. Um die Implementierung zu vereinfachen werden bisher jedoch nur literale Attribute angeboten.

Alle SVars müssen eindeutig durch einen SVar-Typ beschrieben werden, der ein Symbol zur Identifizierung und einen Scala-Datentyp umfasst. Die Symbole für Editor-SVars beginnen mit 'editor', die Symbole für Domänenmodell-SVars werden mit 'model' gekennzeichnet. Daran wird der Attributname aus dem Modell oder im Falle der statischen Editor-SVars die oben genannten Bezeichner angehängt, abgetrennt durch einen Punkt.

Beispiele:

#. ``editor.disabled``
#. ``editor.color``
#. ``model.function``




