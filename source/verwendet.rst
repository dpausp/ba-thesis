*********************************
Verwendete Techniken und Software
*********************************

Scala
*****

Die Implementierung der Konzepte dieser Arbeit erfolgte nahezu vollständig in der Programmiersprache Scala :cite:`odersky_programming_2011` :cite:`www:scala` 

Die Verwendung von Scala ergab sich aus der Entscheidung, die in Scala implementierte Simulations-Middleware :ref:`simulatorx` als Basis für den I>PM3D-Prototypen zu verwenden. 

Scala wird als objektfunktionale Programmiersprache charakterisiert. "Objektfunktional" soll die Bestrebungen ausdrücken, Aspekte aus funktionalen und objektorientierten Programmiersprachen zu einer effektiven Programmiersprache zu kombinieren.

Scala wird zur Zeit vorwiegend auf der Java VM genutzt, wobei der Compiler auch in der Lage ist, CIL-Code für die .NET-Runtime zu erzeugen. 
I>PM3D läuft wegen einiger Abhängigkeiten von Java-Bibliotheken bisher ausschließlich auf der Java VM.

Hier werden nur Features kurz vorgestellt, die für die Implementierung besonders hilfreich waren und in späteren Kapiteln erwähnt werden.


.. _traits:

Traits
------

Gegenüber Java unterstützt Scala eine (eingeschränkte) Mehrfachvererbung von Implementierungscode über sogenannte **traits**. 
Traits kann man sich auch als ein Java-Interface vorstellen, in dem Methoden schon vorimplementiert sein können.

Zur Vereinfachung dürfen Traits keinen Konstruktor definieren.

Neben der Verwendung als "Interface" wie in Java werden diese oft genutzt um wiederverwendbare Code-Einheiten zu realisieren, die sich in verschiedenen Klassen verwenden lassen. 
Traits werden daher oft als "Mixin" bezeichnet.
Der "Vorgang", einen trait zu einer Klasse hinzuzufügen – wie im folgenden Code gezeigt wird – wird in dieser Arbeit "einmischen" genannt.
.. code-block:: scala

    class Example extends BaseClass with MixinTrait

Example wird also von BaseClass abgeleitet und MixinTrait eingemischt.

Actors
------

Ein sinnvoller Einsatzbereich von Scala ist unter Anderem die Erstellung von parallelen und verteilten Anwendungen.
Dazu kommt oft das Actor-Modell :cite:`haller_scala_2009` zum Einsatz, das vorher schon in der Programmiersprache Erlang :cite:`www:erlang` realisiert wurde.

Grundlage für das Actor-Patterns ist das *message passing*. 
Dies bedeutet, dass verschiedene Actors ausschließlich über Nachrichten Informationen austauschen.
Umgekehrt bedeutet das, dass nicht auf gemeinsame, veränderliche Datenstrukturen zugegriffen wird ("shared nothing"). 
Die Kommunikation der Actors erfolgt überlicherweise asynchron.

In Scala wird eine Nachricht oft durch ein Objekt einer *case class* dargestellt.

Case Classes fassen Daten zu unveränderlichen Objekten zusammen, wie im folgenden Code gezeigt wird:

.. code-block:: scala

    case class Message(data: String, number: Int)
    receivingActor ! Message("hello!", 42)

Ein Actor kann auf Basis eines (Java)-Threads realisiert sein, jedoch ist dies keine zwingende Voraussetzung. 


.. _implicit:

Implizite Funktionen
--------------------

Es ist möglich, sogenannte "implizite Funktionen zu definieren, indem ein "implicit" vorangestellt wird. 
Diese Funktionen werden vom Compiler automatisch eingesetzt, wenn diese benötigt werden. Dazu müssen die Funktion im der scala-Quelldatei direkt importiert worden.


Besonders praktisch sind diese Funktionen für die Realisierung von "transparenten" Adaptern, wie sie im vorliegenden Projekt genutzt werden.

.. code-block:: scala

    implicit def conceptToAdapter(m: MConcept) = new MConceptAdapter(m)

Mit dieser Definition lassen sich nun Methoden, die für MConceptAdapter definiert sind auch auf Objekten des Typs MConcept aufrufen als wären sie Teil von MConcept. [#f4]_


.. _parser-kombinatoren:

Parser-Kombinatoren
-------------------

Die Scala-Standardbibliothek bietet eine einfache Möglichkeit, Parser mit Hilfe von Parser-Kombinatoren :cite:`odersky_programming_2011` zu erstellen. 
Dies wurde in dieser Arbeit für die Laden von Modellen in einer textuellen Repräsentation verwendet. 

Einfache Parser werden von Parser-Kombinatoren zu komplexeren Parsing-Ausdrücken zusammengesetzt. Parser sind als Funktionen definiert, die einen String auf eine beliebige Ausgabe abbilden. 
Parser-Kombinatoren sind Funktionen höherer Ordnung, die Parser als Eingabe erwarten und als Ausgabe wiederum eine Parser-Funktion liefern.

Anders ausgedrückt stellen Parserkombinator-Ausdrücke direkt die Grammatik der Sprache dar.

In Scala werden die Bestandteile der textuellen Eingabe oft in Objekte von *case classes* übersetzt, die zusammen einen Syntaxbaum der Eingabe ergeben.

Folgende Parser-Funktion 

.. code-block:: scala

    def stringAssignment = ident ~ ("=" ~> stringLits <~ ";") ^^ {
      case id ~ stringLits => LiteralTypeAssignment(id, stringLits)
    }

würde beispielsweise die LMM-String-Zuweisung 

.. code-block:: java

    functions = "a", "test";

    
erkennen und in ein Scala-Objekt des Typs *LiteralTypeAssignment* übersetzen. Dieser Typ könnte wie folgt definiert sein:

.. code-block:: scala

    case class LiteralTypeAssignment(id: String, stringLiterals: List[String])


.. _simulatorx:

Simulator X
***********

*Simulator X* bezeichnet es sich um ein neuartige Simulations-Middleware, die die Realisierung von interaktiven Anwendungen in einer virtuellen 3D-Umgebung besonders einfach machen soll. 
Der Fokus liegt hierbei auch auf einer Anbindung von neuartigen Eingabemethoden wie Gesten- und Sprachsteuerung. Dies macht Simulator X zu einer gut geeigneten Plattform für den I>PM3D-Prototypen.

*Simulator X* setzt auf dem (Scala-)Actor-Modell auf welches dafür sorgt, dass Programmkomponenten möglichst gut entkoppelt werden

Außerdem sorgt dies auch dafür, dass auch aktuelle Rechnersysteme mit mehreren Prozessorkernen gut ausgelastet werden können ohne den Programmierer mit der expliziten Verwaltung von parallelen Threads und den daraus resultierenden Schwierigkeiten zu belasten.

Aufbauend auf dem Actor-Modell stellt *Simulator X* ein Event-System und eine Abstraktion globaler Zustandsvariablen zur Verfügung. 

Globale Zustandsvariablen, SVars genannt, vereinfachen für den Programmierer den Umgang mit verteilten Daten. Ein bestimmtes Datum wird von genau einem Actor, dem Besitzer verwaltet. Andere Actors besitzen nur eine spezielle Referenz auf den Wert und müssen mit bem Besitzer kommunizieren um den Wert auszulesen oder zu manipulieren.
Eine zugeordnete SVarDescription\ [#f1]_ benennt die SVar, gibt ihr einen Scala-Datentyp und definiert deren Semantik in einer Anwendung.

Zusammengehörige Referenzen auf Zustandsvariablen werden zur einfacheren Handhabung zu Entitäten zusammengefasst. Eine Entity beschreibt genau ein Simulationsobjekt\ [#f2]_ und dessen Daten. 

Simulator-X-Anwendungen sind aus Komponenten aufgebaut. Diese setzen auf dem Actormodell auf und kommunizieren miteinander über den Austausch von Nachrichten oder durch das Setzen von SVars in Entities.
Eine Komponente sollte möglichst eine genau abgegrenzte Funktionalität wie beispielsweise ein KI-Modul oder eine Grafikausgabeeinheit realisieren. 

Um eine Entity zu beschreiben wird eine *EntityDescription* erstellt, die aus mehreren *Aspect*-Definitionen aufgebaut sein kann.

Aspects beschreiben sozusagen eine Facette der Entity und sind einer bestimmten Komponente zugeordnet. So gibt es beispielsweise Grafik- oder Physik-Aspects.
Über die Aspekt-Definition können Werte durch den Benutzer vorgegeben werden, die einer Komponente weitere Informationen geben, wie die komponenten-internen Entity-Repräsentation erstellt werden soll.
Beispiele hierfür sind die Masse des Objekts für eine Physikkomponente oder der Pfad zu einer Modell-Datei für die Grafikkomponente.

Wenn eine Entity vom Simulator-X-System erstellt wird, wird dieser Aspect an die zugeordnete Komponente weitergegeben. 
Andere Komponenten können sich allerdings beim *WorldInterface* registrieren um Informationen über alle Aspects zu bekommen.

*Simulator X* befindet sich gerade in der Entwicklung. Für das vorliegende Projekt wird eine Version von August 2011 genutzt.

.. _opengl:

OpenGL / LWJGL
**************

Um die Grafikausgabe des I>PM3D-Projektes zu realisieren wurde die plattformunabhängige 3D-Schnittstelle OpenGL :cite:`www:opengl` genutzt. 

Als Anbindung an OpenGL wird die Java-Spielebibliothek LWJGL :cite:`www:lwjgl` in der Version 2.8.2 eingesetzt. 
Zusätzlich stellt LWJGL eine Schnittstelle für den Zugriff auf Tastatur- und Mausdaten zur Verfügung.

Hier soll nur einige wenige Hinweise zu "modernem" OpenGL und den in späteren Kapiteln benutzten Begriffen gegeben werden. 

In älteren OpenGL-Versionen (1.x) wurden von OpenGL viele, fest eingebaute Funktionen wie die Berechnung der Beleuchtung und Texturierung bereitgestellt, die vom Programmierer einfach nur aktiviert und konfiguriert werden mussten. 
Deshalb wird "altes" OpenGL oft mit dem Begriff *fixed-function-Pipeline* in Verbindung gebracht.

Mit Version 3.0 wurden viele dieser Funktionen aus dem Kern von OpenGL entfernt. In neueren Versionen müssen die Berechnungen selbst durch den Programmierer in *Shadern* implementiert werden. 

Das neue Konzept gibt jedoch dem Programmierer die Freiheit, auch völlig neue Grafikeffekte zu implementieren, die mit der alten Pipeline nicht oder nur schwer umsetzbar gewesen wären. 
Diese Möglichkeit wurde in dieser Arbeit auch für einige "Spezialeffekte" genutzt, wie in :ref:`render-bibliothek` beschrieben wird.

Bei *Shadern* handelt es sich um kleine Programme, die in der Programmiersprache GLSL (OpenGL Shading Language) geschrieben und die direkt auf dem Grafikprozessor von sogenannten *Shader-Einheiten* ausgeführt werden.
Diese Programme erfüllen verschiedene Aufgaben an von OpenGL festgelegten Positionen innerhalb der Rendering-Pipeline. In OpenGL 4 werden folgende Typen unterstützt:

Vertex-Shader  
    arbeiten auf einzelnen Modell-Vertices und sind beispielsweise für die Transformation von Modellkoordinaten in das von OpenGL benutzte Koordinatensystem zuständig.

Geometry-Shader
    könnnen aus den gegebenen Vertices neue Zwischen-Vertices erzeugen.

Fragment-Shader 
    werden einmal pro Fragment aufgerufen [#f3]_ und implementieren bespielsweise Texturierung und Beleuchtung.

Tesselation-Shader (ab OpenGL 4)
    können komplett neue Geometrien erzeugen.

Mit *Vertex-Attributen* lassen sich beliebige Daten pro Vertex, an die Shaderprogramme übertragen; häufig sind das Vertexkoordinaten, Normalen und Texturkoordinaten.
Vertex-Attribute werden vom Shader aus Puffern im Grafikspeicher ausgelesen, welche als Vertex Buffer Objects (VBO) bezeichnet werden.

*Uniforms* übermitteln Werte an Shaderprogramme, die üblicherweise über ein komplettes Grafikobjekt konstant bleiben. Dies können beispielsweise Lichtparameter oder Farbwerte sein.


Sonstiges
*********

StringTemplate
--------------

Um Prozessmodelle in einer textuellen Form speichern zu können wird die Template-Bibliothek *StringTemplate*, in der Version 4.0.4 verwendet. :cite:`parr_language_2009` 

ST folgt dem Prinzip, Templates als Text mit Platzhaltern zu definieren. Die Platzhalter werden durch das Setzen von Attributen aus dem Anwendungsprogramm heraus mit Inhalt gefüllt.

Um die Nutzung von *StringTemplate* in Scala zu vereinfachen wurde eine dünne Abstraktionsschicht in Scala implementiert. 
Diese Schicht sorgt unter Anderem dafür, dass beliebige Scala-Objekte als Java-Bean an *StringTemplate* weitergegeben werden können, auch wenn sie selbst nicht der Java-Bean-Konvention entsprechen.

Für Erstellung eines den Konventionen folgenden Wrapper-Objekts wird :cite:`www:clapper` genutzt.

Beispiel für ein Template, dass eine String-Zuweisung in LMM produziert:


.. code-block:: scala

    val assignTemplate = "<attribName> = \"<value>\""
    val assignST = ST(assignTemplate)
    assignST.addAll(
        "attribName" -> "functions",
        "value" -> "test")
    val output = assignST.render


.. _simplex3d:

Simplex3D-Math
--------------

Im gesamten I>PM3D-Projekt wird die in Scala implementierte Mathematikbibliothek *Simplex3D-Math* in der Version 1.3 :cite:`www:simplex3d` genutzt. 

Durch die Bibliothek werden Matrizen, Vektoren und dazugehörige Utility-Funktionen bereitgestellt. Deren API orientiert sich weitgehend an der OpenGL Shading Language.

SLF4J / Logback
---------------

Für die Aufzeichnung von Logging-Informationen wird die Java-Logging-API *SLF4J* :cite:`www:slf4j` in der Version 1.6.4 mit Logback (1.0.0) als Implementierung eingesetzt. 
Um die Einbindung in Scala zu verbessern wurde ein eigener Wrapper für die SLF4J-API entwickelt.


.. [#f1] Beispiele für SVar-Typen: *Color*, *Transformation* oder *Mass*
.. [#f2] Dies könnte im Prozesseditor beispielsweise ein Modellelement wie ein Prozess oder eine Kontrollflusskante sein.
.. [#f3] Ein Fragment entspricht einem Pixel auf dem Bildschirm, wenn man Antialiasing vernachlässigt
.. [#f4] Dies wird (auch von offizieller Seite) als "Pimp my Library" bezeichnet. Näheres zu impliziten Funktionen: :cite:`odersky_programming_2011`
