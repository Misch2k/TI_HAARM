
# Projekt HAARP
## Technische Informatik Projekt
Programm für eine **SPS** geschrieben mit **TwinCat3.**

# Ausgangslage und Problemstellung

Das vorligende Projekt beschäftigt sich mit der messdatenerfassung aus verschiedenen Gebäuden un deren Auswertung.  

Der Auftraggeber wünscht folgende Automatisierungstechnische Lösung für Messdatenerfassung und Auswertung:  
- Erfassung aller relevanten hauseigenen Daten über TCP/IP
- Erkennen, dass in den Gebäuden Energie bezogen wird(Erfassung der eingeschalteten Lampen)
- Laufzeit der eingeschalteten Lampen (Muss nicht gemacht werden)
- Darstellung der integrierten Gebäude (schematisch)
- Übergabe von simulierten Werten (Licht, Wind, Regen(bool))
- Alarmbitüberwachung und Weiterleitung an alle Gebäude.
- Implementierung von zentralen Wetterdaten, wie Aussenlicht, Regen, Windgeschweindigkeit.
- Erfassung der Anzahl Personen, welche auf Besuch kommen (Zähler der Anz. Klingel-Tast-Impulse)
- Für alle kontinuierlichen Signale is eine Kurvendarstellung zu implementieren.  

Alle geforderten Funktionen sind übersichtlich zu implementieren. Berücksichtigen Sie, dass wir einerseits direkte Fühler am Haus haben(intern), und anderersetits Wetterdaten über eine zentrale einheit vorgegeben bekommen (extern). Alle implementierte Funktionen sind so zu gestalten, dass man diese zwischen Intern und Extern umschalten kann.  
In einem weiteren SChritt sind die einzelnen Werte der beteiligten Gebäude übersichtlich zu visualisieren. Alle relevanten Daten müssen über den PC dargestellt werden.  
Das notwenidge Projektmanagement aufzubauen.

# Inhalt dieses Repositori

Dieses Repo beinhaltet die Zentrale Steuerung / Datenerfassung / Wettersimulation der Gebäude.  

## Bausteine

### Alarm Baustein
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Alarm.PNG?raw=true)

#### Beschreibung
Wird in einem Haus eingebrochen, wird ein Globaler Alarm ausgelöst und an alle Häuser gesendet.  
Wenn kein Einbruch Signal mehr vorhanden ist, bleibt der Globale Alarm für die **Alarm_OFF_Time** bestehen.
Wenn **Simulations-Modus** eingeschalten wird, wird auch das **Simulations_Haus** berücksichtigt.
#### Input
- Alarm Haus 1 - Haus 4 (Einmal Netzwerk und einmal von Visu)
- Alarm Simulationshaus
- Simulations-Modus
- Zeit bis Alarm abgeschalten wird (Abschaltverzögerung)
- Reset vom Globalen Alarm (Abschaltverzögerung überbrücken)

#### Output
- Globaler Alarm  

### Alarm Bild
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Alarm_Bild.PNG?raw=true)
#### Beschreibung
Weil der Alarm in der Visu als Bild dargestellt wird, haben wir einen Baustein erstellt, welcher das Passende Bild (Als STRING) liefert.
#### Input
- Globaler Alarm
- Alarm aller Häuser + Simulations Haus
- Bild wenn Alarm
- Bild wenn kein Alarm
#### Output
- Bild für das jeweilige Haus
### Besucher
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Besucher.PNG?raw=true)
#### Beschreibung
Gesamtzahl der Besucher aller Häuser werden zusammengezählt. 
Die Besucher können durch die Visu (Manuell) und durch den Automatikbetrieb (um 00:00 Uhr, Täglich)  zurückgesetzt werden.
#### Input
- Besucher Aller Häuser (INT)
- Simulations-Modus
- Reset Visu und Automatik
#### Output
- Gesamtzahl der Besucher
- Globaler Reset aller Besucher
### Heizung
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Heizung.PNG?raw=true)
#### Beschreibung
Jedes Haus kann eine Temperatur wünschen, die Höchste Temperatur wird dann geliefert.
Bereich der Temperatur: 15°C bis 30°C
Wird eine Änderung registriert, dauert es die **Heizung_Reaktion** Zeit lange, bis die Temperatur erreicht ist.
#### Input
- Gewünschte Temperatur der Einzelnen Häuser
- Simulations-Modus
- Zeit bis Temperatur erreicht ist
#### Output
- Gelieferte Temperatur
### Regenmacher
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Regenmacher.PNG?raw=true)
#### Beschreibung
Für die Simulation von Regen. 
Generiert:
- Ob es heute Regnet
- Wenn ja:
	- Es regnet jetzt.
	- Wann der Regen anfängt
	- Wie lange er Dauert
	- Bis wann es Regnet
Informationen zur Regendauer werden in der Wetterstation Visu dargestellt.
#### Input
- Soll der Regen generiert werden, Freigabe-bit (Hier Wahrscheinlichkeit nicht 0)
- Aktuelle Zeit
- Regenwahrscheinlichkeit
- Simulation (Nicht verwendet)
#### Output
- Es Regnet
- Es Regnet heute
- Regen beginn Stunde, Minute
- Regen dauer (in Minuten)
- Regen ende Stunde, Minute
### Sonne
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Sonne.PNG?raw=true)
#### Beschreibung
Im Automatik-Modus wird anhand der Tageszeit eine Sinuskurve generiert, welche die aktuelle Helligkeit darstellt.
Ist der Automatik-Modus ausgeschalten, wird der Wert, welcher in der Visu (Slider) eingestellt ist, verwendet.
Zudem werden die Informationen in einem Array gespeichert und in der Visu(Visu_Settings) angezeigt.
#### Input
- Tagesfortschritt
- Automatik Modus
- Wert von Slider (Visu)
- Sekundentakt
- Minimum Licht
- Maximum Licht
#### Output
- Aktuelle Helligkeit (Lumen)
- Trigger Signal um den Wert im Array zu speichern
### Uhr
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Uhr.PNG?raw=true)
#### Beschreibung
Umwandlung von Zeit in Grad (für die Analoge Uhr in der Visu)
#### Input
- Stunde, Minute, Sekunde
#### Output
- Rotation von: Stundenzeiger, Minutenzeiger, Sekundenzeiger)
### Bild Wechsler
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Visu_BildToggle.PNG?raw=true)
#### Beschreibung
Einfacher Selektor--Baustein für String.(ev. hätte auch ein **SEL** Baustein funktioniert.
#### Input
- Licht brennt
- Bild wenn Licht brennt
- Bild wenn Licht nicht brennt
#### Output
- Aktuelles Bild
### Wetterbericht
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Wetterbericht.PNG?raw=true)
#### Beschreibung
Output ist das Richtige Bild (ICON) für die Wetterbericht Visu.
#### Input
- Regen Aktuell
- Wind Aktuell
- Regen Heute
#### Output
- Aktuelles Wetter Bild
- Vorhersage Bild
### Wind
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/FB_Wind.PNG?raw=true)
#### Beschreibung
Simuliert im Automatik-Modus den Aktuellen Wind.
Ist der Automatik-Modus ausgeschalten, wird der Wert vom Slider(Visu) genommen.
Die Aktuelle Windstärke wird in einem Array gespeichert und in der Visu angezeigt.
#### Input
- Automatik
- Slider
- Wind Maximum
- Sekundentakt
#### Output
- Aktueller wind
- Trigger zum Speichern des Wertes
### Zeit
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/GET_TIME.PNG?raw=true)
#### Beschreibung
Liest die aktuelle Systemzeit aus.
Die Ausgegebene Zeit kann mit einem Manipulator beschleunigt werden.
#### Input
- ZeitManipulation
#### Output
- Tagesfortschritt
- Zeit
- Anzahl Tage seit Start der SPS
### INT R Trig
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/INT_TRIG.PNG?raw=true)
#### Beschreibung
R_Trig für INT Werte. 
## Visu
### Haupt Visu
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/Visu_MainVisu.PNG?raw=true)
#### Beschreibung
Haupt Visu.
Ist zusammengesetzt aus allen Visus von diesem Projekt. Diese sind jeweils als Frame eingebunden.
Links sind die Informationen über die Einzelnen Häuser zu sehen.
In der Mitte oben, ist der Wetterbericht für heute.
Die Uhr zeigt die Aktuelle Uhrzeit an
Unten sind allgemeine Informationen  zu Finden.
Über den Button **Settings** können die Einstellungen geöffnet werden. Dort sind die Licht Emissionen und Wind Daten der letzten Zeit zu sehen. Es kann ebenfalls der Globale Alarm und die Besucher manuell zurückgesetzt werden.

### Einstellungen
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/Visu_Settings.PNG?raw=true)
#### Beschreibung
Anzeige der Lichtemissionen und Wind der letzten Zeit.
Besucher und Alarm können Manuell zurückgesetzt werden.
### Haus n
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/Visu_Haus_n.PNG?raw=true)
#### Beschreibung
Visualisierung eines Hauses.
Sonne Rotiert und ändert das Bild (Tag, Nacht)
Hinterrund ändert Helligkeit entsprechend der Lichtemission.
Schloss neben dem Haus wird rot, wenn es ein Alarm Signal Sendet
Die Wolke zeigt an, ob es zur Zeit regnet.
Licht im Haus wechselt die Farbe (Schwarz / Gelb) wenn mindestens 1 Lampe brennt.

### Visu Uhr
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/Visu_Uhr.PNG?raw=true)
#### Beschreibung
Eigene Visu für die Analoge Uhr.
### Visu Wetterbericht
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/Visu_Wetterbericht.PNG?raw=true)
#### Beschreibung
Zeigt das Wetter vom Heutigen Tag an. Immer die Vorhersage und das Aktuelle Wetter.
### Visu Zentrale
![FB_Alarm](https://github.com/Misch2k/TI_HAARM/blob/master/HAARP3/BausteineBilder/Visu_Zentrale.PNG?raw=true)
#### Beschreibung
Hier werden die Output Daten der Zentrale dargestellt.

## Allgemein
Alle Bausteine sind in Strukturiertem Text geschrieben.
Doku ist im Share zu Finden.

## Netzwerk
Netzvariablen sind mit Ethercat realisiert.

# Over and Out