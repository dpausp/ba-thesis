.. _anhang-dvd:

**************************************
Systemanforderungen und Inhalt der DVD
**************************************

Systemanforderungen von i>PM3D
==============================

Hardware
--------

* Grafikchip mit OpenGL 3.3-Unterstützung. Getestet mit Geforce 8400, 9400GS, AMD A-3850.
* mindestens 1,5 GByte freier Arbeitsspeicher
* 200MB Festplattenspeicher
* Minimale Auflösung 1024x768
* Optional: Microsoft Kinect, Nintendo WiiMote

Software
--------

* Betriebssystem Windows (getestet unter Windows 7) oder Linux (getestet unter Ubuntu Linux 11.10 und Gentoo Linux)
* Grafiktreiber mit Unterstützung für OpenGL 3.3
* Java Virtual Machine in einer Version ab 1.6
* (Scala und weitere Libraries sind in die ausführbare jar der Anwendung integriert)

DVD
===

Ausführbare Dateien
-------------------

Die DVD enthält eine ausführbare Version von i>PM3D. 
Für den Start der Anwendung werden .bat-Dateien (Windows) und sh-Dateien (Linux) für unterschiedliche Konfigurationen der Eingabegeräte angeboten.
Weitere Hinweise zur Ausführung des Programms werden auf der DVD in ``README.txt`` gegeben.
Außerdem ist der Collada2Scala-Compiler enthalten. Hinweise dazu befinden sich in ``README-collada2scala.txt``.

Eclipse-Projekte
----------------

Die DVD umfasst den vollständigen Quellcode von i>PM3D.

Die mitgelieferten Scala-Eclipse-Projekte teilen sich wie folgt auf:

 * MMPEEditor: Editorkomponente, Modellkomponente / Modellanbindung
 * MMPERenderer: Render-Bibliothek, Collada-Loader, Collada2Scala
 * MMPESirisAddons: Benötigte Erweiterungen für Simulator X: Renderkomponente, modifizierte Physikkomponente
 * MMPEUtils: Verschiedene Scala-Hilfsklassen und -objekte (Logging, Mathematik)
 * MMPEResources: verwendete Texturen, Shader und Collada-Dateien sowie Modelle
 * LMM4MMPE: LMMLight-Implementierung, Parser, M2T
 * ScalaST4: Anbindung von StringTemplate

(Rückfragen zur Ausführung des Programms oder zur Verwendung und Kompilierung der Projekte in Eclipse oder mit SBT bitte an Tobi.Stenzel@gmx.de)

Weitere Hinweise werden auf der DVD in ``README.txt`` gegeben.

Beilagen
--------

* Video BPMN-Prozess am Flughafen: ``brown-airport.flv``
* PDF (Anzeigeversion) der Bachelorarbeit: ``IPM3D-Tobias-Stenzel.pdf``
* PDF (Druckversion): ``IPM3D-Tobias-Stenzel-print.pdf``
* In der Arbeit gezeigte Screenshots von i>PM3D: ``screenshots``
* Diagramme (.dia-Format): ``diagrams``
* In der Arbeit gezeigte Abbildungen: ``ext-pics``

