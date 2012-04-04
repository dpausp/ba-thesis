******************
Fazit und Ausblick
******************

In der vorliegenden Arbeit wurde die Einbindung von Prozessmodellen und deren Visualisierung im i>PM3D-Prototypen sowie die Spezifikation der verwendeten Modellierungssprache durch Metamodelle vorgestellt.

Aufgrund der Komplexität dieses Themas, der dabei auftretenden Fragestellungen und der insgesamt noch eher schwach ausgeprägten Forschungslage zum Thema 3D-Visualisierung von Prozessen kann dies jedoch nur ein Anfang sein. 

Probleme und naheliegende Erweiterungsmöglichkeiten, welche speziell die Visualisierung betreffen, wurden schon im :ref:`Kapitel zur Visualisierung <vis-probleme-erweiterung>` besprochen.
Besonders wichtig wären in diesem Zusammenhang umfangreichere Konfigurationsmöglichkeiten, die nach Möglichkeit auch in das Metamodell aufgenommen werden sollten, um eine einheitliche Konfiguration der Visualisierungsparameter zu ermöglichen. 
Außerdem sollte über automatische Verfahren zur Unterstützung des Benutzers, beispielsweise Layout-Algorithmen und eine intelligente Wahl der Lichtparameter nachgedacht werden.

Daneben könnte untersucht werden, inwieweit sich geometriebasierte Ansätze, wia bspw. aus i>PM:sup:`2` bekannt, in den dreidimensionalen Raum übertragen lassen.

In Zukunft wäre es besonders wichtig, die Effektivität von 3D-Visualisierungsansätzen genauer zu untersuchen. 
Dabei sollten auch Benutzer eingebunden werden, die sich zwar mit dem Thema Prozessmodellierung auskennen, jedoch noch kaum Erfahrung im Umgang mit 3D-Umgebungen haben.

Eingabe und Benutzerinteraktion wurden in dieser Arbeit größtenteils ausgeklammert, da diese von :cite:`uli` und :cite:`buchi` im Rahmen des Projekts bearbeitet wurden. 
Dies ist aber eigentlich ein "untrennbarer" Bestandteil, wenn man die Effektivität von 3D-Visualisierungen – insbesondere in Modellierungswerkzeugen – bewerten will.

Bei eigenen Versuchen mit dem Prototypen hat sich besonders gezeigt, dass die Effektivität mit dem Vorhandensein gut funktionierender und intuitiver Eingabemethoden steht und fällt. Daher ist es besonders lohnenswert, zukünftige Arbeiten in diese Richtung zu lenken. 

Da am Lehrstuhl, an dem dieses Projekt entstanden ist, ebenfalls die umfangreiche Metamodellierungsumgebung OMME entwickelt wird, wäre eine Integration von I>PM3D in diese überlegenswert. 
Für die Realisierung des Prototypen wurden schon Prinzipien aus OMME wie die Austauschbarkeit und Separation der Metamodelle für Editor und Modellierungsdomäne sowie die Spezialisierung von Instanzen genutzt. 
Um die Flexibilität weiter zu erhöhen sollte nach dem Vorbild von MDF die Spezifikation der grafischen Repräsentation vom Editor-Modell getrennt werden.

Vielversprechend könnten auch Kombinationen von 3D und 2D-Visualisierungen sein, um die Vorteile aus beiden "Welten" nutzen zu können. 
Dies könnte durch die Integration von 2D-Elementen in I>PM3D realisiert werden. 
Außerdem wäre es technisch möglich, i>PM3D auf Basis von *Simulator X* in die Eclipse-Plattform zu integrieren. 
Damit könnten 3D-Editoren neben mit MDF spezifizierten 2D-Editoren in derselben Umgebung genutzt werden.

Der Anpassbakeit der Visualisierung und der Modellierungskonstrukte, die durch die Verwendung des Metamodellierungs-Ansatzes entsteht, muss auch eine entsprechende Flexibilität der Implementierung gegenüberstehen. 

In der Modellanbindung wurde diese noch nicht ganz erreicht, da durch diese bisher eine feste Auswahl an Visualisierungsparametern vorgegeben wird, um die Implementierung zu erleichtern. 
Dies könnte – möglicherweise auch mit Änderungen in künftigen Versionen von *Simulator X* – wohl flexibler realisiert werden.
*Simulator X* war aber grundsätzlich für die Realisierung des Projekts gut geeignet; besonders die Entkoppelung von Funktionalitäten durch die Komponentenaufteilung und das *Entity / SVar*-Konzept, welche für die Bereitstellung der Modellfunktionalitäten genutzt wurden haben sich als positiv herausgestellt.

Durch die Render-Bibliothek, die im Rahmen dieser Arbeit entwickelt wird, ist eine Integration von weiteren Figuren, wie in einem Anwendungsbeispiel im letzten Kapitel gezeigt wurde leicht möglich. 
Diese könnten auch speziell auf eine 2D-Darstellung ausgelegt sein, um einen 2D-Editor mit i>PM3d zu realisieren, wobei hier noch einige Änderungen an der restlichen Implementierung vorgenommen werden müssten.
Allgemein hat sich die Render-Bibliothek durch ihre Erweiterbarkeit und der leichten Einbindung in das Anwendungsprogramm als sehr praktisch für die Umsetzung herausgestellt.
Mit jener sollten sich auch andere 3D-Anwendungen gut umsetzen lassen, ohne die "Freiheiten" des Programmierers einzuschränken, möglicherweise auch auf Basis von Simulator X über die hier entwickelte Integration über die Renderkomponente.

Insgesamt lässt sich zum Projekt sagen, dass ein durchaus benutzbarer Prototyp eines 3D-Prozessmodellierungswerkzeugs entstanden ist, der als Basis für weitere Entwicklungen dienen kann (und sollte). Prinzipiell lässt sich i>PM 3D auch schon für die Visualisierung und Bearbeitung von Modellen nutzen, die sich ebenfalls in einer graphbasierten Form darstellen lassen, wenn die Metamodelle entsprechend angepasst werden, beispielsweise für die Modellierung von Proteinen oder Reaktionsnetzwerken in der Bioinformatik.
