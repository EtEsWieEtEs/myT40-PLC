# myT40-PLC Rev3.3 M44 V5

Die Version-5 (Rev3.3 M44) ist fertig.

Folgende Anpassungen wurden am Prototyp-4 vorgenommen, Ergebnis ist nun die Version-5:
			
2024-02-29	Beschriftungskorrektur FFC 4-Layer, Versionsdatum auf OP-TOP von 2023-01-17 auf 2024-01-17

2024-05-09	M40 Rev.2.8 FFC-4Layer, OP+UP Korrekturen V4 zum Kaltstart: im ausgeschalteten Zustand kein Start möglich ((DOKU FÜR ANPASSUNG VON M38-Platinen))
			- UP: U51 Verpolung von Ub an Pin-3 und Pin-5: Ist= Pin-5:GNDD 0V, Pin-3:Vcc 5V, Soll: Pin-5:Vcc 5V, Pin-3:GNDD, 2 Platinenunterbrechungen und zwei Brücken
			  Man sollte Einzelgatter nicht einfach drehen, und erwarten, dass die Spannungsversorgungsanschlüsse noch die gleiche Orientierung haben...
			- OP: Spannungsteiler R126 und R128 arbeiten nicht im Falle des Systemstarts. Off-Widerstand ist 4080 Ohm von Teensy-Pin-3 (SW_MODE_2)
			  - R126 10k ersetzt durch 220 Ohm SMD 0805  (OP)
			  - R128 16k ersetzt durch Zener-Diode ZF 3,0 Baugröße SOD80(minimelf) ((BZY55B3V0: SMD 0805 zu teuer, wenn man sie als einzige bei mouser bestellt))
			  Damit ist die UinHigh am ODER-Gatter auch größer als 2,3V bei eingesetztem Teensy40
			- RESET ähnlich: RESET=High wird nicht erzeugt, wenn Steuerung startet. Grund: R501||R50=4k76 und Innenwiderstand µC am Teensy40-Pin-4 beträgt 1,341V bei 3,3V Ub PullUp ca. 3258 Ohm. 
			  Damit ist UresetHigh=1,34V zu klein für ODER-Gatter (UinHighMin=2,1V), NE555 wird daher nicht getriggert.
			  - R501 OP) entfällt , da PullUp zu 3V3_INTERFACE , abgeschaltet durch Q2, daher unsinning
			  - R50  10k (UP) ersetzt durch 220 Ohm SMD-0805 , PullUp zu 3V3, erzeugt ein UinHigh am ODER-Gatter von 2,1V mit eingesetztem Teensy40
			  OP: U49 Mit Prototyp-4 muss hier ein Voltage-Translator verwendet werden, Ub=5V, UinA/B 3,3V Level : SN74LV1T32DBVR oder SN74LV1T32DBVRG4
			  UP: Darstellung Modell-Platinen-Aufdruck D51 gedreht, Anode jetzt auf Ub, 
			      D-Symbole in Bedruckung eingezeichnet.
			  OP: Darstellung Modell J3 korrekt gedreht
2024-06-30	Da die CPU beim Start weit über 100mA zieht, fällt die Versorgungsspannung auf 3,4V und U48 löst einen RESET=Low aus. 
			Im Falle von SW_MODE_2=Low für einen Kaltstart beduetet das eine Endlosschleife vor dem Start der CPU. U48 entfällt.
			Aus dem gleichen Grund wird C1, Glättung auf der 3,3VInterface-Schiene entfernt, 100µF sind zu groß.
			C1 bleibt erst einmal ohne Bestückung und damit optional.
	
2024-06-09	M41 Rev.2.9 FFC-4-Layer: OP+UP Verbesserung und Korrekturen von M40 eingebaut: 
			3V3 kommt über weiteren Pin (J34/J36) von UP nach OP, statt aus örtlich permanent vorhandener 5V_CPU_IN mittels Zenerdiode 3V0
			- Damit entfällt die 3V-Zener-Diode D50 von Rev_2.8_M40
			- Der Widerstand R126 wird von 2k7 auf 1k oder 220 Ohm geändert.
			
2024-08-03	M42	Rev.3.0 FFC-4-Layer: OP
			Hardware Watchdog hinzugefügt TPS3431, PullUp Diode D50 nötig, da TPS3431 einen open collector hat, muss PushPull des U49 beschnitten werden.
			Prog und OnOff auf Switches SW4 und SW5 gelegt, am Platinenrand von außen erreichbar
2024-06-10	SMD Testpoints TP1-4 für LED1(WDin), 3V3, TR, Q



2024-11-7	M43	Rev.3.1 FFC-4-Layer: OP
			Aus M39 RS485-Beschaltung übernommen: RS485 Belegung identisch zum Original, CAN1+RS485 auf J7, CAN1+CAN2 auf J6
			Jumper JP6 entscheidet ob auf J7-Pin-2 GND von CAN oder RS485 anliegt, Jumper erreichbar im Schacht
			SW2 von CAN2 und Busabschluss-Dioden RS485 haben Positionen getauscht.
			Leiterbahnführungen überarbeitet, Lage der GND-Potentiale auf den den Signalen gegenüberliegenden Layern korrigiert
			Kompatibilität zu M42 bleibt erhalten
			
2025-04-06	M43 Konvertierung nach KiCad6 und KiCad7
			UP von HomeTeensy40_PFS-4_3.0_M42, kompatibel zu OP M38 und OP M43
			OP von HomeTeensy40_FFC-4_3.0_M42-RS485-Sond als HomeTeensy40_FFC-4_3.0_M42-RS485-Sond_K7
			Diverse Leiterbahn-Optimierungen

2025-05-03	M43	Rev.3.1 FFC-4-Layer: OP
			///TPS3890 Delay bei Kaltstart für Watchdog von 16s, als Idee skizziert
			Watchdog-Zeiten festgelegt: 1.6s, 3,6s, 11s, 20s
			Kompatibilität zu M42 bleibt erhalten

2025-07-08	M43	Rev.3.1 FFC-4-Layer: OP
			///U52 NE555-CMOS Delay bei Kaltstart für Watchdog bei jedem RESET von 1,6-12,7sec, als Idee skizziert
			///U53 Inverter CMOS, invertiert Ausgang NE555 , um eine Einschaltverzögerung zu bekommen.
			///RESET steuert U52, damit beginnt der Delay für den WD bei Kalt- und Warmstart.

2025-08-14	M43	
			Delay bei Kaltstart für Watchdog: U52 kein NE555 und kein TPS3890 sondern ein S-80929CNMC 
			( HIGH-PRECISION VOLTAGE DETECTOR WITH DELAY CIRCUIT (EXTERNAL DELAY TIME SETTING))
			steuert EN_WD am HW-WD an und verzögert dessen Aktivierung, wenn TR_U50 wieder High wurde, bzw bei KaltStart mit C106=3µ3 für 18sec
			U53 Buffer-Inv für LED D123/R175 auf EN_WD  , On wenn EN_WD=Low
			U54 Buffer-Inv für LED D123/R175 auf TR_U50 , On wenn TR_U50=Low
			
2025-10-05	M43 FFC für J3 mit PFS erzeugt. Abschluss als V3.1

2025-12-31	M43 FFC + PFS V3.1
			OP : Vertauschen von CAN1 und CAN2 am Teensy. Das ermöglicht die optionale Nutzung der Seriellen Schnittstelle RX1/TX1 alternativ zu CRX1/CTX1 für ein OTA-Modul
			Dabei steht CAN2 auf beiden DIN-Buchsen zur Verfügung, parallel zu RS485.

2026-05-17	M43 FFC + PFS V3.2 , OP & UP   --> V5
			U40 und R50 auf die OP. U40 mit Reset-Ausgang auf TR-U50, R50 als Pull-UP für RESET.
			Das hat den Vorteil, dass ein Spannungsabfall unter die Schwelle von U40, CPU und Interfaces abschaltet.

FFC only:
========================


2026-05-26	M43 FFC V3.2 , OP & UP   --> V5
			Modifikation der PE-Flächen-Abstände zu anderen Potentialen auf mindestens 1mm in allen Lagen
			OP in PFS noch nicht angepasst.
			LED1 Leiterbahn verlegt, da zu nah an PE
			Alternative zu ISO1050 ist der ADM3050E, der einen vertauschten Anschluss für TXD und RXD hat.
			Um für deren eventuelle Verwendung einen Leiterbahnpatch zu ermöglichen, wurden die Endstücke der CAN1RX/CAN2TX vom inneren Layer 
			IN1 auf B-Layer per Via gelegt. Siehe B-Silk-Markierungen "X" bei "*3" unter der Teensy CPU. Das gleiche ist am ISO1050 für beide
			Busse möglich.
			PE-Optimierung im DIN-Anschluss-Feld

2026-05-31	M43 FFC V3.2 , OP & UP   --> V5
			Komplett-Modifikation Potential-Abstände OP und UP in Version FFC
			J8/J21, J36/J34 neu geteilt und auf mehr Abstand GNDD zu GND_CAN
			J25/J24 geteilt und zu J33/J47 auf mehr Abstand für GNDD und GNDA
			PE und LED1,2,3 Leitungsführung optimiert. Ub_CAN geändert.
			RV4, RV5, RV6 Isolierte GND_*-Potentiale mit 46VAC gegen PE abgesichert
			CANRX/CANTX Leitungen getauscht um PROG- und ONOFF-Switch unter die CPU verlagern zu können, damit Abstand von GNDD-bezogenen
			Potentialen zu PE und 24V-Input vergrößert werden kann.
			Mindest-Abstand überall mindestens 1,1mm.

(2026-07-25)	M43 FFC V3.2 , OP        --> V5
			U55 SOIC-8 Alternativer Foodprint zu DIL-8 von U9, z.B. auch für FM-25er SPI-FM-RAM
			CS für U55 kann durch auftrennen auch separat angesteuert werden (Fädeldraht von anderem ungenutzten CS-Signal).
			Damit waren zwei Speicherbausteine ansprechbar, einer in DIL-8 und ein zweiter mit SOIC-8.

2026-07-25	M44 FFC V3.3 , OP & UP   --> V5
			OP: U55 SOIC-8 Alternativer Foodprint zu DIL-8 von U9, z.B. auch für FM-25er SPI-FM-RAM
			    CS für U55 kann durch auftrennen auch separat angesteuert werden (Fädeldraht von anderem ungenutzten CS-Signal).
			    Damit waren zwei Speicherbausteine ansprechbar, einer in DIL-8 und ein zweiter mit SOIC-8.
			    R172 versetzt, war zu dicht an J3
			UP: Output-LED intern angesteuert. Es entfallen die Bauteile R102-R109, D106-D113, R110-R117, D114-D121
			OP: R173 und D52, beide optional sind entfallen, da eine Kombination mit einer UP V4 nicht mehr möglich ist, wegen der
			    Verschiebung der Potentiale an den Verbindungen zwischen OP und UP.
20260904	M44 PFS V3.3, OP + UP    --> V5 
			Variante PFS bereitgestellt.
