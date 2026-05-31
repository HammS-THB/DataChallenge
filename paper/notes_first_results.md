# Erste Ergebnisse zum Datencheck
## Aufgabentyp
Bei der Aufgabe handelt es sich um eine überwachte Mehrklassen Klassifikation. Ziel ist es, aus Smartphone-Sensordaten die Aktivität einer Person vorherzusagen.
Die gelabelte Datei `measures.csv` enthält die Zielvariable `activity`. Die Datei `to_predict.csv` enthält Beobachtungen ohne Aktivitätslabel, für die später mit dem finalen Modell Vorhersagen erzeugt werden sollen.

## Datenquelle und Datenstruktur
Der Datensatz basiert auf dem Human Activity Recognition Using Smartphones Dataset. Die Daten wurden mit einem Samsung Galaxy S II Smartphone erhoben, das von den Versuchspersonen an der Taille getragen wurde. Die ursprünglichen Signale stammen vom Beschleunigungssensor und vom Gyroskop.
Die Rohsignale wurden bereits vorverarbeitet und in Feature-Vektoren umgewandelt. Die Features stammen unter anderem aus Beschleunigungs-, Gyroskop-, Jerk-, Magnituden- und Frequenzbereichssignalen.

## Datenimport
Die CSV-Dateien verwenden Semikolon als Trennzeichen und Komma als Dezimalzeichen. Deshalb wurden die Daten mit folgenden Parametern geladen:

```python
measures = pd.read_csv(DATA_DIR / "measures.csv", sep=";", decimal=",")
to_predict = pd.read_csv(DATA_DIR / "to_predict.csv", sep=";", decimal=",")
```

Nach dem Import hatten die Datensätze folgende Dimensionen:

| Datensatz | Zeilen | Spalten | Beschreibung |
|---|---:|---:|---|
| `measures.csv` | 7352 | 563 | Gelabelte Daten mit Zielvariable `activity` |
| `to_predict.csv` | 2947 | 562 | Ungelabelte Daten ohne `activity` |

Damit besteht `measures.csv` aus 561 Feature-Spalten sowie den Spalten `subject` und `activity`. `to_predict.csv` enthält dieselben 561 Feature-Spalten sowie `subject`, aber keine Zielvariable.

## Fehlende Werte
Beide Datensätze wurden auf fehlende Werte geprüft. Es wurden keine fehlenden Werte gefunden.

| Datensatz | Fehlende Werte | Spalten mit fehlenden Werten |
|---|---:|---:|
| `measures.csv` | 0 | 0 |
| `to_predict.csv` | 0 | 0 |

Damit ist keine Imputation fehlender Werte notwendig.

## Klassenverteilung
Der gelabelte Datensatz enthält sechs Aktivitätsklassen.

| Aktivität | Anzahl | Anteil |
|---|---:|---:|
| `LAYING` | 1407 | 0.191 |
| `STANDING` | 1374 | 0.187 |
| `SITTING` | 1286 | 0.175 |
| `WALKING` | 1226 | 0.167 |
| `WALKING_UPSTAIRS` | 1073 | 0.146 |
| `WALKING_DOWNSTAIRS` | 986 | 0.134 |

Die Klassen sind nicht exakt gleich verteilt, aber es liegt keine extreme Klassenungleichheit vor. Die größte Klasse ist `LAYING`, die kleinste Klasse ist `WALKING_DOWNSTAIRS`.

## Subjects
Der gelabelte Datensatz enthält Beobachtungen von 21 Personen:

```text
1, 3, 5, 6, 7, 8, 11, 14, 15, 16, 17, 19, 21, 22, 23, 25, 26, 27, 28, 29, 30
```

Die laut Aufgabenstellung vorgeschriebenen Trainingspersonen `1`, `3`, `5` und `6` sind im Datensatz vorhanden. Auch die vorgeschriebenen Testpersonen `27`, `28`, `29` und `30` sind vorhanden.

## Messfenster pro Person

Die Anzahl der Messfenster pro Person liegt zwischen 281 und 409. Die Testpersonen enthalten folgende Anzahl an Beobachtungen:

| Subject | Anzahl Messfenster |
|---:|---:|
| 27 | 376 |
| 28 | 382 |
| 29 | 344 |
| 30 | 383 |

Damit umfasst das spätere Testset insgesamt 1485 Beobachtungen.