# Projekt HAARP
## Technische Informatik Projekt
Programm für eine **SPS** geschrieben mit **TwinCat3.**
Dies ist eine Zentralsteuerung für eine Wohngemeinschaft welcher mehrere Gebäude angehören.

### Die Steuerung ist zuständig um:
1. Im normal Betrieb Daten der Häuser zu empfangen
2. Rückmeldung an die Häuser zu geben.
3. Informationen in der Visualisation darstellen

Die Kommunikation der Zentrale zu den Häusern findet über Ethercat statt.

### Es gibt zwei Betriebsarten:
1. Automatik
2. Simulation

Im Automatik Betrieb werden die Daten der Häuser bearbeiten.
Im Simulationsbetrieb werden auch die Daten des Simulationshaus berechnet.

### Wetterdaten die berechnet werden:
1. Sonne als Real
2. Regen als Bool
3. Wind als Real

und mehr...