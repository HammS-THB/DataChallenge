# Planung und Vorgehensweise

## Ziel der Analyse

Ziel der Analyse ist es, ein Modell zu entwickeln, das aus Smartphone-Sensordaten die Aktivität einer Person vorhersagt. Die Zielvariable ist `activity`. Da im Datensatz `measures.csv` die korrekten Aktivitätslabels bekannt sind, handelt es sich um eine überwachte Mehrklassen-Klassifikation.

Das finale Modell soll anschließend auf den ungelabelten Datensatz `to_predict.csv` angewendet werden. Die erzeugten Vorhersagen werden später in der Datei `my_prediction.csv` gespeichert.

## Grundidee

Die Analyse soll nicht nur ein möglichst gutes Modell erzeugen, sondern auch eine nachvollziehbare Einschätzung der Modellleistung liefern. Deshalb wird der Datensatz zunächst untersucht, anschließend nach Personen in Trainings- und Testdaten getrennt und danach mit verschiedenen Klassifikationsmodellen gearbeitet.

Besonders wichtig ist, dass Trainings- und Testdaten keine Personen gemeinsam enthalten. Dadurch wird realistischer geprüft, ob das Modell auf bisher unbekannte Personen generalisieren kann.

## Geplante Arbeitsschritte

| Nr. | Arbeitsschritt | Status |
|---:|---|---|
| 1 | Daten laden und Struktur verstehen | Abgeschlossen |
| 2 | Datenqualität prüfen | Abgeschlossen |
| 3 | Klassenverteilung und Subjects untersuchen | Abgeschlossen |
| 4 | Train/Test-Split nach Subjects erstellen | Offen |
| 5 | Explorative Analyse und erste Visualisierungen durchführen | Offen |
| 6 | Features und Zielvariable festlegen | Offen |
| 7 | Mehrere Klassifikationsmodelle vergleichen | Offen |
| 8 | Modellleistung mit Cross-Validation schätzen | Offen |
| 9 | Bestes Modell auf dem festgelegten Testset bewerten | Offen |
| 10 | Finale Vorhersagen für `to_predict.csv` erzeugen | Offen |
| 11 | Ergebnisse im wissenschaftlichen Paper dokumentieren | Begonnen |

## Datenverständnis

Zuerst werden die vorhandenen Dateien betrachtet. Die Datei `measures.csv` enthält die gelabelten Daten mit Smartphone-Features, einer Subject-ID und der Zielvariable `activity`. Die Datei `to_predict.csv` enthält dieselben Features und eine Subject-ID, aber keine Zielvariable.

Zusätzlich liefern `README.txt` und `features_info.txt` Informationen zur Herkunft und Bedeutung der Daten. Daraus geht hervor, dass die Features aus Beschleunigungs- und Gyroskopsignalen berechnet wurden. Die Daten sind bereits vorverarbeitet und enthalten keine Rohsignale, sondern abgeleitete statistische Merkmale.

## Datenqualität

Im nächsten Schritt wird geprüft, ob die Daten vollständig und plausibel sind. Dazu gehören insbesondere:

- Anzahl der Beobachtungen und Spalten
- vorhandene Aktivitätsklassen
- vorhandene Subjects
- fehlende Werte
- auffällige Klassenungleichgewichte
- Anzahl der Messfenster pro Person

Diese Prüfung ist wichtig, weil sie entscheidet, ob vor dem Modelltraining zusätzliche Bereinigungsschritte notwendig sind. Bisher wurde festgestellt, dass weder in `measures.csv` noch in `to_predict.csv` fehlende Werte vorhanden sind.

## Klassenverteilung

Die Verteilung der Zielvariable `activity` wird untersucht, um zu prüfen, ob alle Klassen ausreichend vertreten sind. Die sechs Klassen sind nicht exakt gleich häufig, aber es liegt keine extreme Klassenungleichheit vor.

Diese Information ist wichtig für die spätere Modellbewertung. Wenn eine Klasse sehr häufig wäre, könnte ein Modell durch häufiges Vorhersagen dieser Klasse eine scheinbar hohe Genauigkeit erreichen. Da die Klassen hier nur moderat ungleich verteilt sind, kann Accuracy als erste Bewertungsmetrik verwendet werden. Zusätzlich sollen später aber auch eine Confusion Matrix und klassenweise Kennzahlen betrachtet werden.

## Subject-basierter Train/Test-Split

Der Train/Test-Split wird nach Subjects durchgeführt und nicht zufällig auf Zeilenebene. Das ist wichtig, weil mehrere Messfenster von derselben Person stammen. Würden Messungen derselben Person sowohl im Training als auch im Test vorkommen, könnte die Modellleistung zu optimistisch eingeschätzt werden.

Laut Aufgabenstellung sollen die Subjects `27`, `28`, `29` und `30` als Testset verwendet werden. Alle übrigen gelabelten Subjects werden für Training und Modellwahl genutzt. Die vorgeschriebenen Trainingspersonen `1`, `3`, `5` und `6` sind im Trainingsbereich enthalten.

Das spätere Testset besteht aus 1485 Beobachtungen.

## Explorative Analyse

Für die explorative Analyse sollen einfache und aussagekräftige Visualisierungen erstellt werden. Geplant sind insbesondere:

- eine Darstellung der Klassenverteilung im gesamten gelabelten Datensatz
- ein Vergleich der Klassenverteilung zwischen Trainings- und Testset
- eine Darstellung der Anzahl an Messfenstern pro Subject
- optional: eine Darstellung wichtiger Features oder Unterschiede zwischen Aktivitäten

Die Visualisierungen sollen helfen, die Datenstruktur besser zu verstehen und zentrale Eigenschaften des Datensatzes im Paper nachvollziehbar darzustellen.

## Feature-Auswahl und Zielvariable

Die Zielvariable ist `activity`. Die Spalte `subject` wird für die Gruppierung und den Train/Test-Split verwendet, aber nicht als normales Modellfeature genutzt.

Als Modellfeatures werden die 561 berechneten Sensormerkmale verwendet. Diese beschreiben statistische Eigenschaften der Bewegungsdaten im Zeit- und Frequenzbereich.

Da die Features laut Datensatzbeschreibung bereits normalisiert und auf einen Wertebereich begrenzt sind, ist keine grundlegende Skalierung zwingend notwendig. Für bestimmte Modelle kann Skalierung jedoch trotzdem sinnvoll sein und wird bei Bedarf innerhalb einer Modellpipeline berücksichtigt.

## Modellvergleich

Für die Modellierung sollen mehrere Klassifikationsverfahren verglichen werden. Geplant sind mindestens zwei unterschiedliche Modelltypen, zum Beispiel:

- ein lineares Baseline-Modell
- ein Random-Forest-Modell
- eine Support Vector Machine
- optional ein k-Nearest-Neighbors-Modell oder ein weiteres Ensemble-Verfahren

Der Vergleich mehrerer Modelle ist wichtig, um die finale Modellwahl nicht willkürlich zu treffen. Die Modelle werden anhand ihrer geschätzten Vorhersageleistung und ihrer praktischen Eignung verglichen.

## Cross-Validation

Die Modellwahl soll mit Cross-Validation auf dem Trainingsdatensatz erfolgen. Dabei ist eine gruppierte Cross-Validation nach `subject` sinnvoll. Dadurch wird verhindert, dass Messungen derselben Person gleichzeitig in Trainings- und Validierungsdaten vorkommen.

Dieses Vorgehen liefert eine realistischere Einschätzung der Modellleistung auf unbekannten Personen als eine zufällige Cross-Validation auf einzelnen Zeilen.

Als Bewertungsgröße wird zunächst Accuracy verwendet. Zusätzlich wird die Fehlerrate betrachtet, also `1 - Accuracy`. Die Rechenzeit der Modelle kann ebenfalls dokumentiert werden.

## Bewertung auf dem Testset

Nach dem Modellvergleich wird das beste Modell ausgewählt und auf dem Trainingsdatensatz trainiert. Anschließend wird es auf dem festgelegten Testset mit den Subjects `27`, `28`, `29` und `30` bewertet.

Die Testauswertung liefert die wichtigste Schätzung der erwarteten Modellleistung. Berichtet werden sollen insbesondere:

- Test Accuracy
- Test Error
- Confusion Matrix
- mögliche Verwechslungen zwischen bestimmten Aktivitäten

Die Confusion Matrix ist besonders wichtig, um zu erkennen, ob bestimmte Aktivitäten systematisch verwechselt werden, zum Beispiel `SITTING` und `STANDING` oder `WALKING_UPSTAIRS` und `WALKING_DOWNSTAIRS`.

## Finales Modell und Vorhersage

Nach der Testbewertung wird ein finales Modell auf allen gelabelten Daten aus `measures.csv` trainiert. Dadurch nutzt das Modell die gesamte verfügbare Information.

Dieses finale Modell wird anschließend auf `to_predict.csv` angewendet. Die Vorhersagen werden in der Datei `my_prediction.csv` gespeichert. Die Reihenfolge der Beobachtungen muss dabei unverändert bleiben, damit jede Vorhersage zur richtigen Zeile aus `to_predict.csv` gehört.

## Dokumentation im Paper

Die Analyse soll in einem wissenschaftlichen Paper dokumentiert werden. Das Paper soll die wichtigsten Schritte nachvollziehbar beschreiben:

- Motivation und Aufgabenstellung
- Herkunft und Struktur der Daten
- Datenqualität und explorative Analyse
- Train/Test-Split
- Modellvergleich und Modellwahl
- Testfehler und Interpretation
- finale Vorhersage
- Grenzen der Analyse

Besonders wichtig ist, dass das Paper anonymisiert ist und keine Hinweise auf die Autorinnen oder Autoren enthält.

## Mögliche Risiken und Grenzen

Ein mögliches Risiko besteht darin, dass die Modellleistung auf dem Testset von den konkreten Testpersonen abhängt. Da das Testset nur aus vier Personen besteht, kann die geschätzte Leistung schwanken.

Außerdem könnten bestimmte Aktivitäten schwer voneinander zu unterscheiden sein, insbesondere wenn sie ähnliche Körperhaltungen oder Bewegungsmuster erzeugen. Beispiele dafür sind `SITTING` und `STANDING` oder die verschiedenen Gehaktivitäten.

Ein weiterer Punkt ist, dass die Daten bereits stark vorverarbeitet wurden. Dadurch kann zwar direkt mit den Features modelliert werden, aber eine Analyse der ursprünglichen Rohsignale ist nicht möglich.

## Zusammenfassung

Die geplante Analyse folgt einem strukturierten supervised-learning-Workflow. Zuerst werden Datenstruktur und Datenqualität geprüft. Danach wird ein subject-basierter Train/Test-Split erstellt. Anschließend werden mehrere Modelle mit Cross-Validation verglichen. Das beste Modell wird auf den vorgegebenen Testpersonen bewertet. Abschließend wird ein finales Modell auf allen gelabelten Daten trainiert und für die Vorhersage der ungelabelten Daten verwendet.