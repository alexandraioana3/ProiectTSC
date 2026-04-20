==================================================
 PROIECT TSC 2026 - InkTime Smartwatch
==================================================
DINU ALEXANDRA - IOANA 332CC


1. DIAGRAMA BLOC

se afla in Diagrama-bloc.png

--------------------------------------------------
2. BILL OF MATERIALS (BOM)
--------------------------------------------------

Componenta      : MCU
Model           : Nordic nRF52840
Cod JLC         : C227306
Datasheet       : https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.7.pdf

Componenta      : Charger
Model           : TI BQ25180
Cod JLC         : C5162464
Datasheet       : https://www.ti.com/lit/ds/symlink/bq25180.pdf

Componenta      : Fuel Gauge
Model           : Maxim MAX17048
Cod JLC         : C224357
Datasheet       : https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf

Componenta      : Accelerometer
Model           : Bosch BMA421
Cod JLC         : C281864
Datasheet       : https://www.mouser.com/datasheet/2/783/bst-bma421-ds000-1509549.pdf

Componenta      : Haptic Driver
Model           : TI DRV2605L
Cod JLC         : C137152
Datasheet       : https://www.ti.com/lit/ds/symlink/drv2605l.pdf

Componenta      : Regulator
Model           : Richtek RT6160
Cod JLC         : C2892336
Datasheet       : https://www.richtek.com/assets/product_pdf/RT6160A/DS6160A-02.pdf


--------------------------------------------------
3. DESCRIERE FUNCTIONALITATE HARDWARE
--------------------------------------------------

[Sistemul de Putere]

- Incarcarea bateriei Li-Po se face prin BQ25180
- Monitorizare curent si temperatura integrata
- RT6160 stabilizeaza tensiunea la 3.3V
- Functionare posibila pana la ~2.8V baterie

[Senzori si Interfete]

- MAX17048:
  * Citire procent baterie prin I2C
  * Fara rezistenta externa de masura

- BMA421:
  * Detectie miscare
  * Step counting
  * Tilt-to-wake

- DRV2605L:
  * Control motor vibratii (ERM/LRA)
  * Efecte haptice integrate

- E-Paper Display:
  * Consum zero in stare statica
  * Control prin SPI


--------------------------------------------------
4. ALOCARE PINI nRF52840
--------------------------------------------------



USB D+ / D-  | USB Data     | USB-C (J5)
             |              | Pini dedicati pentru comunicatia USB nativa


P0.06        | I2C SDA      | Senzori / PMIC
             |              | Magistrala date (BMA421, MAX17048, BQ25180)

P0.07        | I2C SCL      | Senzori / PMIC
             |              | Magistrala clock pentru comunicatia I2C


P0.02        | SPI SCK      | E-Paper Display
             |              | Semnal de ceas pentru magistrala SPI

P0.03        | SPI MOSI     | E-Paper Display
             |              | Transmisie date imagine catre ecran

P0.05        | SPI CS       | E-Paper Display
             |              | Chip Select - activeaza comunicarea cu ecranul

P0.15        | EPD DC       | E-Paper Display
             |              | Data/Command control pentru driverul de ecran

P0.16        | EPD RST      | E-Paper Display
             |              | Reset hardware pentru panoul E-Paper

P0.17        | EPD BUSY     | E-Paper Display
             |              | Feedback de la ecran (procesare imagine)


P0.12        | HAPTIC EN    | DRV2605L
             |              | Activare / Trigger pentru driverul haptic

P0.11        | PMIC INT     | BQ25180
             |              | Intrerupere de la charger

P0.10        | ALERT        | MAX17048
             |              | Alerta baterie (ex: nivel critic)


P0.08        | IMU INT1     | BMA421
             |              | Intrerupere accelerometru (tap/tilt)

P1.08        | IMU INT2     | BMA421
             |              | Intrerupere secundara (low power)


P1.06        | BUTTON       | SW_UP
             |              | Buton navigare sus

P1.04        | BUTTON       | SW_DN
             |              | Buton navigare jos


P0.18        | RESET        | Debug Port
             |              | Reset hardware extern


SWDIO        | Debug Data   | TC2030 (J2)
             |              | Linie date programare

SWDCLK       | Debug Clock  | TC2030 (J2)
             |              | Linie ceas programare


P0.00 / 0.01 | XL1 / XL2    | Cristal 32kHz
             |              | Oscilator low power / RTC

XC1 / XC2    | HF Crystal   | Cristal 32MHz
             |              | Frecventa principala + BLE


VBUS         | Power Sense  | USB VBUS
             |              | Detectie 5V USB

VDDH         | High Voltage | 3.3V VCC
             |              | Alimentare principala


--------------------------------------------------
5. DESIGN LOG SI DECIZII
--------------------------------------------------

[Status: Finalizare Partiala]

- Overlap Errors (#16):
  * 65 erori acceptate
  * Cauza: densitate mare aQFN + PCB 2 straturi
  * Solutie viitoare: PCB 4 straturi

- Airwires (#15):
  * 16 conexiuni ramase
  * Nu afecteaza functionarea de baza
  * Evitare scurtcircuite in zone critice

- Antena RF:
  * Ground stitching aplicat
  * Keep-out respectat sub antena

- Vizualizare 3D:
  * PCB validat mecanic (1.0mm grosime)
  * Fara modele 3D componente


--------------------------------------------------
6. FISIERE DE FABRICATIE
--------------------------------------------------

Folder: /Manufacturing

Contine:

- Gerber Files (pentru productie PCB)
- BOM (lista componente)
- Pick and Place (asamblare SMT)

==================================================