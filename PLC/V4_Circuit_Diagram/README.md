# myT40-PLC V2.9 M38 (Prototype-4)

Errata:

Prototype 4 (V2.9 M38)
==================================

2024-01-17	M38 Rev.2.6 OP+UP  (Veröffentlichung als V4 PFS 4-Layer)
			M38 Rev.2.6 OP+UP  (Veröffentlichung als V4 PFS )
			Neu U51:  Ugs-Pegelwandler 5V auf 3,3V für Q2 Gate (Q2 zur Abschaltung 3,3V Interfaces) (Wahrscheinlich nicht nötig, denn : -12V <= Ugs <= 12V)
			Es entfällt damit auch D50 (ZenerD 3,6V SOT23).
			Neu R501: R50 als einziger PullUp für RESET auf UP und für CPU auf auf OP über Steckverbindung, Solitärbetrieb OP nicht möglich
			daher ein zusätzlicher PullUp R501 auf OP neben Extention-Slot
2024-02-29	Beschriftungskorrektur FFC 4-Layer, Versionsdateum auf OP-TOP von 2023-01-17 auf 2024-01-17


Prototype 4.x (V2.9 M41) Beschreibung der Fehlerbeseitigungen
==================================

2024-06-09	M41 Rev.2.9 OP+UP Korrekturen für Prototyp-V4 zum Kaltstart: im ausgeschalteten Zustand kein Start möglich
			  -> U51 Spannungsversorgung korrigiert
			  -> R126 und R50 ersetzt durch 220 Ohm SMD 0805  (OP+UP)
			  -> C1, R128, R501, U48 (OP) entfallen
			  -> U49 OR-Gatter mit LevelShift-Variante ersetzt (OP)
			  -> J34 und J36 bringen 3,3V für PullUp an R126 für SW_MODE_2 (UP+OP)
			
Ausblick:
==================================

Prototype 5 (V3.0 M41) in Planung
			Diese hoffentlich letzte Version wird zusätzlich einen HW-Watchdog erhalten.
			Dieser ist deswegen nötig, falls das Programm auf dem Teensy selbigen in einen ungewollten Zustand treibt, warum auch immer.
