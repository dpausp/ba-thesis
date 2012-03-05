*********************************
Verwendete Techniken und Software
*********************************

Scala
*****

Die Implementierung der Konzepte dieser Arbeit erfolge nahezu vollständig in der Programmiersprache Scala :cite:`odersky_programming_2011` :cite:`www_scala` 

Die Verwendung von Scala ergab sich aus der Entscheidung, die Simulations-Middleware :ref:`simulator_x` als Basis für den I>PM3D-Prototypen zu verwenden. 

Scala wird als objektfunktionale Programmiersprache charakterisiert. "Objektfunktional" soll die Bestrebungen ausdrücken, Aspekte aus funktionalen und objektorientierten Programmiersprachen zu einer effektiven Programmiersprache zu kombinieren.

Scala wird zur Zeit vorwiegend auf der Java Virtual Machine genutzt, wobei der Compiler auch in der Lage ist, CIL-Code für die .NET-Runtime zu erzeugen. I>PM3D läuft wegen einiger Abhängigkeiten von Java-Bibliotheken bisher ausschließlich auf der Java VM.

Actors
------

Ein sinnvoller Einsatzbereich von Scala ist unter Anderem die Erstellung von parallelen und verteilten Anwendungen.
Dazu kommt oft das Actor-Modell :cite:`haller_scala_2009` zum Einsatz, das vorher schon in der Programmiersprache Erlang :cite:`www_erlang` realisiert wurde.

Grundlage für das Actor-Patterns ist das *message passing*, das bedeutet, dass verschiedene Actors ausschließlich über Nachrichten Informationen austauschen. In Scala ist dies üblicherweise ein Objekt einer *case class*, die  vor Allem dazu benutzt werden, Daten zu unveränderlichen Objekten zusammenzufassen.

.. code-block:: scala

    case class Message(data: String, number: Int)
    receivingActor ! Message("hello!", 42)


Actors können mit Hilfe von Threads realisiert sein, jedoch ist dies keine zwingende Voraussetzung.

Parser-Kombinatoren
-------------------

Die Scala-Standardbibliothek bietet eine einfache Möglichkeit, Parser mit Hilfe von Parser-Kombinatoren :cite:`odersky_programming_2011` zu erstellen, welche im Rahmen dieser Arbeit für die Laden von Modellen in einer textuellen Repräsentation genutzt wurde. Parser-Kombinatoren werden in funktionalen Programmiersprachen genutzt um rekursiv absteigende Parser zu realisieren.

Einfache Parser werden von Parser-Kombinatoren zu komplexeren Parsing-Ausdrücken zusammengesetzt. Parser sind als Funktionen definiert, die einen String auf eine beliebige Ausgabe abbilden. Parser-Kombinatoren sind Funktionen höherer Ordnung, die Parser als Eingabe erwarten und als Ausgabe wiederum eine Parser-Funktion liefern.

In Scala werden die Bestandteile einer textuellen Eingabe oft in Objekte von *case classes* übersetzt, die zusammen einen abstrakten Syntaxbaum ergeben.

Die Notation von Parser-Kombinatoren in Scala erinnert an die Spezifikation einer Grammatik mittels der Extended-Backur-Naur-Form.

Folgende Parser-Funktion 

.. code-block:: scala

    def stringAssignment = ident ~ ("=" ~> stringLits <~ ";") ^^ {
      case id ~ stringLits => LiteralTypeAssignment(id, stringLits)
    }

würde beispielsweise in die LMM-String-Zuweisung ``functions = "a", "test";`` erkennen und in ein Scala-Objekt des Typs *LiteralTypeAssignment* übersetzen.

.. _simulator_x:

Simulator X
***********

Bei Simulator X handelt es sich um ein Konzept und eine prototypische Implementierung für eine neuartige Simulations-Middleware, die die Realisierung von interaktiven Anwendungen in einer virtuellen 3D-Umgebung besonders einfach machen soll. 
Der Fokus liegt hierbei auch auf einer Anbindung von neuartigen Eingabemethoden wie Gesten- und Sprachsteuerung. Dies macht Simulator X zu einer gut geeigneten Plattform für den I>PM3D-Prototypen.

Simulator X setzt auf dem (Scala-)Actor-Modell auf welches dafür sorgt, dass Programmkomponenten möglichst gut entkoppelt werden

Außerdem sorgt dies auch dafür, dass auch aktuelle Rechnersysteme mit mehreren Prozessorkernen gut ausgelastet werden können ohne den Programmierer mit der expliziten Verwaltung von parallelen Threads und den daraus resultierenden Schwierigkeiten zu belasten.

Aufbauend auf dem Actor-Modell stellt Simulator X ein Event-System und eine Abstraktion globaler Zustandsvariablen zur Verfügung. 

Ein Event-System ist für interaktive 3D-Anwendungen sehr hilfreich, da auf eine Vielzahl von Ereignissen, die vor allem durch Eingabegeräte verursacht werden, reagiert werden muss.


Globale Zustandsvariablen, SVars genannt, vereinfachen für den Programmierer den Umgang mit verteilten Daten. Ein bestimmtes Datum wird von genau einem Actor, dem Besitzer verwaltet. Andere Actors besitzen nur eine spezielle Referenz auf den Wert und müssen mit bem Besitzer kommunizieren um den Wert auszulesen oder zu manipulieren.
Eine zugeordnete SVarDescription [#f1]_ benennt die SVar, gibt ihr einen Scala-Datentyp und definiert deren Semantik in einer Anwendung.

Zusammengehörige Referenzen auf Zustandsvariablen werden zur einfacheren Handhabung zu Entitäten zusammengefasst. Eine Entity beschreibt genau ein Simulationsobjekt [#f2]_ und dessen Daten. 

Simulator-X-Anwendungen sind aus Komponenten aufgebaut. Diese setzen auf dem Actormodell auf und kommunizieren miteinander über den Austausch von Nachrichten oder durch das Setzen von SVars in Entities.
Eine Komponente sollte möglichst eine genau abgegrenzte Funktionalität wie beispielsweise ein KI-Modul oder eine Grafikausgabeeinheit realisieren. 

Komponenten können Aspekte zu Entitäten hinzufügen. 

Bei der Erzeugung einer Entity können über einen Aspekt Werte durch den Benutzer vorgegeben werden, die für eine bestimmte Komponente bestimmt sind.


Die genutzte Version von Simulator X (Stand August 2011) bringt eine Anbindung an die Open-Source-Physikengine JBullet :cite:`www_jbullet` mit, die innerhalb des I>PM3D-Projekts für verschiedene Aufgaben wie die Selektion von Modellelementen und die Realisierung von Modellierungsebenen genutzt wird.

Die mitgelieferte Renderkomponente, die für die grafische Ausgabe zuständig ist, war für das vorliegende Projekt allerdings nicht sinnvoll nutzbar und wurde durch eine Anbindung an eine selbst entwickelte, ebenfalls OpenGL-basierte :ref:`render_bibliothek` ersetzt. 
Dies war durch den modularen Aufbau von Simulator X problemlos umsetzbar. 

.. _opengl:

OpenGL / LWJGL
**************

Um die Grafikausgabe des I>PM3D-Projektes zu realisieren wurde die plattformunabhängige 3D-Schnittstelle OpenGL :cite:`www_opengl` genutzt. Die :ref:`render_bibliothek` nutzt ausschließlich Funktionalitäten, die in Version 3.3 des OpenGL-Standards nicht als "deprecated" markiert sind. Die im Projekt von :cite:`uli` für Menüs genutzte Nifty-GUI-Bibliothek erfordert allerdings noch OpenGL in der Version 1.x. Somit muss die Anwendung in einem abwärtskompatiblen Grafikmodus gestartet werden.


Als Anbindung an OpenGL wird die Java-Spielebibliothek LWJGL :cite:`www_lwjgl` in der Version 2.8.2 eingesetzt. 
Zusätzlich stellt LWJGL eine Schnittstelle für den Zugriff Tastatur und Maus zur Verfügung.


Sonstiges
*********

StringTemplate
--------------

Um Prozessmodelle in einer textuellen Form speichern zu können wird die Template-Bibliothek *StringTemplate*, im Folgenden mit *ST* abgekürzt, in der Version 4.0.4 verwendet. :cite:`Parr:2009:LIP:1823613` 

ST folgt dem Prinzip, Templates als Text mit Platzhaltern zu definieren. Die Platzhalter werden durch das Setzen von Attributen aus dem Anwendungsprogramm heraus mit Inhalt gefüllt.

Um die Nutzung von ST in Scala zu vereinfachen wurde eine dünne Abstraktionsschicht in Scala implementiert. Diese Schicht sorgt unter Anderem dafür, dass beliebige Scala-Objekte als Java-Bean an ST weitergegeben werden können, auch wenn sie selbst nicht der Java-Bean-Konvention entsprechen.
Für Erstellung eines den Konventionen folgenden Wrapper-Objekts wird :cite:`www_clapper` genutzt.

Beispiel für ein Template, dass eine String-Zuweisung in LMM produziert:


.. code-block:: scala

    val assignTemplate = "<attribName> = \"<value>\""
    val assignST = ST(assignTemplate)
    assignST.addAll(
        "attribName" -> "functions",
        "value" -> "test")
    val output = assignST.render


Simplex3D-Math
--------------

Im gesamten I>PM3D-Projekt wird die in Scala implementierte Mathematikbibliothek *Simplex3D-Math* in der Version 1.3 :cite:`www_simplex3d` genutzt. Durch die Bibliothek werden Matrizen, Vektoren und dazugehörige Utility-Funktionen bereitgestellt. Deren API orientiert sich weitgehend an der OpenGL Shading Language.

SLF4J / Logback
---------------

Für die Aufzeichnung von Logging-Informationen wird die Java-Logging-API *SLF4J* :cite:`SLF4J` in der Version 1.6.4 mit Logback (1.0.0) als Implementierung eingesetzt. 
Um die Einbindung in Scala zu verbessern wurde ein eigener Wrapper für die SLF4J-API entwickelt.


.. [#f1] Beispiele für SVar-Typen: *Color*, *Transformation* oder *Mass*
.. [#f2] Dies könnte im Prozessmodelleditor beispielsweise ein Modellelement wie ein Prozess oder eine Kontrollflusskante sein.
