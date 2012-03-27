.. _modellanbindung:

***************
Modellanbindung
***************

In diesem Kapitel werden die Anforderungen an die "Modellanbindung" und deren Realisierung besprochen. 
Innerhalb des Prozessmodellierungswerkzeugs stellt die Modellanbindung das Bindeglied zwischen einer Benutzerschnittstelle, wie sie von der Editorkomponente :cite:`uli` realisiert wird und dem zu manipulierenden Modell dar.

Genauer ergeben sich für die Modellanbindung folgende funktionale Anforderungen:

#. Erstellen und Löschen von Modellelementen
#. Bewegen, Rotieren und Skalieren von Modellelementen
#. Verändern von Prozessmodellattributen
#. Modifizieren der Visualisierungsparameter von Modellelementen; beispielsweise Farbe oder Schriftart
#. Laden und Speichern von Prozessmodellen und deren visueller Repräsentation

Modellkomponente
================

Wie in :ref:`simulatorx` erläutert bestehen Simulator-X-Anwendungen aus einer Reihe von Komponenten, die jeweils ein begrenztes Aufgabengebiet abdecken.

Die ModelComponent stellt folglich dem System alle Funktionalitäten zur Verfügung, die im Zusammenhang mit der Manipulation von Modellen stehen. 

So wird der Zugriff auf die Modelle wird vollständig von der ModelComponent gekapselt; es gibt für andere Systembestandteile keine Möglichkeit, direkt darauf zuzugreifen.
Abgesehen von der erhöhten Übersichtlichkeit und Wartbarkeit wird dies auch durch die actor-basierte und damit parallele Architektur von Simulator X vorgegeben.

Commands
--------

Die Funktionen werden zum Teil als "Commands" bereitgestellt, die über das Kommunikationssystem von :ref:`simulatorx` an die ModelComponent geschickt werden.
In der momentanen Umsetzung werden diese Commands ausschließlich durch die Editorkomponente genutzt, die von :cite:`uli` beschrieben wird.

Es existieren Commands für die folgenden Funktionen:

* Laden von Metamodellen
* Laden, Speichern, Erstellen, Schließen und Löschen von Usage-Modellen
* Erstellen und Löschen von Knoten und Szenenobjekten
* Erstellen einer Kante zwischen zwei Knoten

Alle anderen Manipulationsmöglichkeiten, das sind diejenigen, die nur Parameter einzelner Modellelemente betreffen, werden über "ModelEntities" bereitgestellt, welche weiter unten in diesem Kapitel :ref:`model-entities` erläutert werden.

Übersicht 
---------

:num:`Abbbildung #modelcomponent-classdiag` zeigt die "Unterkomponenten" und die "Umgebung" der ModelComponent.

.. _modelcomponent-classdiag:

.. figure:: _static/diags/modelcomponent-classdiag.eps
    :width: 16cm

    Unterkomponenten der ModelComponent (links) und Umgebung (vereinfacht)

Trait Component wird von Simulator X bereitgestellt und muss von allen Komponenten implementiert werden. 
Dessen Methoden beziehen sich überwiegend auf den :ref:`"Lebenszyklus" <lebenszyklus>` von Entities.

Die Implementierung dieser Methoden erfolgt durch das eingemischte Trait **ModelComponentEntityLifecycle**.
Von Trait *ModelComponentHandlers* werden die Funktionen bereitgestellt, die eingehende Nachrichten (in erster Linie sind dies Commands) von anderen Komponenten verarbeiten und diese gegebenenfalls beantworten. Solche Funktionen werden in Simulator X als "Handler" bezeichnet.

Im folgenden Abschnitt wird die Speicherrepräsentation von Modellen sowie das Laden und Speichern beschrieben.
Die Verwaltung der geladenen Modelle wird durch das Object **ModelContext** für die ModelComponent bereitgestellt.

Modell-Persistenz
=================

Eine Anforderung an den Prototypen ist es, neue Modelle erstellen, diese abzuspeichern und wieder laden zu können. 
Wie in :ref:`modellhierarchie` angesprochen werden in I>PM3D Modelle eingesetzt, die in der Sprache LMMLight verfasst werden.

Diese Modelle werden in Dateien in einer textuellen Darstellung abgelegt und daraus wieder geladen.

Für das Laden wird der im Rahmen dieser Arbeit entstandene LMMLight-Parser genutzt, der mit Hilfe der vorher vorgestellten :ref:`parser-kombinatoren` implementiert wurde.
Der Parser liefert einen Syntaxbaum der textuellen Eingabe, der aus "unveränderlichen" (immutable) Objekten aufgebaut ist.

Speicherrepräsentation eines LMMLight-Modells
---------------------------------------------

Um die Modelle in der Anwendung verändern zu können wird der vom Parser gelieferte Syntaxbaum in eine andere Struktur überführt. 
Der so erzeugte Objektgraph ist an die an die EMF-Repräsentation zur Laufzeit angelehnt, wie sie in OMME von XText erzeugt wird.

Vom Graphen wird der hierarchische Aufbau von LMM, wie in :ref:`lmm` gezeigt abgebildet.
Die Elemente von LMM werden durch analog benannte Klassen repräsentiert, die mit dem Buchstaben "M" beginnen.

So wird die "Wurzel" von einer MModel-Instanz gebildet, der sich MLevels unterordnen, die wiederum MPackages mit MConcepts sowie weiteren MPackages enthalten.
Weiterhin kann ein MConcept andere MConcepts referenzieren. So ergibt sich ein azyklischer, gerichteter Graph.

Der Vorteil zur Nutzung von XText ist, dass es sich hier Objekte, die die Vorteile von Scala nutzen und daher in einer Scala-Umgebung bequem genutzt werden können. 
Besonders deutlich wird das bei den von Scala bereitgestellten Collections, die deutlich mehr Funktionalität bieten als die von Java oder EMF bereitgestellten.

Ausgehend von einem *MModel*-Objekt kann die ModelComponent in einem Modell navigieren und dieses modifizieren, beispielsweise neue Concepts anlegen oder Attributzuweisungen vornehmen.


Vereinfachung des Umgangs mit Modellen
--------------------------------------

Um den Zugriff auf die Modelle zu vereinfachen und öfter vorkommende Aufgaben auszulagern wurde eine Reihe von Adaptern für die in der Speicherrepräsentation der Modelle genutzten Klassen implementiert.
Beispielsweise gibt es einen MConceptAdapter, dessen Methoden beispielsweise den schnellen Zugriff auf alle zuweisbaren Attribute (*assignableAttributes*), das Setzen von Werten (*setValue*) oder die Abfrage von Concept-Relationen (*instanceOf*) erlauben.

[#f6]_

Für alle Adapter werden :ref:`implicit` angeboten, die die gekapselten Objekte direkt um die Methoden "erweitern", die in den Adaptern definiert sind.

Laden von Metamodellen
----------------------

Wie in :ref:`modellhierarchie` beschrieben wurde, werden Metamodelle für die Spezifikation der verwendeten Modellierungssprache und deren Repräsentation eingesetzt. 
Diese sollen prinzipiell austauschbar sein. Dazu wird von der ModelComponent die Funktion bereitgestellt, Metamodelle zur Laufzeit zu laden.

Um das Laden der Modelle anzustoßen ist folgendes Command definiert:

.. code-block:: scala

    case class LoadMetaModels(domainModelPath: String, editorModelPath: String, loadAsResource: Boolean) 
        extends Command

LoadAsResource gibt an, ob die Pfade als Java-Resource-Path zu einer Metamodell-Datei interpretiert ("true") oder direkt im Dateisystem gesucht werden sollen ("false").

Es wird zur Vereinfachung der Implementierung davon ausgegangen, dass die Metamodelle der Domäne und des Editors immer paarweise geladen werden. 
Mehrere Repräsentationen zu einer Domäne zu laden ist damit beispielsweise noch nicht möglich.

Die ModelComponent lässt prinzipiell das Laden von mehreren Metamodell-Paaren zu. Jedoch wird dies von der Editorkomponente noch nicht unterstützt.

Nachdem Metamodelle geladen worden sind, werden von der ModelComponent Informationen aus den Modellen ausgelesen, die für die Editorkomponente relevant sind.

Zum Einen ist dies eine Auflistung der verfügbaren Kanten- und Szenenobjekttypen, die vom Benutzer erzeugt werden können und die der Editor zu diesem Zweck in seiner Palette anzeigt.
Zum Anderen wird der Editor über die Knoten-"Metatypen" informiert, von denen nach dem Typ-Verwendungs-Konzept zur Laufzeit Typen vom Benutzer angelegt werden können.

Die Kommunikation zwischen Editor- und Modellkomponente wird in :num:`Abbildung #loadMetamodels-sequencediag` am Laden von Metamodellen beispielhaft gezeigt.


.. _loadMetamodels-sequencediag:

.. figure:: _static/diags/loadMetamodels-sequencediag.eps
    :width: 16cm

    Sequenzdiagramm LoadMetaModels (vereinfacht).
    Nachrichten, die mit Großbuchstaben beginnen stellen Commands beziehungsweise Replies dar; Nachrichten mit Kleinbuchstaben sind gewöhnliche Methodenaufrufe und -rückgabewerte.


Laden und Schließen von Usage-Modellen und Umgang mit mehreren Modellen
-----------------------------------------------------------------------

Usage-Modelle umfassen den aktuellen Zustand eines Prozessmodells und dessen Repräsentation im Editor. 
Ein konkretes "Prozesmodell" wird geöffnet, indem das zugehörige Domain- und Editor-Usage-Model geladen werden.

Das Command *LoadUsageModels* ist analog zum Command *LoadMetaModels* definiert, wie im Abschnitt darüber beschrieben.

Es können von der Anwendung zur Laufzeit mehrere Usage-Modelle (zu denselben Metamodellen) geladen werden. 
In der ModelComponent ist jeweils ein Usage-Model-Paar als "aktiv" gekennzeichnet.
Commands wie das Erstellen von Knoten beziehen sich immer auf das aktive Usage-Model. Welches Modell "aktiv" ist kann über das Command *SetActiveUsageModel* geändert werden.

Modelle können über *CloseUsageModel* wieder geschlossen werden, wobei alle seit dem letzten Speichern erfolgten Änderungen verloren gehen.

Der Umgang mit mehreren Modellen wird auch von der Editorkomponente unterstützt.

Nachdem ein Usage-Modell geladen wurde wird der Aufrufer analog zum Laden von Metamodellen über die im Usage-Modell definierten Knotentypen informiert.

Speichern von Usage-Modellen
----------------------------

"Speichern" bedeutet hier, dass die Änderungen an Modellelementen in das Usage-Model zurückgeschrieben werden und das Modell anschließend in textueller Form persistiert wird.
Analog zum Lade-Command *LoadUsageModels* werden bei *SaveUsageModels* zwei Dateinamen für Domänen- und Editormodell angegeben. Java-Resource-Pfade sind hier nicht erlaubt.

Um die Speicherrepräsentation des Modells wieder in eine textuelle Darstellung zu überführen wird der in :ref:`stringtemplate` gezeigte Wrapper für die StringTemplate-Bibliothek genutzt.
Für die Sprache LMMLight wurde eine Reihe von Templates definiert, die nach dem Setzen der Template-Attribute eine textuelle Darstellung erzeugen, die durch den LMMLight-Parser wieder eingelesen werden kann.\ [#f5]_

.. _model-entities:

Modell-Entitäten
================

Objekte, mit denen verschiedene Teile des Systems interagieren werden in ref:`simulatorx` durch Entities beschrieben. Diese *ModelEntities*

Es ist daher zweckmäßig, für jedes Modellelement, also für Knoten und Verbindungen sowie für Szenenobjekte eine zugehörige Entity zu erstellen.
*ModelEntities* werden von der ModelComponent erzeugt, wenn über ein Command die Erstellung von neuen Elementen angefordert wird oder ein Modell geladen wird. 
Näheres zum Ablauf wird im Abschnitt :ref:`lebenszyklus` dargelegt.

.. _modelentities-aspects:

Aspekte
-------

Wie aus :ref:`simulatorx` bekannt sind Entity-Definitionen aus Aspekten aufgebaut, die einzelnen Komponenten zugeordnet sind. 
Die für ModelEntites genutzten Aspects werden hier aufgeführt.

Physik
^^^^^^

Knoten und Szenenobjekte sollen in die physikalische Simulation aufgenommen werden, um Kollisionen zu erkennen und eine Auswahl der Elemente zu ermöglichen. :cite:`uli` :cite:`buchi`

Hierfür stellt die Physikkomponente verschiedene Aspects bereit, die besagen, dass eine bestimmte Geometrie für die entsprechende Entity genutzt werden soll.
Da bisher nur annähernd quaderförmige Geometrien für die Visualisierung von Knoten genutzt werden, wird hier für alle Knoten der *PhysBox*-Aspect verwendet.

Kanten definieren keinen Physik-Aspect und besitzen daher keine physikalische Darstellung.\ [#f7]_

Grafik
^^^^^^

Die :ref:`Renderkomponente` stellt verschiedene RenderAspects bereit, die der Renderkomponente alle nötigen Informationen mitteilen, um ein Visualisierungsobjekt zur entsprechenden Entity anzulegen.

Szenenobjekte, für die es bisher nur die Möglichkeit gibt, diese aus COLLADA-3D-Modelldateien zu laden werden von der Renderkomponente selbst erzeugt. 
Solche Szenenobjekte sind statisch durch das 3D-Modell definiert. 
Das bedeutet in diesem Zusammenhang, dass ihr Erscheinungsbild zur Laufzeit nicht geändert werden kann, abgesehen von Position, Rotation und Skalierung.
In der Entity-Beschreibung wird dafür der *ShapeFromFile*-Aspect angegeben.

Für Knoten und Kanten wird dagegen der *ShapeFromFactory*-Aspect genutzt, der besagt, dass sich die Renderkomponente das Grafikobjekt von einer externen Factory erzeugen lassen soll.
Für ModelEntities steht die *ModelDrawableFactory* zur Verfügung, welche später in einem :ref:`Anwendungsbeispiel <beispiel-neue-modellfigur>` modifiziert wird, um ein Grafikobjekt für einen neuen Knotentyp hinzuzufügen.

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

.. _model-svars-transformation:

Setzen von Position, Ausrichtung und Größe eines Objekts
--------------------------------------------------------

(Dieser Unterabschnitt beschreibt von Simulator X vorgegebene Funktionalität. Projektspezifische Anpassungen sind in Fußnoten angegeben.)

Position und Ausrichtung sind – wie in der Computergrafik üblich – zu einer Transformations-Matrix zusammengefasst. 
Die Skalierung eines Objekts wird durch einen Vektor (3 Komponenten) angegeben.\ [#f8]_
Beide Werte werden für Knoten und Szenenobjekte von der Physikkomponente verwaltet.

Sie können von anderen Komponenten verändert werden, indem eine Nachricht an die Physikkomponente geschickt wird:

.. code-block:: scala
    
    physics ! SetTransformation(newTransformationMatrix)
    physics ! SetScale(newScaleVector)

Von der Physikkomponente werden außerdem zwei SVars zur Entity hinzugefügt (Typen ScaleVec und Transformation), die allerdings nur lesend genutzt werden dürfen.

Beispielsweise kann so die aktuelle Transformation ausgegeben werden\ [#f9]_:

.. code-block:: scala

    processEntity.svarGet(Transformation) { 
        value => println("current transformation of processEntity: " + value) 
    }

Dabei ist zu beachten, dass der Aufruf der in geschweiften Klammern angegebenen, anonymen Funktion asynchron erfolgt wie in :ref:`simulatorx` beschrieben wurde.

Für Objekte ohne Physik-Aspekt (Kanten) werden die genannten SVars durch die Renderkomponente bereitgestellt. 
Diese leisten dasselbe, dürfen aber auch verändert werden:

.. code-block:: scala

    processEntity.svarSet(Transformation)(newTransformationMatrix) 


.. _modellanbindung-svars:

Modell-SVars
------------

Über die Zustandsvariablen (SVars) der Modell-Entitäten ist es für Aktoren im System möglich, die Parameter eines Modellobjekts zu verändern.

Die von einer ModelEntity angebotenen SVars lassen sich in drei Gruppen einteilen. 
SVars können direkt Attribute aus den beiden zugrunde liegenden (Meta)-Modellen abbilden oder von der ModelComponent definiert sein.

#. **Domain-Model-SVars** 
   Solche SVars werden zu Attributen erzeugt, die im Domänen-Metamodell definiert sind und denen in Concepts im Usage-Model Werte zugewiesen werden können [#f3]_\ . 
   Sie stellen somit die Schnittstelle dar, über die Modellattribute wie die Funktion eines Prozesses oder der Name eines Konnektors verändert werden können.
   Unterstützt werden alle literalen Datentypen; den SVars werden die passenden Scala-Datentypen zugewiesen.

#. **Editor-Model-SVars**
   Diese SVars werden nach Bedarf aus den Attributen des Editor-Metamodells erstellt. 
   Sie erlauben es, die Visualisierung der Elemente anzupassen, wie sie im Editormodell beschrieben wird.\ [#f4]_
   Neben literalen Attributen werden hier auch Concept-Attribute unterstützt. Diese werden für die meisten hier genannten SVars benötigt.

   Welche Editor-Attribute unterstützt werden wird von der ModelComponent festgelegt.\ [#f5]_ 
   
   Das sind im Einzelnen:

    * Hintergrundfarbe (*backgroundColor*)
    * Schrift (*font*)
    * Schriftfarbe (*fontColor*)
    * Texturpfad (*texture*)
    * Liniendicke (*thickness*)
    * Spekulare Farbe (*specularColor*)

#. **Editor-SVars**
   Dies sind SVars, die keine direkte Entsprechung im Modell haben und deren Werte daher auch nicht persistiert werden. 
   Sie sind automatisch für alle Modellelemente definiert oder werden durch Modellattribute "aktiviert". 
   Dabei handelt es sich um:

   * SVars für die Auswahl von :ref:`Visualisierungsvarianten <visualisierungsvarianten>`: 

     * Deaktivierung (*disabled*), 
     * Hervorhebung (*highlighted*)
     * Selektion (*selected*)

   * Parameter für die Visualisierungsvarianten 
     
     * Breite des Selektionsrahmens (*borderWidth*)
     * Hevorhebungsfaktor (*highlightFactor*)
     * Transluzenzfaktor bei deaktivierten Elementen (*deactivatedAlpha*)
    
   Alle hier genannten SVars werden von der ModelComponent aktiviert, wenn im Modell das Attribut *interactionAllowed* auf "true" gesetzt ist.
   

Alle SVars müssen eindeutig durch eine *SVarDescription* beschrieben werden, der ein Symbol zur Identifizierung und einen Scala-Datentyp umfasst. 
Die Symbole für Editor-SVars beginnen mit 'editor', die Symbole für Domänenmodell-SVars werden mit 'model' gekennzeichnet. 
Daran wird der Attributname aus dem Modell oder im Falle der Editor-SVars einer der unter 3. genannten Bezeichner angehängt, abgetrennt durch einen Punkt.

Anwendungsbeispiel 
^^^^^^^^^^^^^^^^^^

Die Nutzung erfolgt analog zu statisch definierten SVars, wie den in :ref:`model-svars-transformation` genannten.

Im folgenden Beispiel wird die Funktion eines Prozessknotens und die Schriftfarbe über die zugehörige Entity verändert:

.. code-block:: scala
    
    processEntity.svarSet("model.function")("Ausarbeitung schreiben")
    processEntity.svarSet("editor.fontColor")(Color.BLACK)

.. _lebenszyklus:

Übersicht über den Lebenszyklus von Model-Entitäten
===================================================

Dieser Abschnitt zeigt kurz, welche wichtigen Schritte im "Lebenszyklus" einer ModelEntity durchlaufen werden.

Komponenten in Simulator X - Framework definieren eine Reihe von Methoden, die vom Framework beim Erstellen oder Löschen einer Entity aufgerufen werden.

Die Erstellung einer ModelEntity folgt dem folgenden Schema:

  #. Die Erstellung wird beispielsweise durch ein CreateNode-Command vom Editor angestoßen. Die ModelComponent erzeugt daraufhin eine *EntityDescription* mit den :ref:`modelentities-aspects` und übergibt diese an das Framework (Methode EntityDescription.realize()), welches die Erstellung der Entity verwaltet und die folgenden Methoden aufruft.

  #. Die Methode *getAdditionalProvidings* gibt eine Sequenz von *SVarDescriptions* zurück, die zu der Entity hinzugefügt werden sollen. Im Falle der ModelComponent sind dies *SVarDescriptions* zu den im vorherigen Abschnitt beschriebenen SVars.

  #. Anschließend wird die Methode *getInitialValues* aufgerufen, welche Initialwerte für die definierten SVars zurückgeben soll. Die ModelComponent liest hierzu die Attributzuweisungen aus den Modell-Concepts aus oder setzt Standardwerte.

  #. Nach Fertigstellung einer Entity wird *newEntityConfigComplete* aufgerufen. Die ModelComponent fügt die Entity zu ihrer internen Repräsentation hinzu und verbindet die Domain-Model-SVars mit den Attributen im Modell. Dies heißt, dass auf der SVar eine Observe-Funktion registriert wird, die bei jeder Änderung des SVar-Wertes auch den Wert im dahinterliegenden Domain-Concept ändert.\ [#f11]_

Der genannte Prozesse läuft auch parallel für die anderen Komponenten ab, für die Aspects in der Entity definiert sind; hier also für die Render- und gegebenenfalls die Physikkomponente.

Beim Löschen spielt sich Folgendes ab:

  #. Das Löschen wird beispielsweise durch ein DeleteNode(fqnToDelete)-Command vom Editor initiiert. Daraufhin startet die ModelComponent den Löschvorgang, indem auf der zur FQN gehörigen Entity die Methode remove() aufgerufen wird.

  #. Simulator X entfernt nun die Entity aus dem System und ruft dabei in der Komponente die *removeFromLocalRep*-Methode auf. In dieser Methode sollen interne Verweise und zugehörige Daten in den Komponenten entfernt werden.


.. [#f2] Dass hier die FQNs aus dem Modell genutzt werden hat keine besondere Bedeutung und ist nur ein "Implementierungsdetail", auf das man sich nicht verlassen solle.

.. [#f3] Die Regeln für die Zuweisbarkeit ...

.. [#f4] Es wäre auch erlaubt, Attribute zu integrieren, die nicht direkt die Visualisierung betreffen, aber das Editor-Verhalten modifizieren. Dies wird bisher aber nicht genutzt.

.. [#f5] Es war nicht möglich, die Implementierung (auf einfachem Wege) so flexibel zu gestalten wie bei Domain-Model-SVars, was leider dazu führt, dass man keine Attribute hinzufügen kann ohne die ModelComponent anzupassen.

.. [#f6] Gewisse Ähnlichkeiten mit anderen Projekten sind rein zufällig ;-)

.. [#f7] Dies ist nicht nötig, da die Auswahl von Kanten nicht unterstützt werden soll und Kollisionen mit Verbindungen eher als hinderlich gesehen wurde. Außerdem könnte eine große Anzahl von Verbindungen schnell zu Geschwindigkeitsproblemen der Simulation führen.

.. [#f8] Skalierung wurde für das Projekt hinzugefügt. Dazu wurde die Physikkomponente modifiziert und die selbstgeschriebene Renderkomponente entsprechend ausgelegt.

.. [#f9] Die svarGet-Methode (ebenfalls svarSet und weitere) wurde für das Projekt in einem impliziten Wrapper für Entities definiert um den Zugriff auf SVars zu "verschönern".

.. [#f10] Die Darstellung ist aber auch durchaus "menschenlesbar" und wird ähnlich formatiert wie im Metamodell-Editor von OMME.

.. [#f11] Bei den Editor-Model-SVars wird ein anderer Ansatz genutzt, da diese teilweise häufig geändert werden (vor allem die Position). Diese SVars werden erst beim Speichern des Modells ausgelesen und zurückgeschrieben um Probleme mit der Ausführungsgeschwindigkeit zu vermeiden.
