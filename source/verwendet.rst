*********************************
Verwendete Techniken und Software
*********************************

Scala
*****

Die Implementierung des i>PM3D-Projekts erfolgte zum größten Teil in der Programmiersprache Scala :cite:`odersky_programming_2011` :cite:`www:scala`.
Die Verwendung von Scala ergab sich aus der Entscheidung, die in Scala implementierte Simulations-Middleware :ref:`simulatorx` als Basis für den Prototypen zu verwenden. 

Scala wird als "objektfunktionale" Programmiersprache charakterisiert. 
"Objektfunktional" soll die Bestrebungen ausdrücken, Aspekte aus funktionalen und objektorientierten Programmiersprachen zu einer flexiblen und effektiven Programmiersprache zu kombinieren.

Scala wird zur Zeit vorwiegend auf der Java VM genutzt, wobei der Compiler auch in der Lage ist, CIL-Code für die .NET-Runtime zu erzeugen. 
I>PM3D läuft wegen einiger Abhängigkeiten von Java-Bibliotheken bisher ausschließlich auf der Java VM.

Die "objektorientierte Facette" Scalas orientiert sich an den Konzepten von Java, bietet aber einige Erweiterungen.

Hier werden nur Features kurz vorgestellt, die für die Implementierung besonders hilfreich waren und in späteren Kapiteln erwähnt werden.


.. _traits:

Traits
------

Als Erweiterung zu Java unterstützt Scala eine (eingeschränkte) Mehrfachvererbung von Implementierungscode über sog. *Traits*. 
Traits kann man sich als ein Java-Interface vorstellen, in dem Methoden schon vorimplementiert sein können.

Zur Vereinfachung dürfen Traits keinen Konstruktor definieren.

Neben der Verwendung als "Interface" – wie in Java – werden diese oft genutzt, um wiederverwendbare Code-Einheiten zu realisieren, die sich in verschiedenen Klassen einsetzen lassen. 
Traits werden daher oft als **Mixin** bezeichnet.
Wie ein Trait zu einer Klasse hinzugefügt wird – auch **einmischen** genannt – zeigt das folgende Scala-Codebeispiel:

.. code-block:: scala

    class Example extends BaseClass with MixinTrait

``Example`` wird hiermit von ``BaseClass`` abgeleitet und ``MixinTrait`` eingemischt.

In dieser Arbeit werden Traits in UML-Diagrammen als Klasse mit dem Stereotyp *<<trait>>* dargestellt und Assoziationen zu einmischenden Klassen mit *<<mixin>>* versehen.

Objects
-------

Anstelle der aus Java bekannten statischen Klassenmethoden oder Singleton-Klassen wird in Scala das *object*-Konstrukt genutzt. 
*Objects* können Klassen "erweitern", das bedeutet, dass das *Object* als Instanz der Klasse betrachtet werden kann. 

Als Beispiel sei hier ein ausführbares Scala-Programm gezeigt, welches eine (im Sinne von Java statische) Methode definiert und aufruft:

.. code-block:: scala

    object Main extends App {
        def hello() {
            println("Hello World")
        }
        hello()
    }

Actors
------

Ein sinnvoller Einsatzbereich von Scala ist unter anderem die Erstellung von parallelen und verteilten Anwendungen.
Dazu kommt oft das **Actor-Modell** :cite:`haller_scala_2009` zum Einsatz, das vorher schon in der Programmiersprache Erlang :cite:`www:erlang` realisiert wurde.

Grundlage für das Actor-Modell ist das **message passing**. 
Dies bedeutet, dass verschiedene Actors ausschließlich über Nachrichten Informationen austauschen.
Es ist nicht erlaubt, auf gemeinsame, veränderliche Datenstrukturen zuzugreifen.

In Scala wird eine Nachricht oft durch ein Objekt einer **case class** dargestellt\ [#f9]_.
Diese Klassen werden dafür genutzt, Daten zu unveränderlichen Objekten zusammenzufassen, wie im folgenden Code gezeigt wird:

.. code-block:: scala

    case class Message(data: String, number: Int)
    receivingActor ! Message("hello!", 42)

In der zweiten Zeile wird ein Objekt der Klasse ``Message`` erzeugt und an ``receivingActor`` gesendet.

Ein Actor kann auf Basis eines (Java)-Threads realisiert sein, jedoch ist dies keine zwingende Voraussetzung. 


.. _implicit:

Implizite Methoden
------------------

Es ist möglich, sog. "implizite Methoden" zu definieren, welche vom Compiler automatisch eingesetzt werden können, wenn diese benötigt werden\ [#f8]_.
Besonders praktisch sind diese Methoden für die Realisierung von "transparenten" Adaptern, wie sie im vorliegenden Projekt genutzt werden. 
Diese werden auch **implizite Wrapper** genannt.

.. code-block:: scala

    implicit def conceptToAdapter(m: MConcept) = new MConceptAdapter(m)

Mit dieser Definition lassen sich nun Methoden, die für ``MConceptAdapter`` definiert sind auch auf Objekten des Typs ``MConcept`` aufrufen als wären sie Teil von ``MConcept``.


.. _parser-kombinatoren:

Parser-Kombinatoren
-------------------

Die Scala-Standardbibliothek bietet eine einfache Möglichkeit, Parser mit Hilfe von Parser-Kombinatoren :cite:`odersky_programming_2011` zu erstellen. 
Dies wird in dieser Arbeit für die Laden von Modellen in einer textuellen Repräsentation eingesetzt. 

Einfache Parser werden von Parser-Kombinatoren zu komplexeren Parsing-Ausdrücken zusammengesetzt. 
Parser sind als Funktionen definiert, die einen String auf eine beliebige Ausgabe abbilden. 
Parser-Kombinatoren sind Funktionen höherer Ordnung, die Parser als Eingabe erwarten und als Ausgabe wiederum eine Parser-Funktion liefern.

In Scala werden die Bestandteile der textuellen Eingabe oft in Objekte von *case classes* übersetzt, die zusammen einen Syntaxbaum der Eingabe ergeben.

Folgende Parser-Funktion 

.. code-block:: scala

    def stringAssignment = ident ~ ("=" ~> stringLits <~ ";") ^^ {
      case id ~ stringLits => LiteralTypeAssignment(id, stringLits)
    }


würde beispielsweise die :ref:`LML-String-Zuweisung<lmm>` 
.. code-block:: java
    
    functions = "a", "test";

erkennen und in ein Scala-Objekt des Typs ``LiteralTypeAssignment`` übersetzen. Dieser Typ könnte wie folgt definiert sein:

.. code-block:: scala

    case class LiteralTypeAssignment(id: String, stringLiterals: List[String])


.. _simulatorx:

Simulator X
***********

*Simulator X* :cite:`latoschik_simulator_2011` :cite:`fischbach_sixtons_2011` bezeichnet eine neuartige Simulations-Middleware, die die Realisierung von interaktiven Anwendungen in einer virtuellen 3D-Umgebung besonders einfach machen soll.
Der Fokus liegt hierbei auch auf einer Anbindung von neuartigen Eingabemethoden wie Gesten- und Sprachsteuerung. Dies macht Simulator X zu einer gut geeigneten Plattform für den I>PM3D-Prototypen.

*Simulator X* setzt auf dem (Scala-)Actor-Modell auf welches dafür sorgt, dass Programmkomponenten möglichst gut entkoppelt werden

Außerdem sorgt dies auch dafür, dass auch aktuelle Rechnersysteme mit mehreren Prozessorkernen gut ausgelastet werden können ohne den Programmierer mit der expliziten Verwaltung von parallelen Threads und den daraus resultierenden Schwierigkeiten zu belasten.

Aufbauend auf dem Actor-Modell stellt *Simulator X* ein Event-System und eine Abstraktion globaler Zustandsvariablen zur Verfügung. 

Globale Zustandsvariablen, SVars genannt, vereinfachen für den Programmierer den Umgang mit verteilten Daten. Ein bestimmtes Datum wird von genau einem Actor, dem Besitzer verwaltet. Andere Actors besitzen nur eine spezielle Referenz auf den Wert und müssen mit dem Besitzer kommunizieren um den Wert auszulesen oder zu manipulieren.
Eine zugeordnete SVarDescription\ [#f1]_ benennt die SVar, gibt ihr einen Scala-Datentyp und definiert deren Semantik in einer Anwendung.

Zusammengehörige Referenzen auf Zustandsvariablen werden zur einfacheren Handhabung zu Entitäten zusammengefasst. Eine Entity beschreibt genau ein Simulationsobjekt\ [#f2]_ und dessen Daten. 

Simulator-X-Anwendungen sind aus Komponenten aufgebaut. Diese setzen auf dem Actormodell auf und kommunizieren miteinander über den Austausch von Nachrichten oder durch das Setzen von SVars in Entities.
Eine Komponente sollte möglichst eine genau abgegrenzte Funktionalität wie beispielsweise ein KI-Modul oder eine Grafikausgabeeinheit realisieren. 

Um eine Entity zu beschreiben wird eine *EntityDescription* erstellt, die aus mehreren *Aspect*-Definitionen aufgebaut sein kann :cite:`wiebusch_enhanced_2012`.

Aspects beschreiben sozusagen eine Facette der Entity und sind einer bestimmten Komponente zugeordnet. So gibt es beispielsweise Grafik- oder Physik-Aspects.
Über die Aspekt-Definition können Werte durch den Benutzer vorgegeben werden, die einer Komponente weitere Informationen geben, wie die komponenten-internen Entity-Repräsentation erstellt werden soll.
Beispiele hierfür sind die Masse des Objekts für eine Physikkomponente oder der Pfad zu einer Modell-Datei für die Grafikkomponente.

Wenn eine Entity vom Simulator-X-System erstellt wird, wird dieser Aspect an die zugeordnete Komponente weitergegeben. 
Andere Komponenten können sich allerdings beim *WorldInterface* registrieren um Informationen über alle Aspects zu bekommen.

*Simulator X* befindet sich gerade in der Entwicklung. Für das vorliegende Projekt wird eine Version von August 2011 genutzt.

.. _opengl:

OpenGL / LWJGL
**************

Um die Grafikausgabe von I>PM3D zu realisieren, wird die plattformunabhängige 3D-Schnittstelle OpenGL :cite:`www:opengl` genutzt. 

Zur Anbindung an OpenGL wird die Java-Bibliothek LWJGL (Lightweight Java Gaming Library) :cite:`www:lwjgl` in der Version 2.8.2 eingesetzt. 
Zusätzlich stellt LWJGL eine Schnittstelle für den Zugriff auf Tastatur- und Mausdaten zur Verfügung.

Hier sollen nur einige wenige Hinweise zu "modernem" OpenGL (ab Version 3.0) und den in späteren Kapiteln benutzten Begriffen gegeben werden. 
Näheres kann in :cite:`wright_opengl_2010` oder unter :cite:`www:opengl` nachgelesen werden.

In älteren OpenGL-Versionen (1.x) wurden von OpenGL viele, fest eingebaute Funktionen wie die Berechnung der Beleuchtung und Texturierung bereitgestellt, die nur aktiviert und konfiguriert werden mussten. 
Deshalb wird "altes" OpenGL oft mit dem Begriff *fixed-function-Pipeline* in Verbindung gebracht.

Mit Version 3.0 wurden viele dieser Funktionen aus dem Kern von OpenGL entfernt. In neueren Versionen müssen die Berechnungen durch den Programmierer selbst in *Shadern* implementiert werden. 

Das neue Konzept gibt jedoch dem Programmierer die Freiheit, auch völlig neue Grafikeffekte zu implementieren, die mit der *fixed-function-Pipeline* nicht oder nur schwer umsetzbar gewesen wären. 
Diese Möglichkeit wurde in dieser Arbeit für einige "Spezialeffekte" genutzt, die sich auf diesem Weg einfach realisieren ließen.

Bei **Shadern** handelt es sich um kleine Programme, die in der Programmiersprache GLSL (OpenGL Shading Language) geschrieben und die direkt auf dem Grafikprozessor von sog. *Shader-Einheiten* ausgeführt werden.
Code kann in GLSL in Funktionen ausgelagert und so in mehreren Shadern genutzt werden.
Shader erfüllen verschiedene Aufgaben an von OpenGL festgelegten Positionen innerhalb der Render-Pipeline :cite:`www:glpipe`.

In OpenGL 4 werden folgende Typen unterstützt:

Vertex-Shader  
    arbeiten auf einzelnen Modell-Vertices\ [#f4]_ und sind beispielsweise für die Transformation von Modellkoordinaten in das von OpenGL benutzte Koordinatensystem zuständig.

Geometry-Shader
    können aus den gegebenen Vertices neue Zwischen-Vertices erzeugen.

Fragment-Shader 
    werden einmal pro Fragment aufgerufen\ [#f3]_ und implementieren beispielsweise Texturierung und Beleuchtung.

Tesselation-Shader (ab OpenGL 4)
    können komplett neue Geometrien erzeugen.

Mit **Vertex-Attributen** lassen sich beliebige Daten pro Vertex an die Shaderprogramme übertragen; häufig sind das Vertexkoordinaten\ [#f4]_, Normalen\ [#f5]_ und Texturkoordinaten\ [#f6]_.
Vertex-Attribute werden vom Shader aus Puffern im Grafikspeicher ausgelesen, welche als Vertex Buffer Objects (VBO) bezeichnet werden.

**Uniforms** übermitteln Werte an Shaderprogramme, die üblicherweise über ein ganzes Grafikobjekt konstant bleiben. Dies können beispielsweise Lichtparameter oder Farbwerte sein.


Sonstiges
*********

.. _stringtemplate:

StringTemplate
--------------

Um Prozessmodelle in einer textuellen Form speichern zu können, wird die Template-Bibliothek *StringTemplate* (ST) in der Version 4.0.4 verwendet. :cite:`parr_language_2009` 

ST folgt dem Prinzip, einen Text mit "Platzhaltern" (Attributen) zu definieren. Die Attribute werden aus dem Anwendungsprogramm heraus gesetzt und so das Template mit Inhalt gefüllt.

Um die Nutzung von ST in Scala zu vereinfachen, wurde für diese Arbeit eine dünne Abstraktionsschicht in Scala implementiert. 
Diese Schicht sorgt unter anderem dafür, dass beliebige Scala-Objekte als Java-Bean an ST weitergegeben werden können, auch wenn sie selbst nicht der Java-Bean-Konvention entsprechen.

Zur Erstellung eines den Konventionen folgenden Wrapper-Objekts wird :cite:`www:clapper` genutzt.

In folgendem Beispiel wird ein Template erstellt, welches die :ref:`LMM-Zuweisung<lmm>` ``function = "test"`` produziert:

.. code-block:: scala

    val assignTemplate = "<attribName> = \"<value>\""
    val assignST = ST(assignTemplate)
    assignST.addAll(
        "attribName" -> "function",
        "value" -> "test")
    val output = assignST.render


.. _simplex3d:

Simplex3D-Math
--------------

Im I>PM3D-Projekt wird die in Scala implementierte Mathematikbibliothek *Simplex3D-Math* in der Version 1.3 :cite:`www:simplex3d` genutzt. 

Durch die Bibliothek werden Matrizen, Vektoren und dazugehörige Utility-Funktionen bereitgestellt. Deren API orientiert sich weitgehend an der OpenGL Shading Language.


.. [#f1] Beispiele für SVar-Typen: *Color*, *Transformation* oder *Mass*
.. [#f2] Dies könnte im Prozesseditor beispielsweise ein Modellelement wie ein Prozess oder eine Kontrollflusskante sein.
.. [#f3] Ein Fragment entspricht – vereinfacht gesagt – einem Pixel auf dem Bildschirm.
.. [#f4] Ein Vertex ist ein "Eckpunkt" eines 3D-Modells. Vertexkoordinaten sind die Koordinaten des Punkts im 3D-Raum. OpenGL "rendert" ein 3D-Objekt, indem eine Liste von Vertices der Reihe nach gezeichnet wird.
.. [#f5] Normalen werden vor allem für die Berechnung der Beleuchtung benötigt.
.. [#f6] Texturkoordinaten sind häufig zweidimensional und werden vor allem dazu genutzt, 2D-Grafiken auf 3D-Objekten zu positionieren.
.. [#f7] Siehe http://www.opengl.org/wiki/Rendering_Pipeline_Overview
.. [#f8] Welche Bedingungen dafür erfüllt sein müssen, kann bspw. in :cite:`odersky_programming_2011` nachgelesen werden.
.. [#f9] Das *case class*-Konstrukt erzeugt eine Klasse, in der gewisse Methoden vorimplementiert sind, die bspw. einen inhaltlichen Vergleich mit dem ==-Operator oder einen Einsatz im *pattern matching* erlauben. Siehe :cite:`odersky_programming_2011`.
