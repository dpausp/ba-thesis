******************
Fazit und Ausblick
******************

In der vorliegenden Arbeit wurde die Einbindung von Prozessmodellen und deren Visualisierung im i>PM3D-Prototypen sowie die Spezifikation der verwendeten Modellierungssprache durch Metamodelle vorgestellt.

Aufgrund der Komplexität dieses Themas, der dabei auftretenden Fragestellungen und der insgesamt noch eher schwach ausgeprägten Forschungslage zum Thema 3D-Visualisierung von Prozessen kann dies jedoch nur ein Anfang sein. 
In Zukunft wäre es wichtig, die Effektivität von 3D-Visualisierungsansätzen genauer zu untersuchen. 
Dabei sollten auch Benutzer eingebunden werden, die sich zwar mit dem Thema Prozessmodellierung auskennen, jedoch noch kaum Erfahrung im Umgang mit 3D-Umgebungen haben.

Eingabe und Benutzerinteraktion wurden in dieser Arbeit größtenteils ausgeklammert, da diese von :cite:`uli` und :cite:`buchi` im Rahmen des Projekts bearbeitet wurden. 
Dies ist aber eigentlich ein "untrennbarer" Bestandteil, wenn man die Effektivität von 3D-Visualisierungen – insbesondere in Modellierungswerkzeugen – bewerten will.

Bei eigenen Versuchen mit dem Prototypen hat sich besonders gezeigt, dass die Effektivität mit dem Vorhandensein gut funktionierender und intuitiver Eingabemethoden steht und fällt. 
Daher ist es besonders lohnenswert, zukünftige Arbeiten in diese Richtung zu lenken. 

Probleme und naheliegende Erweiterungsmöglichkeiten, welche speziell die Visualisierung betreffen, wurden schon im :ref:`Kapitel zur Visualisierung <vis-probleme-erweiterung>` besprochen.
Besonders wichtig wären in diesem Zusammenhang umfangreichere Konfigurationsmöglichkeiten, die nach Möglichkeit auch in das Metamodell aufgenommen werden sollten, um eine einheitliche Konfiguration der Visualisierungsparameter zu ermöglichen. 
Außerdem sollte über automatische Verfahren zur Unterstützung des Benutzers, beispielsweise Layout-Algorithmen und eine intelligente Wahl der Lichtparameter nachgedacht werden.
Daneben könnte untersucht werden, inwieweit sich geometriebasierte Ansätze, wia bspw. aus i>PM:sup:`2` bekannt, in den dreidimensionalen Raum übertragen lassen.

Einige bei der Vorstellung von verwandten Arbeiten zur Visualisierung gezeigten Nutzungsmöglichkeiten der dritten Dimension können mit dem Prototypen schon realisiert werden. 
Elemente nach ihrer "Wichtigkeit" oder nach anderen Kriterien im Vordergrund oder Hintergrund der 3D-Szene zu zeigen (:ref:`siehe <gogolla>`) ist aufgrund der freien Platzierbarkeit der Elemente möglich. 
Jedoch wäre hierfür auch eine algorithmische Unterstützung hilfreich, die beispielsweise anhand von Attributwerten aus dem Prozessmodell Elemente in einer bestimmten Weise anordnet.

Im Prototypen lassen sich gleichzeitig mehrere Modelle darstellen. 
So ist es prinzipiell möglich, hierarchische Modelle mit mehreren Verfeinerungsstufen (:ref:`siehe <betz>`) darzustellen, wie es für die Darstellung von "kompositen" Prozessen benötigt wird. 
Jede Stufe könnte so durch ein Modell repräsentiert werden, dessen Modellelemente sich auf einer :ref:`Modellierungsfläche <modellierungsflaechen>` befinden. 
Mehrere Modellierungsflächen können versetzt im Raum dargestellt werden.

Es fehlt allerdings noch entsprechende Unterstützung, um Beziehungen zwischen mehreren Modellen darzustellen und abzuspeichern. 
Außerdem wäre es sehr hilfreich, beispielsweise durch einen Doppelklick auf einen kompositen Prozessknoten das zugehörige Verfeinerungsmodell in derselben Szene anzeigen und wieder ausblenden zu können.

Verschiedene Modelltypen anzeigen und verknüpfen zu können, wäre wohl eine weitere lohnende Entwicklungsmöglichkeit. 
Dazu müsste die gleichzeitige Verwendung von mehreren Metamodellen durch die Editorkomponente unterstützt werden (:ref:`siehe<laden-metamodelle>`).

Da am Lehrstuhl, an dem dieses Projekt entstanden ist, ebenfalls die umfangreiche Metamodellierungsumgebung OMME entwickelt wird, wäre eine Integration von I>PM3D in diese überlegenswert. 
Für die Realisierung des Prototypen wurden schon Prinzipien aus OMME wie die Austauschbarkeit und Separation der Metamodelle für Editor und Modellierungsdomäne sowie die Spezialisierung von Instanzen genutzt. 
Um die Flexibilität weiter zu erhöhen sollte nach dem Vorbild von MDF die Spezifikation der grafischen Repräsentation vom Editor-Modell getrennt werden.

Vielversprechend könnten auch Kombinationen von 3D und 2D-Visualisierungen sein, um die Vorteile aus beiden "Welten" nutzen zu können. 
Dies könnte durch die Integration von 2D-Elementen in I>PM3D realisiert werden. 
Außerdem wäre es technisch möglich, i>PM3D auf Basis von *Simulator X* in die Eclipse-Plattform zu integrieren. 
Damit könnten 3D-Editoren neben mit MDF spezifizierten 2D-Editoren in derselben Umgebung genutzt werden.

Der Anpassbarkeit der Visualisierung und der Modellierungskonstrukte, die durch die Verwendung des Metamodellierungs-Ansatzes entsteht, muss auch eine entsprechende Flexibilität der Implementierung gegenüberstehen. 

In der Modellanbindung wurde diese noch nicht ganz erreicht, da durch diese bisher eine feste Auswahl an Visualisierungsparametern vorgegeben wird, um die Implementierung zu erleichtern. 
Dies könnte – möglicherweise auch mit Änderungen in künftigen Versionen von *Simulator X* – wohl flexibler realisiert werden.
*Simulator X* war aber grundsätzlich für die Realisierung des Projekts gut geeignet; besonders die Entkoppelung von Funktionalitäten durch die Komponentenaufteilung und das *Entity / SVar*-Konzept, welche für die Bereitstellung der Modellfunktionalitäten genutzt wurden haben sich als positiv herausgestellt.

Durch die Render-Bibliothek, die im Rahmen dieser Arbeit entwickelt wird, ist eine Integration von weiteren Figuren, wie in einem Anwendungsbeispiel im letzten Kapitel gezeigt wurde leicht möglich. 
Diese könnten auch speziell auf eine 2D-Darstellung ausgelegt sein, um einen 2D-Editor mit i>PM3D zu realisieren, wobei hier noch einige Änderungen an der restlichen Implementierung vorgenommen werden müssten.
Allgemein hat sich die Render-Bibliothek durch ihre Erweiterbarkeit und der leichten Einbindung in das Anwendungsprogramm als sehr praktisch für die Umsetzung herausgestellt.
Mit jener sollten sich auch andere 3D-Anwendungen gut umsetzen lassen, ohne die "Freiheiten" des Programmierers einzuschränken, möglicherweise auch auf Basis von Simulator X über die hier entwickelte Integration über die Renderkomponente.

Insgesamt lässt sich zum Projekt sagen, dass ein durchaus benutzbarer Prototyp eines 3D-Prozessmodellierungswerkzeugs entstanden ist, der als Basis für weitere Entwicklungen dienen kann (und sollte). Prinzipiell lässt sich i>PM 3D auch schon für die Visualisierung und Bearbeitung von Modellen nutzen, die sich ebenfalls in einer graphbasierten Form darstellen lassen, wenn die Metamodelle entsprechend angepasst werden, beispielsweise für die Modellierung von Proteinen oder Reaktionsnetzwerken in der Bioinformatik.
