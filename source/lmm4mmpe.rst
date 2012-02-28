********
LMM4MMPE
********

Basic Facts
===========

* ist angelehnt an LMM von OMME 2 (Stand etwa Ende 2010)

* implementiert LMM Core

Unterstützte Modelelemente
==========================

* Model

* Level

* Package (mit Subpackages)

* Concept

* Attribute (Literal / Concept)

* Assignment (Literal / Concept)

* Enums vorgesehen, werden geparst und in AST umgesetzt aber weiter nicht unterstützt.


Komponenten
===========

ResourceUtils
-------------

von hier werden Parsing-Aktionen gestartet.

* loadModelFromFile

* loadModelFromResource

* loadModelFromString

LmmParser
---------

Der LmmParser ist mit der Scala-Parsercombinator-Library geschrieben, die Teil der Standardbibliothek von Scala ist.
Ein Parser wird darin als Komposition von Parser-Funktionen geschrieben, die an die Darstellung einer BNF-Grammatik erinnern.

Der Parser baut aus dem Eingabetext rekursiv die AST-Darstellung des Modells auf.

Ast
---

Abstrakter Syntaxbaum der Sprache, besteht aus immutable case classes, die gelesene Modellelemente repräsentieren.

Model-Access-API
================

Zum einfachen und schnellen Zugriff auf die Modelle gibt es für Modellelemente dazugehörige Access-Funktionsmodule mit einem impliziten Adapter, der Modellelemente kapselt und Aufrufe an das Access-Funktionsmodul weitergibt.

ModelAccess
-----------

* Zugriff auf Level über den Namen

LevelAccess
-----------

* Zugriff auf Packages über den Namen

PackageAccess
-------------

* Zugriff auf Concepts über den Namen


ConceptAccess
-------------

* bla


