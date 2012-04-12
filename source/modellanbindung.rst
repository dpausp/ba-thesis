.. _modellanbindung:

***************
Modellanbindung
***************

Innerhalb des Prozessmodellierungswerkzeugs stellt die Modellanbindung (:num:`Abbildung #ipm3d-uebersicht-modellanbindung`) das Bindeglied zwischen einer Benutzerschnittstelle, wie sie von der Editorkomponente :cite:`uli` realisiert wird und dem zu manipulierenden Modell dar (:ref:`Anforderungen (e,f) <anforderungen>`).

Genauer ergeben sich für die Modellanbindung folgende funktionale Anforderungen:

#. Erstellen und Löschen von Modellelementen
#. Bewegen, Rotieren und Skalieren von Modellelementen
#. Verändern von Prozessmodellattributen
#. Modifizieren der Visualisierungsparameter von Modellelementen; beispielsweise Farbe oder Schriftart
#. Erstellen, Laden und Speichern von Prozessmodellen und deren visueller Repräsentation

.. _ipm3d-uebersicht-modellanbindung:

.. figure:: _static/diags/ipm3d-uebersicht-modellanbindung.eps
    :height: 9cm

    Die Modellanbindung im Kontext von i>PM3D


.. _modellkomponente:

Modellkomponente
================

Wie in :ref:`simulatorx` erläutert, bestehen Simulator-X-Anwendungen aus einer Reihe von Komponenten, die jeweils ein wohl definiertes Aufgabengebiet abdecken.
Die ``ModelComponent`` stellt folglich dem System alle Funktionalitäten zur Verfügung, die im Zusammenhang mit der Manipulation von Modellen stehen (*Anforderung (e)*). 

So wird der Zugriff auf die Modelle vollständig von der ``ModelComponent`` gekapselt; es gibt für andere Systembestandteile keine Möglichkeit, direkt darauf zuzugreifen.
Abgesehen von der erhöhten Übersichtlichkeit und Wartbarkeit wird dies auch durch die Actor-basierte Architektur und damit nachrichtenbasierte Kommunikation ("message passing") von Simulator X vorgegeben.

Commands
--------

Die Modellfunktionen werden zum Teil als ``Commands`` bereitgestellt, die über das Kommunikationssystem von :ref:`simulatorx` an die ``ModelComponent`` geschickt werden.
In der momentanen Umsetzung werden diese ``Commands`` ausschließlich durch die Editorkomponente genutzt, die von :cite:`uli` beschrieben wird.

Es existieren ``Commands`` für die folgenden Funktionen:

* Laden von Metamodellen
* Laden, Speichern, Erstellen, Schließen und Löschen von (Usage-)Modellen
* Erstellen und Löschen von Knoten und Szenenobjekten
* Erstellen einer Kante zwischen zwei Knoten

Alle anderen Manipulationsmöglichkeiten — das sind diejenigen, die nur Parameter einzelner Modellelemente betreffen – werden über ``ModelEntities`` bereitgestellt, welche weiter unten in diesem Kapitel :ref:`\ <model-entities>` erläutert werden.

Übersicht 
---------

Da die Funktionalität der ``ModelComponent`` relativ umfangreich ist, ist diese wiederum in die in  :num:`Abbildung #modelcomponent-classdiag` gezeigten "Unterkomponenten" aufgeteilt.

.. _modelcomponent-classdiag:

.. figure:: _static/diags/modelcomponent-classdiag.eps
    :width: 16cm

    Unterkomponenten der ``ModelComponent`` (vereinfacht)

Das Trait ``Component`` wird von Simulator X bereitgestellt und muss von allen Komponenten implementiert werden. 
Dessen Methoden beziehen sich überwiegend auf den :ref:`"Lebenszyklus" <lebenszyklus>` von ``Entities``.
Die Implementierung dieser Methoden erfolgt durch das eingemischte Trait ``ModelComponentEntityLifecycle``.

Von Trait ``ModelComponentHandlers`` werden die Funktionen bereitgestellt, die eingehende Nachrichten (vor allem ``Commands``) von anderen Komponenten verarbeiten und diese gegebenenfalls beantworten. 
Solche Funktionen werden in Simulator X als "Handler" bezeichnet.

Das Laden (mittels "LMMLight-Parser") und Speichern ("ModelToText") sowie die Verwaltung der geladenen (Meta-)Modelle wird der ``ModelComponent`` durch das Object ``ModelContext`` bereitgestellt.

Modell-Persistenz
=================

Das Prozessmodellierungswerkzeug muss in der Lage sein, neue Modelle zu erstellen, diese abzuspeichern und wieder zu laden (*Anforderung (f)*).
Die Modelle werden in der Sprache :ref:`lmmlight` beschrieben, welche in Dateien in einer textuellen Darstellung abgelegt und daraus wieder geladen werden kann.

Für das Laden wird der im Rahmen dieser Arbeit entstandene **LMMLight-Parser** genutzt, der mit Hilfe der Scala-:ref:`parser-kombinatoren` implementiert wurde.
Der Parser liefert einen Syntaxbaum der textuellen Eingabe, der aus "unveränderlichen" (immutable) Objekten aufgebaut ist.

Speicherrepräsentation eines LMMLight-Modells
---------------------------------------------

Um die Modelle in der Anwendung verändern zu können, wird der vom Parser gelieferte Syntaxbaum in eine andere Struktur überführt. 
Der so erzeugte Objektgraph ist an die an die EMF :cite:`www:emf`-Repräsentation zur Laufzeit angelehnt, wie sie in OMME von XText :cite:`www:xtext` erzeugt wird.

Vom Graphen wird der hierarchische Aufbau von LMM, wie in :ref:`lmm` gezeigt abgebildet.
Die Elemente von LMM werden durch analog benannte Klassen repräsentiert, die mit dem Buchstaben "M" beginnen.

So wird die "Wurzel" von einer ``MModel``-Instanz gebildet, der sich ``MLevels`` unterordnen, die wiederum ``MPackages`` mit ``MConcepts`` sowie weiteren ``MPackages`` enthalten.
Weiterhin kann ein ``MConcept`` andere ``MConcepts`` referenzieren. So ergibt sich ein azyklischer, gerichteter Graph.
Ausgehend von einem ``MModel``-Objekt kann die ``ModelComponent`` in einem Modell navigieren und es modifizieren, beispielsweise neue Concepts anlegen.

:num:`Abbildung #domain-model-beispiel` zeigt beispielhaft einen Ausschnitt aus der Speicherrepräsentation des :ref:`Domain-Model-Stacks<domain-model-stack>`.
Im Beispiel ist das Concept ``ProcessUsage`` eine Verwendung von ``ProcessA``. 
Mit ``ProcessUsage`` ist daher eine ``MConceptReference`` assoziiert, welche die Spezialisierungsrelation zwischen den beiden Concepts repräsentiert.
``ProcessUsage`` hat außerdem eine ausgehende Kante zu einer anderen (nicht gezeigten) Verwendung. 
Ausgedrückt wird dies durch die Zuweisung vom Typ ``MConceptAssignment``, welche wiederum die zugehörige Kante ``ControlFlowA`` referenziert. 
Die Zuweisung gehört zu einem Attribut mit dem Namen "outboundControlFlows", das im Concept ``Node`` aus dem :ref:`pmm` definiert ist.

.. _domain-model-beispiel:

.. figure:: _static/diags/domain-model-beispiel.eps
    :width: 18cm

    Speicherrepräsentation eines Beispiel-Prozessmodells (Ausschnitt)

Vereinfachung des Umgangs mit Modellen
--------------------------------------

Um den Zugriff auf die Modelle zu vereinfachen und öfter vorkommende Aufgaben auszulagern, wurde eine Reihe von Adaptern für die in der Speicherrepräsentation der Modelle genutzten Klassen implementiert.
Ein Beispiel dafür ist der ``MConceptAdapter``, dessen Methoden beispielsweise den schnellen Zugriff auf alle zuweisbaren Attribute (``assignableAttributes``), das Setzen von Werten (``setValue``) oder die Abfrage von Concept-Beziehungen (``instanceOf``) erlauben.
Für alle Adapter werden :ref:`implicit` angeboten, die die gekapselten Objekte direkt um die Methoden "erweitern", die in den Adaptern definiert sind.

.. _laden-metamodelle:

Laden von Metamodellen
----------------------

Wie in :ref:`modellhierarchie` beschrieben wurde, werden Metamodelle für die Spezifikation der verwendeten Modellierungssprache und deren Repräsentation eingesetzt. 
Diese sollen prinzipiell austauschbar sein. Dazu wird von der Modellkomponente die Funktion bereitgestellt, Metamodelle zur Laufzeit zu laden.

Um das Laden der Modelle anzustoßen, ist folgendes ``Command`` definiert:

.. code-block:: scala

    case class LoadMetaModels(domainModelPath: String, editorModelPath: String, loadAsResource: Boolean) 
        extends Command

Von ``loadAsResource`` wird angegeben, ob die Pfade als Java-Resource-Path zu einer Metamodell-Datei interpretiert ("true") oder direkt im Dateisystem gesucht werden sollen ("false").

Es wird zur Vereinfachung der Implementierung davon ausgegangen, dass die Metamodelle der Domäne und des Editors immer paarweise geladen werden. 
Mehrere Repräsentationen zu einer Domäne zu laden ist somit noch nicht möglich.
Die Modellkomponente lässt prinzipiell das Laden von mehreren Metamodell-Paaren zu. Jedoch wird dies von der Editorkomponente :cite:`uli` noch nicht unterstützt.

Nachdem die Metamodelle geladen worden sind, werden von der Modellkomponente Informationen aus den Modellen ausgelesen, die für die Editorkomponente relevant sind.
Zum Einen ist dies eine Auflistung der verfügbaren Kanten- und Szenenobjekttypen, die vom Benutzer erzeugt werden können und die der Editor zu diesem Zweck in seiner Palette anzeigt.
Zum Anderen wird der Editor über die Knoten-"Metatypen" informiert, von denen nach dem Typ-Verwendungs-Konzept zur Laufzeit Typen vom Benutzer angelegt werden.

Die Kommunikation zwischen Editor- und Modellkomponente wird in :num:`Abbildung #load-metamodels-sequencediag` am Laden von Metamodellen beispielhaft gezeigt.
Nachrichten, die mit Großbuchstaben beginnen stellen ``Commands`` beziehungsweise Replies dar; Nachrichten mit Kleinbuchstaben sind gewöhnliche Methodenaufrufe und -rückgabewerte.


.. _load-metamodels-sequencediag:

.. figure:: _static/diags/loadMetamodels-sequencediag.eps
    :width: 16cm

    Sequenzdiagramm LoadMetaModels (vereinfacht).


Laden und Schließen von und Umgang mit mehreren Modellen
--------------------------------------------------------

Ein konkretes "Prozesmodell" wird geöffnet, indem das zugehörige *Domain-Model* und *Editor-Usage-Model* geladen werden.
Zusammen werden diese beiden Modelle im Folgenden vereinfachend das *Usage-Modell* genannt, welches den aktuellen Zustand eines Prozessmodells und dessen Repräsentation im Editor umfasst\ [#f1]_.

Das ``Command`` ``LoadUsageModels`` ist analog zum ``Command`` ``LoadMetaModels`` definiert, wie im vorherigen Unterabschnitt beschrieben.

Es können von der Anwendung zur Laufzeit mehrere Usage-Modelle (zu denselben Metamodellen) geladen werden. 
In der ``ModelComponent`` ist jeweils ein Usage-Model-Paar als "aktiv" gekennzeichnet.
Commands wie das Erstellen von Knoten beziehen sich immer auf das aktive Usage-Model. Welches Modell "aktiv" ist kann über das ``Command`` ``SetActiveUsageModel`` geändert werden.

Modelle können über ``CloseUsageModel`` wieder geschlossen werden, wobei alle seit dem letzten Speichern erfolgten Änderungen verloren gehen.

Der Umgang mit mehreren Modellen wird auch von der Editorkomponente unterstützt.

Nachdem ein Usage-Model geladen wurde, wird der Aufrufer analog zum Laden der Metamodelle über die im *Domain-Model* definierten Knotentypen informiert.

Speichern von Modellen
----------------------

"Speichern" bedeutet hier, dass die Änderungen an Modellelementen in das Usage-Model zurückgeschrieben werden und das dieses anschließend in textueller Form persistiert wird.
Analog zu ``LoadUsageModels`` werden bei ``SaveUsageModels`` zwei Dateinamen für Domain- und Editormodell angegeben. Java-Resource-Pfade sind hier nicht erlaubt.

Um die Speicherrepräsentation des Modells wieder in eine durch den LMMLight-Parser lesbare\ [#f10]_, textuelle Darstellung zu überführen, wird der in :ref:`stringtemplate` vorgestellte Wrapper für die StringTemplate-Bibliothek genutzt.


.. _model-entities:

Modell-Entitäten
================

Objekte, mit denen verschiedene Teile des Systems interagieren, werden in :ref:`simulatorx` durch Entities beschrieben. 
Es ist daher zweckmäßig, für jedes Modellelement sowie für Szenenobjekte eine zugehörige Entity zu erstellen.
``ModelEntities`` werden von der ``ModelComponent`` erzeugt, wenn über ein Command die Erstellung von neuen Elementen angefordert oder ein Modell geladen wird. 

Wie aus :ref:`simulatorx` bekannt werden Entities mit Hilfe von ``EntityDescriptions`` beschrieben, die aus ``Aspects`` aufgebaut sind.
In :num:`Abbildung #entity-description` ist ein eine solche ``Entity``-Definition zu sehen.
Der Ablauf bei der Erstellung einer ``ModelEntity`` aus einer ``EntityDescription`` wird im Abschnitt :ref:`lebenszyklus` vorgestellt.

.. _entity-description:

.. figure:: _static/diags/entity_description.eps
    :width: 16.5cm

    ``EntityDescription`` für einen Knoten (nur ausgewählte und vereinfachte Attribute)

.. _modelentities-aspects:

Aspekte
-------

Die zur Erstellung von ``ModelEntities`` genutzten ``Aspects`` werden im Folgenden beschrieben.

Physik
^^^^^^

Knoten und Szenenobjekte sollen in die physikalische Simulation eingebunden werden, um Kollisionen zu erkennen und eine Auswahl der Elemente zu ermöglichen :cite:`uli` :cite:`buchi`.
Hierfür stellt die Physikkomponente verschiedene ``Aspects`` bereit, die besagen, dass eine bestimmte physikalische Repräsentation zu einer Entity erzeugt werden soll.
Da bisher nur annähernd quaderförmige Geometrien für die Visualisierung von Knoten genutzt werden, wird hier für alle Knoten der ``PhysBox``-Aspect (:num:`Abbildung #entity-description`) verwendet.

Kanten definieren keinen Physik-Aspect und besitzen daher keine physikalische Repräsentation\ [#f7]_.

Grafik
^^^^^^

Die :ref:`Renderkomponente` stellt verschiedene ``RenderAspects`` bereit, die der Renderkomponente alle nötigen Informationen mitteilen, um ein Visualisierungsobjekt zur entsprechenden Entity anzulegen.

Szenenobjekte, welche aus COLLADA-3D-Modelldateien geladen werden, werden von der Renderkomponente selbst erzeugt. 
Solche Szenenobjekte sind statisch durch das 3D-Modell definiert. 
Das bedeutet in diesem Zusammenhang, dass ihr Erscheinungsbild zur Laufzeit nicht geändert werden kann (abgesehen von Position, Rotation und Skalierung).
In der Entity-Beschreibung wird dafür der ``ShapeFromFile``-Aspect angegeben.

Für Knoten und Kanten wird dagegen der ``ShapeFromFactory``-Aspect genutzt, der besagt, dass sich die :ref:`renderkomponente` das Grafikobjekt von einer externen Factory erzeugen lassen soll.
In :num:`Abbildung #entity-description` ist zu sehen, dass im ``Aspect`` die ``ModelDrawableFactory`` angegeben wird, welche alle Grafikobjekte für Knoten und Kanten erzeugt.
Parameter ``creationData`` gibt den Typ des gewünschten Objekts an, der in den Figuren im :ref:`Editor-Metamodell<ebl-figures>` spezifiziert wurde. 
Die ``ModellDrawableFactory`` wird später in einem :ref:`Anwendungsbeispiel <beispiel-neue-modellfigur>` modifiziert, um ein Grafikobjekt für einen neuen Knotentyp hinzuzufügen.

Modell
^^^^^^

Für die drei Elementtypen Knoten, Kanten und Szenenobjekte gibt es jeweils einen Aspect, der von ``ModelAspect`` abgeleitet ist, wie beispielsweise den ``NodeAspect``, wie er in :num:`Abbildung #entity-description` zu sehen ist.
``ModelAspects`` sind der ``ModelComponent`` zugeordnet und enthalten für Nutzer der ``ModelEntity`` relevante Informationen. 

Für alle Elemente, die von ``ModelEntities`` repräsentiert werden wird ein voll qualifizierter Name (``fqn``) vergeben, der das Element eindeutig innerhalb des Systems identifiziert.
Dieser Name wird in ``Commands`` verwendet, die sich auf bestimmte Elemente beziehen, wie beispielsweise das Verbinden oder Löschen von Knoten.
Bei Knoten und Kanten wird dafür die FQN des entsprechenden Modellelementes aus dem *Domain-Model* genutzt. Szenenobjekte werden über die FQN des *Editor-Usage*-Concepts identifiziert\ [#f2]_.

Außerdem wird ein Identifikationsstring (``creatorId``) mitgeliefert, der vom Ersteller eines Elements definiert wird. 
Mit "Ersteller" ist hier der Absender des entsprechenden ``Commands`` oder die ``ModelComponent`` selbst gemeint. 
Diese ID kann von diesem dafür benutzt werden, neu erstellte Entities in internen Datenstrukturen richtig zuzuordnen.


.. _model-svars-transformation:

Setzen und Auslesen von Position, Ausrichtung und Größe
-------------------------------------------------------

(Dieser Unterabschnitt beschreibt von Simulator X vorgegebene Funktionalität. Projektspezifische Anpassungen sind mit Fußnoten versehen.)

Position und Ausrichtung sind – wie in der Computergrafik üblich :cite:`akenine-moller_real-time_2008` – zu einer Transformations-Matrix zusammengefasst. 
Die Skalierung eines Objekts wird durch einen Vektor (mit drei Komponenten) angegeben\ [#f8]_.
Beide Werte werden für Knoten und Szenenobjekte von der Physikkomponente verwaltet.

Sie können von anderen Komponenten verändert werden, indem eine Nachricht an die Physikkomponente geschickt wird:

.. code-block:: scala
    
    physics ! SetTransformation(newTransformationMatrix)
    physics ! SetScale(newScaleVector)

Von der Physikkomponente werden außerdem zwei SVars zur Entity hinzugefügt (Typen ``ScaleVec`` und ``Transformation``), die allerdings nur lesend genutzt werden dürfen.

Beispielsweise kann so die aktuelle Transformation ausgegeben werden\ [#f9]_:

.. code-block:: scala

    processEntity.svarGet(Transformation) { 
        value => println("current transformation of processEntity: " + value) 
    }

Dabei ist zu beachten, dass der Get-Aufruf "nicht-blockierend" erfolgt.
Wie in :ref:`simulatorx` beschrieben wurde, muss der Wert einer SVar eventuell von einem anderen Actor über das Kommunikationssystem angefragt werden. 
Die anonyme Funktion (im Code-Beispiel in geschweiften Klammern) wird ausgeführt, sobald die Antwort vorliegt.

Für Objekte ohne Physik-Aspekt (Kanten) werden die genannten SVars durch die Renderkomponente bereitgestellt. 
Diese leisten dasselbe, dürfen aber auch verändert werden:

.. code-block:: scala

    processEntity.svarSet(Transformation)(newTransformationMatrix) 


.. _modellanbindung-svars:

Modell-SVars
------------

Weitere Parameter der Modellobjekte lassen sich ebenfalls über ``SVars`` auslesen und setzen.

Diese ``SVars`` lassen sich in drei Gruppen einteilen. 
SVars können direkt Attribute aus den beiden zugrunde liegenden (Meta)-Modellen abbilden oder von der Modellkomponente definiert sein:

#. **Domain-Model-SVars**: 
   Solche SVars werden zu Attributen erzeugt, die im *Domain-Meta-Model* definiert sind und denen in Concepts im *Domain-Model* Werte zugewiesen werden können.
   Sie stellen somit die Schnittstelle dar, über die Modellattribute wie die Funktion eines Prozesses oder der Name eines Konnektors verändert werden können.
   Unterstützt werden alle literalen Datentypen; den SVars werden die passenden Scala-Datentypen zugewiesen.

#. **Editor-Model-SVars**:
   Diese SVars werden nach Bedarf aus den Attributen des *Editor-Metamodels* erstellt. 
   Sie erlauben es, die Visualisierung der Elemente anzupassen, wie sie im Editormodell beschrieben wird\ [#f4]_.
   Neben literalen Attributen werden hier auch Attribute unterstützt, die Concepts referenzieren. Letztere werden für die meisten hier genannten SVars benötigt.

   Welche Editor-Attribute unterstützt werden wird von der ``ModelComponent`` festgelegt; dies sind\ [#f5]_:
   
    * Hintergrundfarbe (``backgroundColor``)
    * Schrift (``font``)
    * Schriftfarbe (``fontColor``)
    * Texturpfad (``texture``)
    * Liniendicke (``thickness``)
    * Spekulare Farbe (``specularColor``)

#. **Editor-SVars**:
   Dies sind SVars, die keine direkte Entsprechung im Modell haben und deren Werte daher auch nicht persistiert werden. 
   Sie sind automatisch für alle Modellelemente definiert oder werden durch Modellattribute "aktiviert".

   * SVars für die Auswahl von :ref:`Visualisierungsvarianten <visualisierungsvarianten>`: 

     * Deaktivierung (``disabled``), 
     * Hervorhebung (``highlighted``)
     * Selektion (``selected``)

   * Parameter für die Visualisierungsvarianten 
     
     * Breite des Selektionsrahmens (``borderWidth``)
     * Hevorhebungsfaktor (``highlightFactor``)
     * Transluzenzfaktor bei deaktivierten Elementen (``deactivatedAlpha``)
    
   Alle hier genannten SVars werden von der Modellkomponente aktiviert, wenn im Modell das Attribut ``interactionAllowed`` auf "true" gesetzt ist.
   

Alle SVars müssen eindeutig durch eine ``SVarDescription`` beschrieben werden, der ein Symbol zur Identifizierung und einen Scala-Datentyp umfasst. 
Die Symbole für Editor-SVars beginnen mit ``editor``; Symbole für *Domain-Model*-SVars werden mit ``model`` gekennzeichnet. 
Daran wird der Attributname aus dem Modell oder im Falle der *Editor*-SVars einer der unter 3. genannten Bezeichner angehängt, abgetrennt durch einen Punkt.

Anwendungsbeispiel 
^^^^^^^^^^^^^^^^^^

Die Nutzung erfolgt analog zu den schon gezeigten :ref:`SVars<model-svars-transformation>`, wozu ebenfalls ein impliziter Wrapper definiert ist.
Im folgenden Beispiel wird die Funktion eines Prozessknotens und die Schriftfarbe über die zugehörige Entity verändert:

.. code-block:: scala
    
    processEntity.svarSet("model.function")("Ausarbeitung schreiben")
    processEntity.svarSet("editor.fontColor")(Color.BLACK)


.. _lebenszyklus:

Übersicht über den Lebenszyklus von Model-Entitäten
===================================================

Dieser Abschnitt zeigt kurz, welche wichtigen Schritte im "Lebenszyklus" einer ``ModelEntity`` durchlaufen werden.

Komponenten in :ref:`simulatorx` definieren eine Reihe von Methoden, die vom Framework beim Erstellen oder Löschen einer Entity aufgerufen werden.

Die Erstellung einer ``ModelEntity`` folgt dem folgenden Schema:

  #. Der Vorgang wird beispielsweise durch ein ``CreateNode-Command`` vom Editor angestoßen. Die Modellkomponente erzeugt daraufhin eine ``EntityDescription`` mit den :ref:`Aspekten<modelentities-aspects>` und übergibt diese an das Framework (Methode ``EntityDescription.realize``), welches die Erstellung der Entity verwaltet und die folgenden Methoden aufruft.

  #. Die Methode ``getAdditionalProvidings`` gibt eine Sequenz von ``SVarDescriptions`` zurück, die zu der Entity hinzugefügt werden sollen. Im Falle der Modellkomponente sind dies ``SVarDescriptions`` zu den im vorherigen Abschnitt beschriebenen SVars.

  #. Anschließend wird die Methode ``getInitialValues`` aufgerufen, welche Initialwerte für die definierten SVars zurückgeben soll. Die Modellkomponente liest hierzu die Attributzuweisungen aus den Modell-Concepts aus oder setzt Standardwerte.

  #. Nach Fertigstellung einer Entity wird ``newEntityConfigComplete`` aufgerufen. Die Modellkomponente fügt die Entity zu ihrer internen Repräsentation hinzu und verbindet die Domain-Model-SVars mit den Attributen im Modell. Dies heißt, dass auf der SVar eine "Observe"-Funktion registriert wird, die bei jeder Änderung des SVar-Wertes auch den Wert im dahinterliegenden Domain-Concept ändert.\ [#f11]_

  #. Zum Abschluss werden Observer benachrichtigt, die auf die Erstellung von neuen Entities hören. Dies ist hier konkret die Editorkomponente, die auf diesem Weg die Entity zu ihrer internen Repräsentation hinzufügen kann.

Der genannte Ablauf spielt sich auch parallel für die anderen Komponenten ab, für die Aspects in der Entity definiert sind; hier also für die Render- und gegebenenfalls die Physikkomponente.

Beim Löschen spielt sich Folgendes ab:

  #. Das Löschen wird beispielsweise durch ein DeleteNode(fqnToDelete)-``Command`` vom Editor initiiert. Daraufhin startet die Modellkomponente den Löschvorgang, indem auf der zur FQN gehörigen Entity die Methode ``remove`` aufgerufen wird.

  #. Simulator X entfernt nun die Entity aus dem System und ruft dabei in der Komponente die ``removeFromLocalRep``-Methode auf. In dieser Methode sollen interne Verweise und zugehörige Daten in den Komponenten entfernt werden.

.. _entity-erstellen-aktivitaet:

.. figure:: _static/diags/entity-erstellen-aktivitaet.eps
    :width: 18cm

    Ablauf der Erstellung einer ModelEntity durch die Editorkomponente


.. [#f1] Die Benennung "Usage-Model" ist eigentlich nicht ganz passend, da das :ref:`domain-model` auch die vom Benutzer erstellten Knotentypen umfasst. Da diese Bezeichnungsweise in der Implementierung zu finden ist, wird diese hier ebenfalls genutzt.

.. [#f2] Dass hier die FQNs aus dem Modell genutzt werden hat keine besondere Bedeutung und ist nur ein "Implementierungsdetail", auf das man sich nicht verlassen solle.

.. [#f4] Es wäre auch erlaubt, Attribute zu integrieren, die nicht direkt die Visualisierung betreffen, aber das Editor-Verhalten modifizieren. Dies wird bisher aber nicht genutzt.

.. [#f5] Es war nicht möglich, die Implementierung (auf einfachem Wege) so flexibel zu gestalten wie bei Domain-Model-SVars, was leider dazu führt, dass man keine Attribute hinzufügen kann ohne die ``ModelComponent`` anzupassen.

.. [#f7] Dies ist nicht nötig, da die Auswahl von Kanten nicht unterstützt werden soll und Kollisionen mit Verbindungen eher als hinderlich gesehen wurden. Außerdem könnte eine große Anzahl von Verbindungen schnell zu Geschwindigkeitsproblemen der Physiksimulation führen.

.. [#f8] Skalierung wurde für das Projekt hinzugefügt. Dazu wurde die Physikkomponente modifiziert und die selbstgeschriebene Renderkomponente entsprechend ausgelegt.

.. [#f9] Die svarGet-Methode (ebenfalls svarSet und weitere) wurde für das Projekt in einem impliziten Wrapper für Entities definiert um den Zugriff auf SVars zu "verschönern".

.. [#f10] Die Darstellung ist aber auch durchaus "menschenlesbar" und wird ähnlich formatiert wie im Metamodell-Editor von OMME.

.. [#f11] Bei den Editor-Model-SVars wird ein anderer Ansatz genutzt, da diese teilweise häufig geändert werden (vor allem die Position). Diese SVars werden erst beim Speichern des Modells ausgelesen und zurückgeschrieben, um Probleme mit der Ausführungsgeschwindigkeit zu vermeiden.
