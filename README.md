==================================================
 PROIECT TSC 2026 - InkTime Smartwatch
==================================================
DINU ALEXANDRA - IOANA 332CC


1. DIAGRAMA BLOC

USB-C (J5)
   │
   ├── VBUS 5V
   ▼
BQ25180 Charger
   │
   ├── VBAT ───────► Li-Po Battery
   │                  │
   │                  └──► MAX17048 Fuel Gauge
   │
   └── SYS (3.7V - 4.2V)
          │
          ▼
     RT6160 Buck-Boost
          │
          └── VCC 3.3V
                │
                ▼
         nRF52840 MCU
            │   │   │   │
            │   │   │   ├── GPIO ─► Buttons / LED
            │   │   │
            │   │   ├── PWM/I2C ─► DRV2605L (Haptic)
            │   │
            │   ├── I2C ─► BMA421 (Accelerometer)
            │   ├── I2C ─► MAX17048
            │
            └── SPI ─► E-Paper Display


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

P0.31  → USB D+        → USB-C (J5)
P0.30  → USB D-        → USB-C (J5)

P0.26  → I2C SDA       → Senzori
P0.27  → I2C SCL       → Senzori

P0.02  → SPI SCK       → Display
P0.03  → SPI MOSI      → Display

P0.12  → PWM/TRIG      → DRV2605L
P0.11  → INT           → BQ25180


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