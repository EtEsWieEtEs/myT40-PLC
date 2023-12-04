# myT40-PLC V2.6 M36 (V4)

Errata:

C51 wird ein Tantal-Elko mit 4µ7 aund 16V, das verbessert die Ausgangsspannung für die CPU

Current:

Aktuell ist die Kaltstart-Logik in Überprüfung. Es gibt einen Zustand ohne garantierte Funktion.

Findings:
Keine Pegelkonverter. Kein Platz.
Möglicherweise falsche Variante für OR-Gatter : U49 ist CMOS muss aber TTL werden.
U49 TTL-Variante , weil Input-V-High minimum 2V sein muss, die werden durch 3V3-Logik erreicht. Die CMOS-Version 74AHC1G32 erwartet jedoch 3,8V für High.
Minimale Ub für OR-Gate ist mit 4,5V noch kleiner als die Uth von U48 mit 4,63V (ADM-803L)
Vermutung: Es sollte ein 74AHCT1G32 verwendet werden.


Transienten in Versorgungsspannung von NE555 bei Schalten möglich: Daher Parallel (auf) C75 100nF einen Tantal-Elko 10V 10µ auflöten.
SMD1206 passt zur Tantalgröße 
https://www.reichelt.de/smd-tantal-10-f-10v-105-c-t520-10u-10-p206542.html?&trstct=pol_0&nbc=1


Nachmessen


Überprüfung ergab folgende Änderungen:

2023-11-17	M37 Rev.2.6 OP
			C51 statt 100nF nun Tantal 4µ7 16V, für sauberere Spannung 5V-CPU-final
2023-11-23	M37 Rev.2.6 OP
			C73 100nF neue Baugröße SMD 0603
			C75 100nF neue Baugröße SMD 0603
			C95 10µF 10V Tantal neuer Kondensator an NE555D, soll Transienten bei Schaltvorgängen reduzieren 
			C76, U50, R133 verschoben
			GNDD Verlauf optimiert

			Drei Merkwürdigkeiten gab es, die darauf schließen ließen, dass der Kaltstart nur funktioniert, solange alle I/O ordentlich parametriert sind.
			1) Das wieder anlaufende Programm erzeugt in der Setup-Routine einen RESET. Wenn SW_MODE_2=LOW ist, dann erfolgt ein erneuter Kaltstart, 
			   solange bis SW_MODE_=HIGH wird. Das ist soweit normal.
			2) Wenn die Versorgungsspannung von 5V für die CPU hinter dem MOSFET Q1 abgeschaltet ist, bleibt die CPU-5V-Leitung auf einer Spannung von 2V 
			   und geht nicht auf 0V runter. Wenn das CPU-Modul ausgebaut ist, erreicht die Abschaltung ihre 0V. 
			   Es liegt also der Schluss nahe, dass die 3,3V-Versorgung der Interface-Bausteine über diverse Signalleitungen eine Rückspeisung auf den 
			   5V-Pin des Teensy4.0-Moduls hervorruft, zumindest wenn dieses die Pin-Konfiguration durch Neustart verloren hat. 
			   Die 2V sind zudem belastbar: an einem 470 Ohm Widerstand fließen bis zu 3,9mA gegen GNDD ab.
			3) Es gibt den Zustand, dass die CPU, bislang noch aus ungeklärten Gründen (evt. zuviel Daten-Output auf dem USB-Progrmmier-Anschluss zu 
			   DEBUG-Zwecken, der nicht mehr abgerufen wird?) nach zwei oder drei Tagen, manchmal aber auch nach 4h, von selber in den Programmier-Modus fällt.
			   In diesem Fall blinkt die LED neben dem Micro-USB-Port. Ein Kaltstart ist dann nicht mehr möglich, weil die 2V an CPU-5V-Pin weiterhin anliegen.
			
			Lösung für 2+3: De 3,3V-Spannungsversorgung für die Interface-Bausteine muss zusammen mit der 5V für die CPU abgeschaltet werden. 
			Das Off-Intervall muss lange genug dauern, damit sich der Kondensator C1 und andere vollständig entladen können. 
			Daher ist auch die Zeitkonstante am NE555 zu verzehnfachen, also 5 Sekunden.
			Zu beachten ist, dass U48 auf der OP die 5V Leitung überwacht und auf RESET meldet. Auch die Kaltstartlogik arbeitet mit permanenten 5V.
			Daher muss das Steuersignal für einen weiteren MOSFET von der Kaltstart-Logik für den 3,3V Spannungsregler auf der UP vorgesehen werden.
			Es kann also nicht Q1 auf die UP verlagert werden, um 5V und die daraus nachgelagert erzeugte 3,3V in einem abzuschalten.
			Die Abschaltung muss den 3,3V-Regler abschalten, weil das Signal für das Gate 5V-basiert ist.
			Wegen der langen Leitung für das Gate sollte ein Tiefpass Störsignale entfernen (C96 und ein noch zu bestimmernder R).
			
			R133 = 470k für 5 Sekunden, 220k für 2,5 Sekunden
			
			Neue Bauteile:
			R169 (0..100 Ohm) koppelt NE555-Ausgang Q (3V3EN.Q2.UP, bzw. Potential COLDSTART_3V3INT) parallel zu Q1 auf OP (5V_CPU) aus und legt ihn an J4-Pin-8 für UP an.
		M37 UP
			Q2 (IRLML-2244) erhhält das NE555-Ausgangssignal über J19-Pin-8 und eine Brücke Br (J34/J36).
			R170 10k der obligatorische Gate-Source-Widerstand.
			C96 100n filtert HF aus, zusammen mit Br ist Tiefpass möglich, wenn statt 0 Ohm ein Widerstand bis 100 Ohme eingesetzt wird.
			C97 100n Eingangskondensator für U13, Spannungsregler 3V3

U49 verhält sich genau so, wie er soll. Es kann daher die CMOS-Version weiterhin verwendet werden.

"V2.6 M37" - Schaltplan und PCB Neu