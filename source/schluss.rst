******************
Fazit und Ausblick
******************

In der vorliegenden Arbeit wurde ein Konzept zur Visualisierung von Prozessen im dreidimensionalen Raum sowie eine Implementierung im Rahmen des I>PM3D-Prototypen erstellt.
Aufgrund der Komplexität dieses Themas, der dabei auftretenden Fragestellungen und der insgesamt noch recht schwach ausgeprägten Forschungslage kann dies jedoch nur ein Anfang sein. 

In Zukunft wäre es besonders wichtig, die Effektivität von solchen Visualisierungen genauer zu untersuchen. Dabei sollten auch Benutzer eingebunden werden, die sich zwar mit dem Thema Prozessmodellierung auskennen, jedoch noch kaum Erfahrung im Umgang mit 3D-Umgebungen haben.

Bei eigenen Versuchen mit dem Prototypen hat sich besonders gezeigt, dass die Effektivität mit dem Vorhandensein gut funktionierender und intuitiver Eingabemethoden steht und fällt. Daher ist es besonders lohnenswert, zukünftige Arbeiten in diese Richtung zu lenken.

Da am Lehrstuhl, an dem dieses Projekt entstanden ist, ebenfalls leistungsfähige Metamodellierungsumgebung OMME entwickelt wird wäre eine weiterführende Integration von I>PM3D mit dieser naheliegend. Für die Realisierung des Prototypen wurden schon Prinzipien wie die Austauschbarkeit und Separation der Metamodelle für Editor und Modellierungsdomäne sowie die Fähigkeit zur Instanz-Spezialisierung genutzt. Um die Flexibilität weiter zu erhöhen sollte nach dem Vorbild von MDF die Spezifikation der grafischen Repräsentation vom Editor-Modell getrennt werden, welches dann hauptsächlich nur noch die Verbindung zwischen grafischen Objekten und Domänenkonzepten herstellt.

Außerdem könnte untersucht werden, inwieweit sich geometriebasierte Ansätze wie aus IPM2 bekannt in den dreidimensionalen Raum übertragen lassen.

Vielversprechend sind auch Kombinationen von 3D und 2D-Visualisierungen um die Vorteile aus beiden "Welten" nutzen zu können. Dies könnte durch die Integration von 2D-Elementen in I>PM3D realisiert werden. Andererseits wäre es technisch möglich, den Prototypen auf Basis von Simulator X in die Eclipse-Plattform realisieren. Damit könnten 3D-Editoren neben mit dem MDF spezifizierten 2D-Editoren in derselben Umgebung genutzt werden.

Zur Verbesserung des visuellen Eindrucks und der Verständlichkeit von 3D-Modellen können noch viele weitere Techniken aus der Computergrafik wie die Stereoskopie, Schatten oder dynamische Transparenz eingesetzt werden (-> Ref).

Der Prototyp wurde bisher auf herkömmlichen Desktop-Systemen sowie auf Projektoren mit üblicher Auflösung (Wert?) getestet. Besonders für die Visualisierung von großen Modellen könnten weitere Versuche mit hochauflösendenen Projektenoder sogar CAVE-Systemen sinnvoll sein. 

Im Rahmen dieser Arbeit sind auch von I>PM3D unabhängig eine moderne und flexible 3D-Renderbibliothek, ein COLLADA-Loader und eine Anbindung der StringTemplate-Bibliothek für Scala entstanden, die in anderen Projekten eingesetzt werden können.
